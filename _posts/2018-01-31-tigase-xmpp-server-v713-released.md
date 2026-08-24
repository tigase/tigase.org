---
title: Tigase XMPP Server v7.1.3 Released!
tags:
  - server
  - server7
  - release
date: "2018-01-31T22:40:32.169Z"
description: Tigase XMPP Server v7.1.3 has been released! Please review the change notes below to see what has changed since our last release.
categories:
  - blog
header:
  overlay_image: /assets/posts/tiger_cleanup_darker.png
---

Tigase v7.1.3 has been released! This is a maintenance release for Tigase v7.1.2 with a few fixes and updates. For changenotes for v7.1.0, visit [this link](../tigase-server-710-release) for detailed changenotes.

# Tigase v7.1.3 Changenotes

## Option to allow external connections using SSL

Previously only plain socket and TLS connections were supported. This change allows using also SSL sockets for external component connections.

# Fixes

- [#6363](https://projects.tigase.org/issues/6363): Fix missing namespaces in packets sent as responses for adhoc commands.
- [#6408](https://projects.tigase.org/issues/6408): Fix issue with multiple XML stanzas sent in single WebSocket frame.
- [#6521](https://projects.tigase.org/issues/6521): Fix ordering of recents queries - always use timestamps for comparison in Unified Archiving component.
- [#6657](https://projects.tigase.org/issues/6657): Fix missing index on tig_ma_jids in Unified Archiving component.

You can download the latest stable version from [distribution section](https://github.com/tigase/tigase-server/releases/tag/tigase-server-7.1.3).
Latest results of Tigase Testsuite are available for [Tigase Testsuite](https://build.tigase.net/tests-results/tts/) and [Tigase TTS-NG](https://build.tigase.net/tests-results/tts-ng/)
