---
title: Tigase XMPP Server v7.1.4 Released!
tags:
  - server
  - server7
  - release
date: "2018-10-15T22:40:32.169Z"
description: Tigase v7.1.4 has been released! This is a maintenance release for Tigase v7.1.3 with a few fixes and updates. For changenotes for v7.1.0, visit [this link](../tigase-server-710-release) for detailed changenotes.
categories:
  - blog
header:
  overlay_image: /assets/posts/tiger_cleanup_darker.png
---

**Tigase XMPP Server v7.1.4** has been released! This is a maintenance release for Tigase v7.1.3 with a few fixes and updates. For changenotes for v7.1.0, visit [this link](../tigase-server-710-release) for detailed changenotes.

# Tigase v7.1.4 Changenotes

- [#7301](https://projects.tigase.org/issues/7301) - Improving AMP Component performance (should use multiple processing threads)
- [#8061](https://projects.tigase.org/issues/8061) - Make sure that RepositoryItems are correctly stored in repository
- [#6911](https://projects.tigase.org/issues/6911) - Fix issue that could lead to tigase.server.BasicComponent#getDefHostName returning empty/invalid value
- [#7687](https://projects.tigase.org/issues/7687) - Fix issue when connections that failed to authenticate within defined timeout resulted in all connections being marked as active
- [#7687](https://projects.tigase.org/issues/7687) - Add configuration of active-user-count timeframe period
- [#7304](https://projects.tigase.org/issues/7304) - Limit VHost log line length

Due to security issues of our previous certificate provider we were forced to migrate to different provider, which in turn entailed update of our certificates. This affects installations using our ACS component - update to version 7.1.4 is required to correctly renew licence file. The update must be applied within next 6 months. After this period, outdated installations will not be able to obtain updated licence file, which may lead to shutting down the service. In case update is not possible, licence file would have to be procured and updated manually.

Please contact us as we offer full support and help with updating the software.

You can download the latest stable version from [distribution section](https://github.com/tigase/tigase-server/releases/tag/tigase-server-7.1.4).
Latest results of Tigase Testsuite are available for [Tigase Testsuite](https://build.tigase.net/tests-results/tts/) and [Tigase TTS-NG](https://build.tigase.net/tests-results/tts-ng/)
