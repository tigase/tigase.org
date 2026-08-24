---
title: Maven repositories URL changes
tags:
  - anonucement
date: "2017-02-20T22:40:32.169Z"
description: We are making it easier to get in touch, get support and track our projects.
categories:
  - blog
header:
  overlay_image: /assets/posts/tiger_cleanup_darker.png
---

For quite some time we were using the basic mean to provide access to our Maven artifacts - simply serving them as a directory view. Recently we made some changes in that area to help with the maintenance and also provide a single access point to our repositories.

This resulted in deploying Apache Archiva under new URL: [http://maven-repo.tigase.org/](http://maven-repo.tigase.org/), from where you can access both final and snapshot repositories.

Please update your Maven configuration accordingly to:

- [http://maven-repo.tigase.org/repository/release](http://maven-repo.tigase.org/repository/release)
- [http://maven-repo.tigase.org/repository/snapshot](http://maven-repo.tigase.org/repository/snapshot)

```xml
<repositories>
    <repository>
        <id>tigase</id>
        <name>Tigase repository</name>
        <url>http://maven-repo.tigase.org/repository/release</url>
    </repository>
    <repository>
        <id>tigase-snapshot</id>
        <name>Tigase repository</name>
        <url>http://maven-repo.tigase.org/repository/snapshot</url>
        <snapshots>
            <enabled>true</enabled>
        </snapshots>
    </repository>
</repositories>
```
