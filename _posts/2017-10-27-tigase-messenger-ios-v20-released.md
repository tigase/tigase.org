---
title: BeagleIM 3.3 and Siskin IM 5.3 released
tags:
  - beagleim
  - OMEMO
  - release
date: "2017-10-27T22:40:32.169Z"
description: Tigase Messenger for iOS v2.0 has been released! Please review the change notes below to see what has changed since our last public release.
categories:
  - blog
header:
  overlay_image: /assets/posts/tiger_cleanup_darker.png
---

**Tigase Messenger for iOS v2.0** has been released! Please review the change notes below to see what has changed since our last public release.

There are lots of new features available in this version. Please consult the documentation for more information on new features.

## Minor changes to software behavior

- [#4615](https://projects.tigase.org/issues/4615): vCard4 over XMPP now supported.
- [#5193](https://projects.tigase.org/issues/5193): Chats can now be sorted by recency and availbility.
- [#5194](https://projects.tigase.org/issues/5194): Added support to customize the number of preview messages presented in recent chats list.
- [#5211](https://projects.tigase.org/issues/5211): Added support manually set presence types.
- [#5383](https://projects.tigase.org/issues/5383): Notifications about new messages are now modified depending on thier origin.
- [#5805](https://projects.tigase.org/issues/5805): Push notifications may now be delivered while Away, XA, or DND is set depending on device confiration.
- [#5947](https://projects.tigase.org/issues/5947): Added support for file transfer using HTTP upload component (XEP-0363).
- [#5977](https://projects.tigase.org/issues/5977): Integrated HTTP file upload with native component and endbled sharing from other applications.
- [#5978](https://projects.tigase.org/issues/5978): Preview now available for files shared over HTTP upload component.
- [#5980](https://projects.tigase.org/issues/5980): Support has been added for Message Delivery Receipts (XEP-0184).
- [#5996](https://projects.tigase.org/issues/5996): Added option to replace full URL of shared file with HTML based 'link to file' link.
- [#6000](https://projects.tigase.org/issues/6000): Support added for one-way presence which can be set by server or by individual contact.
- [#6031](https://projects.tigase.org/issues/6031): Message text is now set to left instead of justifying.
- [#6033](https://projects.tigase.org/issues/6033): Ability to differentate contacts by account has been added.
- [#6074](https://projects.tigase.org/issues/6074): Roster edit form has been updated and expanded.
- [#6077](https://projects.tigase.org/issues/6077): Join MUC form has been updated.
- [#6081](https://projects.tigase.org/issues/6081): Auto-authorize option is disabled by default.
- [#6090](https://projects.tigase.org/issues/6090): Ability to copy sections of text and full messages, as well as share them has been added.
- [#6156](https://projects.tigase.org/issues/6156): Enter can now be set to send messages instead of add new line as an option.
- [#6185](https://projects.tigase.org/issues/6185): Text typed into chats is now saved if device is put to sleep before it is sent.

## Fixes

- [#5370](https://projects.tigase.org/issues/5370): Connectivity errors are now displayed on device to indicate and display failure conditions.
- [#5803](https://projects.tigase.org/issues/5803): When devices reconnect to servers, they will synchronize messages using XEP-0313 Message Archive Management if enabled on both device and server.
- [#5961](https://projects.tigase.org/issues/5961): Fixed crash from missing data when updating avatar.
- [#5965](https://projects.tigase.org/issues/5965): Fixed crash on processing incoming stanza while disconnecting, or opening chat with text unable to parse to a URL.
- [#5974](https://projects.tigase.org/issues/5974): Fixed crash when receiving messages in MUC while chat was open.
- [#5975](https://projects.tigase.org/issues/5975): Improved badge refresh rate.
- [#5985](https://projects.tigase.org/issues/5985): Fixed crash while viewing file preview and download file size set to unlimited.
- [#5999](https://projects.tigase.org/issues/5999): Fixed delay between detection of disconnection and delivery of push notification.
- [#6032](https://projects.tigase.org/issues/6032): Fixed visual bug displaying large text on smaller device in some setting menus.
- [#6035](https://projects.tigase.org/issues/6035): Fixed issue where delivery receipt messages could bounce back as errors.
- [#6069](https://projects.tigase.org/issues/6069): & [\#6155](https://projects.tigase.org/issues/6155) Application now updates information from roster pushes correctly.
- [#6100](https://projects.tigase.org/issues/6100): Modified dispatch queue usage to prevent a race condition when creating a chat from device.
- [#6101](https://projects.tigase.org/issues/6101): Improved JID comparison to prevent chats with same JID and different content showing in seperate chats in recent chat list.
- [#6110](https://projects.tigase.org/issues/6110): Improved database access and fixed concurrency issues from db timeout issues.
- [#6130](https://projects.tigase.org/issues/6130): Reduced usage of `UIApplication.delegate`.
- [#6167](https://projects.tigase.org/issues/6167): Improved stability of application after download failure due to connectivity issues.
- [#6170](https://projects.tigase.org/issues/6170): Fixed crashing when updating roster views.
- [#6172](https://projects.tigase.org/issues/6172): Fixed crash when opening MUC while client is fetching messages from MUC server.
- [#6303](https://projects.tigase.org/issues/6303): Fixed issue where recent screen could hang during message synchronization.

Please visit the **[iTunes Store](https://itunes.apple.com/us/app/tigase-messenger/id1153516838?mt=8)** to obtain the new version\!
