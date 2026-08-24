---
title: Tigase Server 7.0.0 Release
tags:
  - server
  - server7
  - release
date: "2015-02-23T22:40:32.169Z"
description: Tigase XMPP Server 7.0.0 has been released! This  release includes many major additions, many more minor additions, and a  few bug fixes. We're sure you will enjoy the software. Binaries are  available from the project's. Sources are available in our. Maven artifacts have been deployed to our. Test results are located on our.
categories:
  - blog
header:
  overlay_image: /assets/posts/tiger_cleanup_darker.png
---

**Tigase XMPP Server 7.0.0** has been released! This release includes many major additions, many more minor additions, and a few bug fixes. We're sure you will enjoy the software. Binaries are available from the project's. Sources are available in our. Maven artifacts have been deployed to our. Test results are located on our.

# Other Changes

- Enabling store_limit=0 in offline message storage; [#2726](https://projects.tigase.org/issues/2726).
- Patch [#2621](https://projects.tigase.org/issues/2621) LastActivity.
- Add schema upgrade code to handle new column to msg_history table (amp offline messages, adding msg_type column); [#2478](https://projects.tigase.org/issues/2478).
- Allow to inject a full Element instead of just a custom status; [#2530](https://projects.tigase.org/issues/2530).
- Switch to final version of docbkx-maven-plugin.
- BOSH SID logger improvements, filesize limitation and log rotation.
- Add mongodb support of [XEP-0013](http://xmpp.org/extensions/xep-0013.html): Flexible Offline Message Retrieval [#2478](https://projects.tigase.org/issues/2478).
- Task [#478](https://projects.tigase.org/issues/478): Simplified PubSub as a basic component API add: listeners scripts.
- [XEP-0013](http://xmpp.org/extensions/xep-0013.html): Flexible Offline Message Retrieval; [#2478](https://projects.tigase.org/issues/2478).
- Separate DataForms.
- Include Sure.IM to be part of Tigase XMPP Server installer as well [#2105](https://projects.tigase.org/issues/2105).
- Eventbus. Move source and timestamp from child element to event attribute. Remove everysecond event.
- Remove unused, old EventBus.
- Task [#815](https://projects.tigase.org/issues/815): Monitoring API.
- Issue [#2607](https://projects.tigase.org/issues/2607) - added documentation about support for annotations to DevGuide.
- Issue [#2607](https://projects.tigase.org/issues/2607) - added @HandleStanzaTypes annotation to set supported stanza types and converted some processors to be annotation based as an example.
- Add Sure.IM to be part of Tigase XMPP Server release, include installer as well [#2105](https://projects.tigase.org/issues/2105).
- Web Installer Documentation.
- Issue [#2607](https://projects.tigase.org/issues/2607) - Added support for annotations on XMPPProcessors.
- Issue [#1080](https://projects.tigase.org/issues/1080) - Rewrite ResourceBind and authentication plugins as preprocessors.
- Issue [#1066](https://projects.tigase.org/issues/1066) - Cleanup StatRecord (removed unit and listValue).
- Change how tigase-web-ui.war is included in packages; [#2105](https://projects.tigase.org/issues/2105).
- Issue [#2564](https://projects.tigase.org/issues/2564) - Added documentation for Mobile v3 optimization.
- Issue [#2105](https://projects.tigase.org/issues/2105) - Added packaging of Sure.IM as tigase-web-ui.war to jars directory of distribution package.
- Javadoc cleanup, remove empty/stub javadocs from overriden methods; [#2541](https://projects.tigase.org/issues/2541).
- Initial Javadoc cleanup, fix errors; [#2541](https://projects.tigase.org/issues/2541).
- Allow to change domain after login. Useful when authzid is different than authcid. [#2522](https://projects.tigase.org/issues/2522).
- Issue [#2533](https://projects.tigase.org/issues/2533) - added methods that create ability to override creation of custom classes extending OutQueue and Counter.
- Reworked BOSH SID logger to utilize JUL filtering; [#2188](https://projects.tigase.org/issues/2188).
- Removing node infromation from DB; [#1946](https://projects.tigase.org/issues/1946).
- Issue [#2427](https://projects.tigase.org/issues/2427) - added configurable number of messages before requesting ack and issue [#2533](https://projects.tigase.org/issues/2533) - added possibility to override method responsible for deciding if ack request is needed as well as possibility to override implementation of any XMPPIOProcessor.
- Issue [#2097](https://projects.tigase.org/issues/2097) - added adhoc script to retrieve list of all cluster nodes.
- Include hostname in CounterDataLogger and include it in primary key; [#655](https://projects.tigase.org/issues/655) - Server statistics logging to DB.
- Guide on clustering check; [#2548](https://projects.tigase.org/issues/2548).
- Include MongoDB libraries in the distribution packages; fixes [#2545](https://projects.tigase.org/issues/2545).
- Task [#2521](https://projects.tigase.org/issues/2521) redesign internal SASL. Use BareJID as username instead of using only localpart.
- Issue [#2506](https://projects.tigase.org/issues/2506) - Added support to use prepared statements returning autogenerated keys.
- Add extended presence processors which allows adding any extended content to presence packets; [#2474](https://projects.tigase.org/issues/2474) sess-man/plugins-conf/presence/extended-presence-processors=.
- Change permissions checking so administrators won't be able to change domain owner.
- Domain blacklist - Server side implementation; [#2485](https://projects.tigase.org/issues/2485).
- Domain blacklist - documentation; [#2484](https://projects.tigase.org/issues/2484).
- XMLRepository Javadocs.
- Issue [#2429](https://projects.tigase.org/issues/2429) - Added support for S2S for mapping of domains to other names to allow support for intermediate server for S2S.
- Generate epub documentation; [#2171](https://projects.tigase.org/issues/2171).
- Documentation naming convention change.
- Include only selected types of documentation in distribution packages: [#2171](https://projects.tigase.org/issues/2171).
- Add 'Guide on accessing statistics', resolves [#2461](https://projects.tigase.org/issues/2461).

You can download the latest stable version from [distribution section](https://github.com/tigase/tigase-server/releases/tag/tigase-server-7.0.0).

Latest results of Tigase Testsuite are available for [Tigase Testsuite](https://build.tigase.net/tests-results/tts/) and [Tigase TTS-NG](https://build.tigase.net/tests-results/tts-ng/)
