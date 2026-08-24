---
title: Tigase XMPP Server v7.1.5 Released!
tags:
  - server
  - server7
  - release
date: "2019-01-24T22:40:32.169Z"
description: Tigase v7.1.5 has been released! This is a maintenance release for Tigase v7.1.4 with a few fixes and updates. For changenotes for v7.1.0, visit [this link](../tigase-server-710-release) for detailed changenotes.
categories:
  - blog
header:
  overlay_image: /assets/posts/tiger_cleanup_darker.png
---

**Tigase XMPP Server v7.1.5** has been released! Please review the change notes below to see what has changed since our last release. This is a maintenance release for Tigase v7.1.4 with a few fixes and updates. For changenotes for v7.1.0, visit [this link](../tigase-server-710-release) for detailed changenotes.

# Tigase v7.1.5 Changenotes

- [#7495](https://projects.tigase.org/issues/7495) - fix issue with not all logs being obfuscated, added testcase, documentation
- [#8226](https://projects.tigase.org/issues/8226) - added support for setting XMPP user status using REST API call

Due to security issues of our previous certificate provider we were forced to migrate to different provider, which in turn entailed update of our certificates. This affects installations using our ACS component - update to version 7.1.4 or newer is required to correctly renew licence file. The update must be applied until end of 2019. After this period, outdated installations will not be able to obtain updated licence file, which may lead to shutting down the service. In case update is not possible, licence file would have to be procured and updated manually.

Please contact us as we offer full support and help with updating the software.

You can download the latest stable version from [distribution section](https://github.com/tigase/tigase-server/releases/tag/tigase-server-7.1.5).
Latest results of Tigase Testsuite are available for [Tigase Testsuite](https://build.tigase.net/tests-results/tts/) and [Tigase TTS-NG](https://build.tigase.net/tests-results/tts-ng/)
