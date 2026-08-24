---
title: Tigase XMPP Server v7.1.1 Released!
tags:
  - server
  - server7
  - release
date: "2017-08-29T22:40:32.169Z"
description: Tigase XMPP Server v7.1.1 has been released! Please review the change notes below to see what has changed since our last release.
categories:
  - blog
header:
  overlay_image: /assets/posts/tiger_cleanup_darker.png
---

**Tigase XMPP Server v7.1.1** has been released! Please review the change notes below to see what has changed since our last release.

This is a maintenance release for Tigase v7.1.0 with a number of fixes and updates. Although this guide will forego changenotes for v7.1.0, we are including major changes and new features list from that version since there are some customers who will be upgrading from older versions directly to v7.1.1. Visit [this link](../tigase-server-710-release) for detailed changenotes for v7.1.0.

# Changenotes

## AMP plugin has had some updates

Offline message retrieval has been slightly changed to use different methods. If you are using the below configuration:

```
sess-man/plugins-conf/http\://jabber.org/protocol/offline/amp-repo-class=tigase.archive.unified.db.JDBCFlexibleOfflineMessageRetrievalRepository
sess-man/plugins-conf/amp/amp-repo-class=tigase.archive.unified.db.JDBCFlexibleOfflineMessageRetrievalRepository
```

Replace it with these settings to use proper retrieval method.

```
sess-man/plugins-conf/http\://jabber.org/protocol/offline/amp-repo-class=tigase.archive.unified.db.JDBCFlexibleOfflineMessageRetrievalRepositoryWithRecents
sess-man/plugins-conf/amp/amp-repo-class=tigase.archive.unified.db.JDBCFlexibleOfflineMessageRetrievalRepositoryWithRecents
```

# New Minor Features & Behavior Changes

- [#1968](https://projects.tigase.org/issues/1986): Implemented a proxy method to synchronize database access on MySQL and prevent desync deadlocks.
- [#5057](https://projects.tigase.org/issues/5057): Tigase and all dependency binaries are now signed.
- Reduced SCRAM CallbackHandler log level from \`WARNING\` to \`FINE\`.

# Fixes

- [\#4878](https://projects.tigase.org/issues/4878): Improved Tigase `tig_users` table to be compatible with MySQL v5.7.
- [\#4908](https://projects.tigase.org/issues/4908): `MonitorComponent` is no longer visable or accessible to non-admin users.
- [\#4973](https://projects.tigase.org/issues/4973): Linited number of items queried when loading expired messages to a user-defined value.
- [\#5098](https://projects.tigase.org/issues/5098): Added StackTrace output to `SEVERE` errors on `SessionManager`.
- [\#5118](https://projects.tigase.org/issues/5118): Fixed NPE when sending certain `iq` stanzas, resulted in loss of in processing.
- [\#5338](https://projects.tigase.org/issues/5338): Fixed NPE retriving presence requests from offline repository.
- [\#5418](https://projects.tigase.org/issues/5418): Fixed potential race condition when identical stanzas are sent during stream negioation.
- [\#5604](https://projects.tigase.org/issues/5604): Mofidied MA and UA to enable retrieval of stored private chatroom messages.
- [\#5705](https://projects.tigase.org/issues/5705): Fixed NPE resulting from failed S2S connection.
- [\#5859](https://projects.tigase.org/issues/5859): Fixed exceptions being thrown when calling methods using reflection in `PreparedStatementInvocationHandler`.
- [\#5885](https://projects.tigase.org/issues/5885): Fixed handling of Subscription request when subscriber cancles request before approval. Rosters are no longer improperly updated.
- `db-create` scripts now include new pubsub schemas.
- `XMPPServer.getConfigurator()` has been marked as depreciated.
- Tigase MUC has been bumped to v2.4.1.
- Tigase PubSub has been bumped to v3.2.1.
- Message Archive has been bumped to v1.2.1.
- Unified Archive has been bumped to v1.0.1.

You can download the latest stable version from [distribution section](https://github.com/tigase/tigase-server/releases/tag/tigase-server-7.1.1).

Latest results of Tigase Testsuite are available for [Tigase Testsuite](https://build.tigase.net/tests-results/tts/) and [Tigase TTS-NG](https://build.tigase.net/tests-results/tts-ng/)
