---
title: Tigase-XMPP Server 8.1.0 General Availability released
tags:
  - server
  - release
date: "2020-07-24"
description: Tigase team is proud to present you new 8.1.0 General Availability release of Tigase XMPP Server packed with many new features and improvements.
categories:
  - blog
header:
  overlay_image: /assets/posts/tigase-adminui-1.png
---
Tigase team is proud to present you new 8.1.0 General Availability release of Tigase XMPP Server packed with many new features and improvements.

Next General Availability version of **Tigase XMPP Server 8.1.0** has been released - and it’s packed with features and improvements! Read on for the details or scroll all the day down for download links.

If this is your first time with the Server be sure to check out [Quick Start guide](https://docs.tigase.net/tigase-server/8.1.0/Administration_Guide/html/#QuickStart)

## Major Changes

### More XMPP extensions

Following XMPP guidelines specified in *Compliance Suites* a number of extensions was included in this release:

- **XEP-0398**: User Avatar to vCard-Based Avatars Conversion ([server-1017](https://projects.tigase.net/issue/server-1017))
- **XEP-0156**: Discovering Alternative XMPP Connection Methods - Tigase already supported handling DNS queries and standardised our `webservice` to XEP-0156 ([http-76](https://projects.tigase.net/issue/http-76))
- **XEP-0410**: MUC Self-Ping (Schrödinger’s Chat) ([muc-122](https://projects.tigase.net/issue/http-76))
- **XEP-0153**: vCard-Based Avatars - added support for setting **vCard avatar for MUC rooms** ([muc-112](https://projects.tigase.net/issue/http-76))
- **XEP-0411**: Bookmarks Conversion ([pubsub-79](https://projects.tigase.net/issue/http-76))
- **XEP-0157**: Contact Addresses for XMPP Services ([server-995](https://projects.tigase.net/issue/server-995)) that can be configured on per VHost basis ([server-1015](https://projects.tigase.net/issue/server-1015))

### Improved connectivity with other servers

`SASL-EXTERNAL` mechanism defined in [XEP-0178: Best Practices for Use of SASL EXTERNAL with Certificates](https://xmpp.org/extensions/xep-0178.html) was added for server-to-server (federated, s2s) connections greatly improving compliance with XMPP network. It’s possible to use both SASL-EXTERNAL and Diallback depending on support in other servers.

![sasl-external]({{ site.url }}{{ site.baseurl }}/assets/posts/sasl-external.png)

### Better security & privacy

When it comes to connectivity, Tigase XMPP Server sported **Hardened Mode** that adjusted networking security settings (supported protocols, cipher suites and keys' length where applicable). We decided include 3-level configuration option for **Hardened Mode** (roughly following *Mozilla’s SSL Configuration Generator*): `relaxed`, `secure` (default) and `strict` and to further eliminate cipher suites that are currently considered insecure.

We strive to provide best possible defaults, so right after installation you will get **A** score on xmpp.net (with proper certificate):
<a href='https://xmpp.net/result.php?domain=tigase.im&amp;type=client'><img src='https://xmpp.net/badge.php?domain=tigase.im' alt='xmpp.net score' /></a>
<a href='https://xmpp.net/result.php?domain=tigase.im&amp;type=server'><img src='https://xmpp.net/badge.php?domain=tigase.im' alt='xmpp.net score' /></a>
![xmpp-net-score]({{ site.url }}{{ site.baseurl }}/assets/posts/xmpp-net-score.png)

What's more - it's very easy to configure desired level on per-domain (per-VHost) basis:
![tigase-hardened-mode-configuration]({{ site.url }}{{ site.baseurl }}/assets/posts/tigase-hardened-mode-configuration.png)

We also enabled by default our anti-spam plugin and because we like all-things-extensible we created a guide how to create your own pluggable filters for anti-spam-plugin.

### Multiple domains (VHosts) support is even better

It was always quite easy to configure and serve multiple domains in Tigase XMPP Server. In this release we made it even better! First of all - we included `Default` VHost item, which allows configuring global defaults for the installation on the fly without having to change configuration files and restart the instance.

![tigase-vhost-configuration-1]({{ site.url }}{{ site.baseurl }}/assets/posts/tigase-vhost-configuration-1.png)
![tigase-vhost-configuration-2]({{ site.url }}{{ site.baseurl }}/assets/posts/tigase-vhost-configuration-2.png)

Internally, we introduced *VHost Extensions* - a mechanism that allows easy addition of configurable options that can be set on per-domain basis.

On top of that we reworked how SSL certificates are handled (especially wildcard ones) and now they are loaded and assigned to correct domain automatically - no need to configure *star*-certificates manually anymore.

![wildcard-certificates]({{ site.url }}{{ site.baseurl }}/assets/posts/wildcard-certificates.png)

### Mobile First

Notifications send to mobile applications via Apple’s and Google’s push servers using **Tigase’s PUSH component** are now encrypted ([#push-25](https://projects.tigase.net/issue/push-25)), requires compatible clients)

MUC component now allows users to register permanent nickname, which makes it possible to receive PUSH notifications even if our client disconnects and is offline ([#muc-115](https://projects.tigase.net/issue/muc-115))

![muc-permanent-member-registration]({{ site.url }}{{ site.baseurl }}/assets/posts/muc-permanent-member-registration.png)

### Installation & management

The (web) installer was simplified making setting up and configuring Tigase even easier ([#http-78](https://projects.tigase.net/issue/http-78)) - now it’s only needed to select desired database, provide it’s details and eventually adjust which components and plugins should be enabled or disabled, but we believe that provided defaults should work well in most of the cases.

![tigase-vhost-configuration-1]({{ site.url }}{{ site.baseurl }}/assets/posts/tigase-installer-1.png)
![tigase-vhost-configuration-2]({{ site.url }}{{ site.baseurl }}/assets/posts/tigase-installer-2.png)
![tigase-vhost-configuration-3]({{ site.url }}{{ site.baseurl }}/assets/posts/tigase-installer-3.png)

After the installation and startup, it’s possible to see basic instance state via web browser either opening `/server/` endpoint ([#server-1164](https://projects.tigase.net/issue/server-1164)), or local file from `logs/server-info.html`)
 
 ![tigase-status-page]({{ site.url }}{{ site.baseurl }}/assets/posts/tigase-status-page.png)
 
 Management the installation using Admin WebUI also received slight visual face-lift ([#http-90](https://projects.tigase.net/issue/http-90))
 
 ![tigase-adminui-1]({{ site.url }}{{ site.baseurl }}/assets/posts/tigase-adminui-1.png)
 ![tigase-adminui-2]({{ site.url }}{{ site.baseurl }}/assets/posts/tigase-adminui-2.png)

### Noteworthy

- Startup time was significantly reduced due to improvements of creating repository pools ([#server-1149](https://projects.tigase.net/issue/server-1149))
- Multi-thread, highly concurrent script execution was improved ([#server-1154](https://projects.tigase.net/issue/server-1154))
- StreamManagement was available, but in this version we decided to enabled it by default.
- More places offer support for [XEP-0059: Result Set Management](https://xmpp.org/extensions/xep-0059.html) - namely PubSub nodes discovery and `jabber:iq:serach`
- [Publishing Options](https://xmpp.org/extensions/xep-0060.html#publisher-publish-options) were added to PubSub ([#pubsub-75](https://projects.tigase.net/issue/pubsub-75))

### New Minor Features & Behavior Changes

- [server-918](https://projects.tigase.net/issue/server-918): AWS obtain public IP and/or DNS address of the EC2 instance
- [server-985](https://projects.tigase.net/issue/server-985): Add support for SCRAM-SHA-512(-PLUS)
- [spam-8](https://projects.tigase.net/issue/spam-8): Enable spam processor by default
- [server-1012](https://projects.tigase.net/issue/server-1012): UserDomainFilter.groovy fails to load
- [server-1014](https://projects.tigase.net/issue/server-1014): Can’t upgrade from 8.0.0GA to 8.1.0-SNAPSHOT
- [server-798](https://projects.tigase.net/issue/server-798): Limit number of messages that are stored in DB per user within a period of time
- [server-827](https://projects.tigase.net/issue/server-827): Seperate Component-based statistics
- [server-1026](https://projects.tigase.net/issue/server-1026): NPE: in JabberIqRegister/EmailConfirmationSender
- [pubsub-82](https://projects.tigase.net/issue/pubsub-82): NPE in RetrieveItemsModule
- [tigaseim-78](https://projects.tigase.net/issue/tigaseim-78): IPv6 connectivity issue
- [server-239](https://projects.tigase.net/issue/server-239): OSGi mode - exceptions in logs
- [server-1020](https://projects.tigase.net/issue/server-1020): Enable stream management by default
- [pubsub-83](https://projects.tigase.net/issue/pubsub-83): NPE in PublishItemModule
- [pubsub-81](https://projects.tigase.net/issue/pubsub-81): Exception during execution of event: tigase.pubsub.modules.PresenceCollectorModule.PresenceChangeEvent
- [server-1021](https://projects.tigase.net/issue/server-1021): NPE: Cannot update BruteForceLocker
- [server-826](https://projects.tigase.net/issue/server-826): UserRepository caches force synchronization even if caching is disabled
- [server-958](https://projects.tigase.net/issue/server-958): Add timeout for opened TCP connections
- [server-1029](https://projects.tigase.net/issue/server-1029): Read receipients are not copied via carbons
- [server-1015](https://projects.tigase.net/issue/server-1015): Allow configuring XEP-0157: Contact Addresses on per VHost basis
- [pubsub-65](https://projects.tigase.net/issue/pubsub-65): RSM and jabber:search for pubsub discovery
- [server-1030](https://projects.tigase.net/issue/server-1030): NPE in VCardTemp when processing initial presence
- [http-72](https://projects.tigase.net/issue/http-72): Change Content-Disposition from attachment to inline
- [server-1045](https://projects.tigase.net/issue/server-1045): NPE in DiscoExtensionsForm
- [server-1048](https://projects.tigase.net/issue/server-1048): Update parent pom and information about suggested JDK
- [push-23](https://projects.tigase.net/issue/push-23): \[JDK12\] Can’t establish encrypted connection with Push/FCM
- [server-978](https://projects.tigase.net/issue/server-978): Improve VHost configuration / extending
- [server-1068](https://projects.tigase.net/issue/server-1068): Improve LogFormat readability (and maybe performance)
- [server-1070](https://projects.tigase.net/issue/server-1070): Improve privacy list loggging
- [server-1071](https://projects.tigase.net/issue/server-1071): NPE in IOService.accept
- [server-710](https://projects.tigase.net/issue/server-710): Registration improvements
- [pubsub-79](https://projects.tigase.net/issue/pubsub-79): XEP-0411: Bookmarks Conversion
- [pubsub-75](https://projects.tigase.net/issue/pubsub-75): Add support for Publishing Options
- [server-1017](https://projects.tigase.net/issue/server-1017): XEP-0398: User Avatar to vCard-Based Avatars Conversion
- [server-994](https://projects.tigase.net/issue/server-994): Add server support for Entity Capabilities: Stream Feature
- [server-995](https://projects.tigase.net/issue/server-995): XEP-0157: Contact Addresses for XMPP Services
- [http-76](https://projects.tigase.net/issue/http-76): Standardise DNS webservice to XEP-0156
- [server-1109](https://projects.tigase.net/issue/server-1109): Add recommended JDK version to documentation
- [push-28](https://projects.tigase.net/issue/push-28): Non-tigase notifications should use high priority (APNS)
- [server-1114](https://projects.tigase.net/issue/server-1114): Can’t register on sure.im with StorkIM
- [server-1005](https://projects.tigase.net/issue/server-1005): Flatten schema to match versioning document
- [server-1116](https://projects.tigase.net/issue/server-1116): account\_status is not checked
- [server-1074](https://projects.tigase.net/issue/server-1074): Hardened Mode improvements
- [server-1125](https://projects.tigase.net/issue/server-1125): StatsDumper.groovy doesn’t work in documentation in 8.x
- [http-85](https://projects.tigase.net/issue/http-85): Pasword resset doesn’t work
- [server-1128](https://projects.tigase.net/issue/server-1128): Possible vulnerability in XML parser
- [server-1130](https://projects.tigase.net/issue/server-1130): NPE i JabberIqAuth
- [http-84](https://projects.tigase.net/issue/http-84): Configurable `resetPassword` endpoint hostname
- [server-1129](https://projects.tigase.net/issue/server-1129): BOSH timeouts on GET requests
- [prv-436](https://projects.tigase.net/issue/prv-436): Conversations compliance - contact developers
- [server-1100](https://projects.tigase.net/issue/server-1100): CAAS and WS testers fail to connect to wss://tigase.im:5291
- [server-1047](https://projects.tigase.net/issue/server-1047): Add SASL-EXTERNAL on s2s conections
- [server-1103](https://projects.tigase.net/issue/server-1103): High priority PUSH notifications are sent for all messages
- [pubsub-93](https://projects.tigase.net/issue/pubsub-93): NPE in CapsChangeEvent
- [server-1137](https://projects.tigase.net/issue/server-1137): Don’t require setting JAVA\_HOME to start server
- [server-1136](https://projects.tigase.net/issue/server-1136): upgrade-schema --help not available
- [utils-19](https://projects.tigase.net/issue/utils-19): tigase-utils doesn’t compile with JDK12
- [server-1138](https://projects.tigase.net/issue/server-1138): Schema files are not sorted correctly during loading
- [pubsub-98](https://projects.tigase.net/issue/pubsub-98): Resources with emoji chars are causing issues with MySQL backend
- [server-1110](https://projects.tigase.net/issue/server-1110): Disabling TLS in VHost configuration doesn’t work
- [server-1078](https://projects.tigase.net/issue/server-1078): Don’t send root CA certificate in chain
- [server-1113](https://projects.tigase.net/issue/server-1113): Don’t advertise SASL-EXTERNAL if own certificate is not valid
- [http-78](https://projects.tigase.net/issue/http-78): Simplify installer
- [server-1133](https://projects.tigase.net/issue/server-1133): Not able to connect via S2S to server with incorrect SSL certificate
- [serverdistribution-2](https://projects.tigase.net/issue/serverdistribution-2): MUC upgrade not linked correctly in global tigase guide
- [server-1149](https://projects.tigase.net/issue/server-1149): Reduce startup time with a lot of database connections
- [server-1148](https://projects.tigase.net/issue/server-1148): "ERROR! Component &lt;x&gt; schema version is not loaded in the database or it is old!" during shutdown
- [server-1153](https://projects.tigase.net/issue/server-1153): Refactor Credentials related `username` to `credentialId` to avoid confussion
- [servers-312](https://projects.tigase.net/issue/servers-312): No cluster connection to send a packet
- [server-1154](https://projects.tigase.net/issue/server-1154): Multi-thread script execution yields wrong results
- [servers-294](https://projects.tigase.net/issue/servers-294): Can’t connect from tigase.im to rsocks.net
- [server-1111](https://projects.tigase.net/issue/server-1111): Can’t establish s2s to upload.pouet.ovh
- [server-1143](https://projects.tigase.net/issue/server-1143): S2S connectivity issue with OpenFire when SASL external is used
- [servers-309](https://projects.tigase.net/issue/servers-309): Issue when connecting to xabber.org: not-authorized: self signed certificate
- [tigaseim-80](https://projects.tigase.net/issue/tigaseim-80): Siskin IM push server is not accessible
- [server-1080](https://projects.tigase.net/issue/server-1080): After updating certificate via ad-hoc/rest only main certificate is updated
- [http-88](https://projects.tigase.net/issue/http-88): Improve REST documentation
- [http-87](https://projects.tigase.net/issue/http-87): "request accept time exceeded" for every request when using `JavaStandaloneHttpServer`
- [server-1151](https://projects.tigase.net/issue/server-1151): BruteForceLockerExtension (and possibly others) settings are not correctly retrieved
- [http-89](https://projects.tigase.net/issue/http-89): Drop result/error packages received by HTTP-API if no connection present to write response to
- [pubsub-99](https://projects.tigase.net/issue/pubsub-99): Notifications are not sent for +notify from nodes with whitelist access mode
- [pubsub-79](https://projects.tigase.net/issue/pubsub-79): XEP-0411: Bookmarks Conversion
- [server-1157](https://projects.tigase.net/issue/server-1157): SCRAM-SHA512 not working
- [server-1159](https://projects.tigase.net/issue/server-1159): Improve handling establishing and terminating of the session
- [server-1152](https://projects.tigase.net/issue/server-1152): Cleanup warnings from JDBCMsgRepository
- [server-1112](https://projects.tigase.net/issue/server-1112): Fallback to diallback if SASL-EXTERNAL fails
- [servers-292](https://projects.tigase.net/issue/servers-292): S2S connectivity issues
- [acspubsub-19](https://projects.tigase.net/issue/acspubsub-19): REST execution fails on other nodes
- [server-1145](https://projects.tigase.net/issue/server-1145): Race condition during storing/loading of offline messages
- [http-90](https://projects.tigase.net/issue/http-90): Add direct links to most useful task in AdminUI main page
- [spam-10](https://projects.tigase.net/issue/spam-10): Add documentation for creation of a custom filter
- [server-1163](https://projects.tigase.net/issue/server-1163): Review and update `SASL Custom Mechanisms and Configuration` documentation
- [server-1164](https://projects.tigase.net/issue/server-1164): After-installation report - installation status
- [systems-76](https://projects.tigase.net/issue/systems-76): Fix issue with StackOverflow due to recursive call in TLSIO; improve debug log
- [server-1082](https://projects.tigase.net/issue/server-1082): Sec-WebSocket-Accept not calculated correctly
- [server-1083](https://projects.tigase.net/issue/server-1083): Messages sent to full jid are returned with error
- [push-25](https://projects.tigase.net/issue/push-25): Add support for sending encrypted PUSHes
- [server-1085](https://projects.tigase.net/issue/server-1085): Improve retrieval of values for all keys in a node in UserRepository
- [muc-115](https://projects.tigase.net/issue/muc-115): Add support for MUC and offline message delivery
- [muc-122](https://projects.tigase.net/issue/muc-122): XEP-0410: MUC Self-Ping (Schrödinger’s Chat)
- [muc-112](https://projects.tigase.net/issue/muc-112): Support for setting vCard avatar for room
- [http-83](https://projects.tigase.net/issue/http-83): Issue with multithreading access to HttpExchange instance
- [httpapijetty-3](https://projects.tigase.net/issue/httpapijetty-3): Support for HTTP/2
- [httpapijetty-6](https://projects.tigase.net/issue/httpapijetty-6): Update Jetty version

## Downloads

- [Binaries (distribution packages)](https://github.com/tigase/tigase-server/releases/tag/tigase-server-8.1.0):
  - [tigase-server-8.1.0-b10857-dist-max.tar.gz](https://github.com/tigase/tigase-server/releases/download/tigase-server-8.1.0/tigase-server-8.1.0-b10857-dist-max.tar.gz)
  - [tigase-server-8.1.0-b10857-dist-max.zip](https://github.com/tigase/tigase-server/releases/download/tigase-server-8.1.0/tigase-server-8.1.0-b10857-dist-max.zip)
- [Source Code](https://github.com/tigase/tigase-server/tree/tigase-server-8.1.0)
- [Maven Artifacts](https://maven-repo.tigase.net/#artifact/tigase/tigase-server/8.1.0)

## Test results

- [Tigase TTS-NG](https://build.tigase.net/tests-results/tts-ng/)
