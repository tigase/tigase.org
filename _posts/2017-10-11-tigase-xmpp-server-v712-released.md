---
title: Tigase XMPP Server v7.1.2 Released!
tags:
  - server
  - server7
  - release
date: "2017-10-11T22:40:32.169Z"
description: Tigase XMPP Server v7.1.2 has been released! Please review the change notes below to see what has changed since our last release.
categories:
  - blog
header:
  overlay_image: /assets/posts/tiger_cleanup_darker.png
---

Tigase v7.1.2 has been released! This is a maintenance release for Tigase v7.1.1 with a few fixes and updates. For changenotes for v7.1.0, visit [this link](../tigase-server-710-release) for detailed changenotes.

# Tigase v7.1.2 Changenotes

## Input Buffer algorithm changed

The algorithm charged with resizing the input buffer size has been reworked. The new algorithm now takes less steps to shrink the input buffer to an appropriate size. This has improved memory usage under operation, and leaves Tigase XMPP Server with a smaller footprint when idle.

## TLS Buffer size reduced

As the input buffer has gotten smaller, so has the TLS buffer. We are now able to utilized 2k per connection instead of an allocated 16k (and 2k thereafter). This significantly reduces the amount of memory needed to run Tigase, and will benefit both high and low activity servers.

# Fixes

- [#5750](https://projects.tigase.org/issues/5750): Statistics retrieved over XMPP now adhere to level rules.
- [#5864](https://projects.tigase.org/issues/5864): Fixed NPE on pre-bind Bosh session script.
- [#6000](https://projects.tigase.org/issues/6000): Fixed issue with dynamic rosters not being recognised while broadcasting presence.

You can download the latest stable version from [distribution section](https://github.com/tigase/tigase-server/releases/tag/tigase-server-7.1.2).
Latest results of Tigase Testsuite are available for [Tigase Testsuite](https://build.tigase.net/tests-results/tts/) and [Tigase TTS-NG](https://build.tigase.net/tests-results/tts-ng/)
