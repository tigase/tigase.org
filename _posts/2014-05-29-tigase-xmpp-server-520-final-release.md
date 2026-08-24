---
title: Tigase XMPP Server 5.2.0 final release
tags:
  - server
  - server7
  - release
date: "2014-05-29T22:40:32.169Z"
description: Finally Tigase XMPP Server 5.2.0, dubbed *FTL - Faster Than Light*, has landed in it's destination (that is our servers) and it's available for download.
categories:
  - blog
header:
  overlay_image: /assets/posts/tiger_cleanup_darker.png
---

Finally Tigase XMPP Server 5.2.0, dubbed _FTL - Faster Than Light_, has landed in it's destination (that is our servers) and it's available for download. As always - binaries are available for download in the [**files section**](https://github.com/tigase/tigase-server/releases/tag/tigase-server-5.2.0) on the project tracking system. Sources are available in our code repository under the tag tigase-server-5.2.0 - [**tags/tigase-server-5.2.0**](https://github.com/tigase/tigase-server/tree/tigase-server-5.2.0). Maven artifacts have been deployed to our [maven repository](http://maven.tigase.org/tigase/tigase-server/5.2.0/). To put a cherry on top we also run automated tests - successful results are available in our [test page](http://build.tigase.org/~tigase/).

Major additions to the Tigase XMPP Server in this release:

- [HTTP API with REST](https://projects.tigase.org/projects/tigase-http-api) as the Tigase server component
- [Websocket](http://www.websocket.org) support as a Tigase server connection manager
- [Message Archiving Component](https://projects.tigase.org/projects/message-archiving) - **[XEP-0136](http://xmpp.org/extensions/xep-0136.html)**
- [Socks5 Proxy Component](https://projects.tigase.org/projects/socks5) with lots of interesting functions - **[RFC 1928](http://tools.ietf.org/html/rfc1928)**
- [STUN Server Component](https://projects.tigase.org/projects/stun) - [STUN](http://en.wikipedia.org/wiki/STUN)
- **[XEP-0198](http://xmpp.org/extensions/xep-0198.html)** - stream management extension with full support with ACK and Stream resumption. It is fresh but extensively tested already. If you do not have software to check it out or to test it, you can either use our [JaXMPP2 client library](https://projects.tigase.org/projects/jaxmpp2) which offers full support for the XEP or have a look at our [Android client](https://projects.tigase.org/projects/tigase-mobilemessenger) which can also make use of it.
- **[XEP-0280](http://xmpp.org/extensions/xep-0280.html)** message carbons extension. Similarly to the above XEP full support for it is also included in our client side library and mobile messenger.
- **[Tigase ACS](http://www.tigase.com/content/tigase-acs-advanced-clustering-strategy)** - which includes main clustering strategy as well as MUC and PubSub cluster-enabled components! Those are not open source but we make them available for free for development and testing.
- In addition to Derby, MySQL and Postresql Tigase now supports as well MS SQL Server.
- Top notch improvements in security (v. [Tigase XMPP Server - grade A software](http://www.tigase.org/content/tigase-xmpp-server-grade-software)).
- Inclusion of new PubSub 3.0.0 which features performance improvements due to reworked schema (v: [PubSub database schema conversion](https://projects.tigase.org/projects/tigase-pubsub/wiki/PubSub_database_schema_conversion)),
- Switched JDK compatibility to JDK7. Number of issues in Tigase server were related to bugs in JDK6 and patching them and creating workarounds was taking up resources. Therefore we decided to switch over to **JDK7** which is now required minimal version.

Other changes which have been made in this release:

- Optimization to sending presence.
- [Cluster auto-configuration](https://tigase.net/content/cl-conn-repo-class) with cluster mode active by default.
- OSGi support built-in to the core of the Tigase XMPP Server.
- [New API for SASL](https://tigase.net/content/sasl-custom-mechanisms-and-configuration) extensions and custom SASL mechanisms.
- API changes in plugin/processors, old API still available although deprecated, new API way faster and less resource consuming.
- Optimizations, lots of them, we expect this version to be significantly faster and use less resources.
- Some metrics names changed to avoid confusion whether this is a number of elements waiting in queue or number of packets already processed.
- Custom RosterAbstract implementation can be now plugged from a configuration file.
- See-other-host implementation improved, now the redirection can be sent to the client after successful login. Still most of XMPP clients do not support this part of RFC so it is optional.
- Improved multi-threading support, Timer replaced with ScheduledExecutorService where appropriate to avoid Timer single-thread bottleneck.
- Extended per-vhost configuration at run-time with new parameters: s2s secret, default filtering policy.
- Changed the way Tigase delivers messages to bare JID - now more common practice, delivers to all active resources.
- TLS required - full implementation, now Tigase won't allow user authentication without TLS if TLS required is set.
- The server whole active configuration is dumped to a text file which can be reviewed and/or copied to init.properties with custom values.
- Invisibility through the extension - **[XEP-0186](http://xmpp.org/extensions/xep-0186.html)**.
- Improved IPv6 support.
- [Clientaccesspolicy.xml](https://tigase.net/content/client-access-policy-file) support added.
- Lots of s2s improvements and bug fixes.
- Fixed a bug in service disco plugin and all plugins extending XMPPProcessorAbstract with a packet delivery for non-existen resource.
- Improved discovery of sessions with stale connections ([#1363](https://projects.tigase.org/issues/1363)).
- Implemented ability to set list of enabled SSL/TLS protocols using 'tls-enabled-protocols' system property.
- Implemented ability to disable client certificate request using 'sasl-external-disabled' system property.
- Fixed issues with forking messages with type other than chat.
- Fixed returning feature-not-implemented for requests without element ([#1418](https://projects.tigase.org/issues/1418)).
- Add number of active users to statistics.
- Added Content-Length header in response to OPTIONS request even if there is no content.
- Fixed sending requests for ack if queue for ack size is bigger than default value of 10.
- Inclusion of HTTP component to the installer ([#1384](https://projects.tigase.org/issues/1384)).
- Add heap memory setting, correct script path ([#52](https://projects.tigase.org/issues/52)).
- Increased security level ([#1628](https://projects.tigase.org/issues/1628)).
- Added implementation of optimizations for mobile devices - MobileV3.
- Execution of vhost ad-hoc scripts on all nodes in cluster mode ([#1587](https://projects.tigase.org/issues/1587)).
- Fix issue with garbled passwords during in-band registration/password change ([#826](https://projects.tigase.org/issues/826)).
- Fixed problem with service discovery incorrect display when a component changed description after it has been initialized.
- Improvements in XML DoS protection ([#1364](https://projects.tigase.org/issues/1364)).
- Automatic roster subscription (_presence_ and _jabber:iq:roster_ plugins has new optio: _auto-authorize_; setting it to true results in automatic, mutual “both” subscription state for both user and contact).

You can download the latest stable version from [distribution section](https://github.com/tigase/tigase-server/releases/tag/tigase-server-5.2.0).

Latest results of Tigase Testsuite are available for [Tigase Testsuite](https://build.tigase.net/tests-results/tts/) and [Tigase TTS-NG](https://build.tigase.net/tests-results/tts-ng/)
