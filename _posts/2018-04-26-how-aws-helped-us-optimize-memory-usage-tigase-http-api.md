---
title: How AWS helped us optimize memory usage in Tigase HTTP API
tags:
  - server
  - AWS
  - pro-tip
date: "2018-04-26T22:40:32.169Z"
description: How AWS helped us optimize memory usage in Tigase HTTP API.
categories:
  - blog
header:
  overlay_image: /assets/posts/tiger_cleanup_darker.png
---

# How AWS helped us optimize memory usage in Tigase HTTP API

## Moving to AWS

Recently we have moved our `xmpp.cloud` (formerly branded as `sure.im`) installation from one hosting provider where we used dedicated servers, to the [Amazon AWS](https://aws.amazon.com/) cloud based hosting. Benefits of this move are listed in [this article](https://tigase.net/article/sureim-has-moved-xmppcloud). During migration we chose to use the smallest possible AWS instances which would be good for hosting the `xmpp.cloud` and `t2.medium` services. Installation performed without any issues, and test runs showed that the systems were properly operating. Should we would need to scale our installation another cluster node could always be started.

## The Crashes

However, after some time we started to experience crashes on the new installation. The JVMs running [Tigase XMPP Server](https://tigase.tech/projects/tigase-server) were being terminated by Linux kernel due to memory allocation issues. In a typical situation, we would receive some `OutOfMemoryError`\`s from JVM notifying us that something is wrong and and that some adjustment would be needed for JVM memory settings. However this time, this was not the case. Instead JVM was being shutdown with a single entry being written to the `tigase-console.log` file along with a `hs_err_pid` file being created. The following entries were written to these files:

```
 There is insufficient memory for the Java Runtime Environment to continue.
 Native memory allocation (mmap) failed to map 12288 bytes for committing reserved memory.
 Possible reasons:
   The system is out of physical RAM or swap space
   In 32 bit mode, the process size limit was hit
 Possible solutions:
   Reduce memory load on the system
   Increase physical memory or swap space
   Check if swap backing store is full
   Use 64 bit Java on a 64 bit OS
   Decrease Java heap size (-Xmx/-Xms)
   Decrease number of Java threads
   Decrease Java thread stack sizes (-Xss)
   Set larger code cache with -XX:ReservedCodeCacheSize=
 This output file may be truncated or incomplete.

  Out of Memory Error (os_linux.cpp:2640), pid=3633, tid=0x00007fe49c4c4700

 JRE version: Java(TM) SE Runtime Environment (8.0_162-b12) (build 1.8.0_162-b12)
 Java VM: Java HotSpot(TM) 64-Bit Server VM (25.162-b12 mixed mode linux-amd64 compressed oops)
 Failed to write core dump. Core dumps have been disabled. To enable core dumping, try "ulimit -c unlimited" before starting Java again
```

**Native memory allocation** suggested that we were having an issue not with java `HEAP` size but rather an insufficient amount of free memory on our AWS instances. After verifying JVM memory settings, we found out that there was still plenty of free memory on the instance, so this issue should not occur. However, this issue was happening about once a day in frequency and it needed to be fixed. Since we are using Java version 8 at this point, the JVM memory is divided into:

- `HEAP`
- `MetaSpace`
- `DirectMemory`

We only had limits set for `HEAP` so the issue must have been with `MetaSpace` or `DirectMemory` growing without any limits, aside from the amount of free RAM memory at our AWS instance.
Additionally, Tigase XMPP Servers at `xmpp.cloud` installation were processing a lot of SPAM messages when crashes were happening. Due to that we suspected a leak in server-to-server (S2S) connection buffers as a lot of those connections was being created and many of them were saturated due to the high amount of incoming messages, most of which were SPAM. Knowing that, we decided to limit amount of memory allowed for `MetaSpace` and `DirectMemory` memory to 128MB.

## Isolating cause of the issue

The following day a 2nd crash occurred, as they happened usually between 24 and 32 hours from when the server starts up. This time before the crash happened we received quite a few `OutOfMemoryError` errors related to `MetaSpace`. Errors were not pointing to native memory being depleted. All of the `MetaSpace` and `OutOfMemoryError` errors where thrown from the code responsible for handling HTTP requests. However, they did not point to any particular location within this code. Since no tests were currently implemented to measure `MetaSpace` requirements for Tigase XMPP server installed at `xmpp.cloud`, we proceeded with a simple fix. `MetaSpace` was configured to use 256MB of space, and [Tigase HTTP API](https://tigase.tech/projects/tigase-http-api) was reconfigured to use [Jetty API Server](<https://www.eclipse.org/jetty/)>). Using Jetty instead of [Java Embedded Server](https://docs.oracle.com/javase/8/docs/jre/api/net/httpserver/spec/com/sun/net/httpserver/package-summary.html)reduces the amount of `DirectMemory` required for handling HTTPrequests.
Unfortunately after 27 hours Tigase XMPP Servers at the `xmpp.cloud` installation were again down and analysis still pointed to the Tigase HTTP API. This component was still the sole source for `OutOfMemoryError` errors related to `MetaSpace` which is allocated out of native memory (non-HEAP).

## Analysis of the memory usage

Having the issue isolated we decided to replicate it in a controlled environment, measure memory usage and take a few memory dumps for comparison of memory usage in different periods. Knowing that it is related directly to HTTP API we focused on testing that component. We began with testing REST API requests which we had started to use internally for one of the new features yet to be introduced for users of `xmpp.cloud`. During those tests `HEAP` memory was almost empty and `MetaSpace` usage increased slightly during the first part of the test. This behavior is expected due to Groovy scripts being compiled and loaded into memory. Later on `MetaSpace` usage was fluctuating but more or less stable. Only `CodeCache` space was changing due to JVM recompiling code to optimize its execution time. As direct calls to REST API were working fine, we had to focus on accessing the HTTP-API component using a web browser. This meant we needed to test the REST API, Admin UI, and other modules which are accessible from a web browser.
Just after executing the first tests using the browser, `MetaSpace` memory usage increased with each request and `MetaSpace` grew until the set limit was reached. Then `OutOfMemoryError` errors began to be thrown. Thanks to the memory dumps which were taken during those tests, we were able to identify which classes were using memory and which were allocated during each request. There we found a lot of classes containing `GStringTemplateScript` and `getTemplate` within their names. Each class was named as `GStringTemplateScript` with a number following, indicating multiple instances. As we are using `GStringTemplateScript` from Groovy to create HTML output for the web browsers, we surmised that somehow this template engine is leaking memory by generating a new classes for each request, and not unloading the older classes when they are not needed. Our memory leak had been found.

## Fixing the Issue

To fix the issue we started with code analysis to find usage patterns of `GStringTemplateEngine` which lead to a leak. In our case, the leak was caused by an automatic reload mechanism of GString templates. These are stored in files under \`scripts/rest/\` directory of Tigase XMPP Server installation directory and are loaded when needed. To make development and customization of those templates easier, they were reloading requested templates on each HTTP request. To make it work fast we kept a single `GStringTemplateEngine` instance (per servlet) which was handling every request. Previously, this mechanism saw a slow amount of `MetaSpace` memory increase, and we had not before experienced such a rapid acceleration of memory use. But this instance of `GStringTemplateEngine` had its own `ClassLoader` and was internally keeping a reference to each class created during parsing of the GString template. This led to an increased usage of `MetaSpace`, `OutOfMemoryError` errors and eventually to crashes. Having the real cause of this issue pinpointed, we reviewed usage of `GStringTemplateEngine` in our code and have changed them, to make sure that:

- We load all templates at once using single `GStringTemplateEngine`
  and cache generate templates. **No more automatic reloading of
  templates.**
- When manual reload of templates is initiated we release old instance
  of `GStringTemplateEngine` and parse templates using the new one.

This way we can still use `GStringTemplateEngine` and our GString templates while maintaining stable `MetaSpace` usage. As we have our template instances cached, responses for HTTP requests from the web browsers will now be faster. As for manual reloading, it will still generate new classes. However as we are releasing instance of `GStringTemplateEngine` and its internal `ClassLoader` we are releasing classes loaded by this `ClassLoader` to `MetaSpace` and making sure that this memory can cleaned by garbage collector. After extensive testing, we were able to confirm that memory usage is now stable.

## What about Amazon Web Services?

As previously mentioned, we recently moved to AWS from our old hosting provided and enabled new feature for our users. This feature is based on Tigase HTTP API and REST API, and uses both APIs extensively. We needed to expose this HTTP-based API to our other services and to do that we decided to use AWS's [Elastic Load Balancer](https://aws.amazon.com/elasticloadbalancing/) to be able to transparently forward those HTTP requests to each of Tigase's cluster nodes. This way it would automatically switch to different nodes if one of them is overloaded or offline.
mazon's Elastic Load Balancer executes a health check requests every few seconds to be able to detect if destination host is up and running fine. In our case it was testing REST API and was generating responses formatted in HTML just as a normal web browser would. This lead `GStringTemplateEngine` to be used for handling each request and each request creating new classes in the memory forcing JVM to use more and more memory until it ran out.

**Thanks to AWS for helping us optimize memory usage in Tigase HTTP
API**
