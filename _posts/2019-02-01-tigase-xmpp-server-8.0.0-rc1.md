---
title: Tigase XMPP Server 8.0.0-RC1 - first Release Candidate
tags:
  - server
  - release
date: "2019-02-01T22:40:32.169Z"
description: After a long development period first Release Candidate release of Tigase XMPP Server, packed with many new features and improvements, is available.
categories:
  - blog
header:
  overlay_image: /assets/posts/tiger_cleanup_darker.png
---

First Release Candidate of next major version of **Tigase XMPP Server 8.0.0** has been released - and it’s packed with features and improvements.

## For users

Version 8 packs following goodies for users:

### Share files with ease

Thanks to implementation of [XEP-0363 HTTP File Upload](https://docs.tigase.net/tigase-server/8.0.0/Administration_Guide/html/#_xep_0363_http_file_upload_now_supported) it’s easier to exchange photos & videos, documentes and more.

### Never miss-out on expressing yourself

Messages stored in the MySQL repository (message archive, groupchat) [correctly handle emojis now](https://docs.tigase.net/tigase-server/8.0.0/Administration_Guide/html/#_emojis_now_supported_on_tigase_xmpp_servers) (Tigase XMPP Server supported and handled exchanged messages with emoji for a long time without problems)

### Never skip a beat

No matter what kind of internet connection you have, thanks to implementation of [XEP-0357: Push Notifications](https://docs.tigase.net/tigase-server/8.0.0/Administration_Guide/html/#_tigase_push_component) you will never miss a message (you should use compatible XMPP Client, we recommend [Tigase iOS Messenger](https://tigase.net/content/tigase-messenger-ios) and [Tigase Android Messenger](https://tigase.net/content/tigase-messenger-android)) and support for [XEP-305 Quickstart](https://docs.tigase.net/tigase-server/8.0.0/Administration_Guide/html/#_xep_305_quickstart_now_supported) will allow you to send messages faster than ever.

### Seamlessly switch between devices

With brand new implementation of [XEP-0313 Message Archive Management (MAM)](https://docs.tigase.net/tigase-server/8.0.0/Administration_Guide/html/#_xep_0313_message_archive_management_now_supported) you can now easily switch devices without loosing flow of your conservation.

### Don’t get distracted

Have you ever received unwanted message? After enabling [Spam Protection](https://docs.tigase.net/tigase-server/8.0.0/Administration_Guide/html/#_spam_protection) in Tigase XMPP Server those will be a thing of the past!

## For administrators

Benefits for all administrators:

### Protection

You can now better protect your installation by enabling [protection against brute-force attacks](https://docs.tigase.net/tigase-server/8.0.0/Administration_Guide/html/#_protection_against_brute_force_attacks) and [CAPTCHA system now available for in-band registration](https://docs.tigase.net/tigase-server/8.0.0/Administration_Guide/html/#_captcha_system_now_available_for_in_band_registration)

### Security

Starting with Tigase XMPP Server 8.0.0 [we are signing all our artifacts](https://docs.tigase.net/tigase-server/8.0.0/Administration_Guide/html/#_all_artifacts_are_signed) and we facilitate [using BouncyCastle for StartTLS](https://docs.tigase.net/tigase-server/8.0.0/Administration_Guide/html/#_bouncycastle_being_used_for_starttls)

By default [all user passwords in repository are hashed](https://docs.tigase.net/tigase-server/8.0.0/Administration_Guide/html/#_changes_in_password_storage) giving your users even greater peace of mind.

### Maintenance

We decided to introduce [TDSL - our new configuration file format](https://docs.tigase.net/tigase-server/8.0.0/Administration_Guide/html/#_new_configuration_file_format) which makes configuring The Server a piece of cake - with well organised structure it’s easier to navigate through the file and support for variables gives you even more flexibility when configuring the service. There is also automatic type detection, so you don’t have to worry about specifying it correctly anymore.

Please note, that we also changed how you configure your domains/VHost - now you only specify single, main domain and you can manage the rest via web browser interface (see: [here](https://docs.tigase.net/tigase-server/8.0.0/Administration_Guide/html/#_default_virtual_host_property_changes))

### Migration from other servers

If you ever wanted to use Tigase XMPP Server but had already started using alternative solutions you have now possibility to migrate all data to Tigase - read more in [Tigase Database Migrator](https://docs.tigase.net/tigase-database-migrator/1.0.0/Tigase_Database_Migrator_Guide/html/) documentation.

## For developers

[Tigase Kernel is here](https://docs.tigase.net/tigase-server/8.0.0/Administration_Guide/html/#_kernel_and_beans_configuration)! It is implementation of [inversion of control (IoC)](https://en.wikipedia.org/wiki/Inversion_of_control) which makes developing with Tigase Platform much easier - for more details dive into [Tigase Kernel documentation](https://docs.tigase.net/tigase-server/8.0.0/Development_Guide/html/#tigasekernel)

## Other improvements

For detailed list of all the changes please check [Tigase XMPP Server 8.0.0 announcement](https://docs.tigase.net/tigase-server/8.0.0/Administration_Guide/html/#tigase800)

## Improvements over RC version

Comparing with recently published Release Candidate version following fixes has been made: \* improve compatibility with MySQL version 8.0.20 and newer

## Downloads

- [Binaries (distribution packages)](https://github.com/tigase/tigase-server/releases/tag/tigase-server-8.0.0):
  - [tigase-server-8.0.0-b10083-dist-max.tar.gz](https://github.com/tigase/tigase-server/releases/download/tigase-server-8.0.0/tigase-server-8.0.0-b10083-dist-max.tar.gz)
  - [tigase-server-8.0.0-b10083-dist-max.zip](https://github.com/tigase/tigase-server/releases/download/tigase-server-8.0.0/tigase-server-8.0.0-b10083-dist-max.zip)
- [Source Code](https://github.com/tigase/tigase-server/tree/tigase-server-8.0.0)
- [Maven Artifacts](https://maven-repo.tigase.net/#artifact/tigase/tigase-server/8.0.0)

# Test results

- [Tigase Testsuite](https://build.tigase.net/tests-results/tts/)
- [Tigase TTS-NG](https://build.tigase.net/tests-results/tts-ng/)
