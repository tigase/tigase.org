---
title: BeagleIM 3.3 and Siskin IM 5.3 released
tags:
  - siskinim
  - intro
date: "2017-05-24T22:40:32.169Z"
description: Tigase Messenger for iOS introduction.
categories:
  - blog
header:
  overlay_image: /assets/posts/tiger_cleanup_darker.png
---

With a bevy of new chat platforms available to consumers, it seems more and more that the emphasis of knowing who is online and what they are doing has fallen out of favor. In place of customized status messages and status alerts, there is now persistent message history, emoji character sets, and an emphasis for media. With this sea change of features, can XMPP mimic the new message direction? It certainly can.

Traditionally, XMPP relies on presence subscriptions to alert other users on a roster when a user is offline, away, or chatting. As it is a part of RFC 6122 it would be difficult to setup servers to do this exclusively. However, it is entirely possible that clients can decide not to use presence subscriptions keeping the RFC intact. It is possible to add users to a roster without requesting or providing a presence subscription using subscription=’none’. Furthermore, messages sent between users are only compared against a block list for processing from <a href="https://xmpp.org/extensions/xep-0016.html">XEP-0016</a> and <a href="https://xmpp.org/extensions/xep-0191.html">XEP-0191</a>. With these in place, the only remaining element to mimic current chat clients is to enable message storage and handling. In many modern chat systems sent messages are sent without feedback to users, and are stored for later use and recall. XMPP already has extensions for offline messaging and message archiving, specified in <a href="https://xmpp.org/extensions/xep-0160.html">XEP-0160</a> and <a href="https://xmpp.org/extensions/xep-0136.html">XEP-0136</a>. All of these features are supported by Tigase XMPP server which can handle these tasks ‘out of the box’ which I used as a test server.

Lastly is the client, which will automatically retrieve offline messages, retrieve history, and ignore presence requests to complete the experience. Recently I have been testing our Tigase Messenger for iOS, and was intrigued at the ability to try this out. One of the options available in the app is to turn off presence subscriptions and not to add any, which would essentially complete the stateless communication experience. Setting this has the added benefit of reducing a considerable amount of traffic: Presence traffic will not be transmitted or taken in from other users. In an XMPP environment, this reduction of verbosity now creates an environment stateless communication.

So what is stateless XMPP communication like? At first I had an unnerving feeling my messages would never get received, or other users on my roster would not send me messages. Although this is an oddity in the XMPP world, I became used to the idea fairly quickly. It took a little longer to get others to message me while offline, but after some coercion, messaging continued as normal. It was interesting to see that there was a large reduction of 'hello are you there' messages. As many of us have gotten used to with SMS, or modern chat services it is far more likely that messages sent will just contain needed information without extra back and forth. In practice, using this stateless XMPP setup can mimic very well how other platforms behave with very little change to the server behavior. In fact in some installations no change is necessary and the experience is driven solely by client configuration. The end result is a mobile-user friendly approach to XMPP that allows for the same robust communication that XMPP provides, but with the ‘send-it-and-forget-it’ convenience of modern applications.

XMPP is based on the principle of being flexible, so much so it’s within the acronym. Although not quite a feature set, an argument could be made that this style is supported on existing XMPP servers; without real modification. This is just one way the tools provided by XMPP can fit into whatever job needs to be done. Tigase Messenger for iOS can provide this experience if you’d like to try it out for yourself on any XMPP server that is supporting the above features. Search for Tigase in the App store, or follow the link in the <a href=http://www.tigase.net/content/tigase-messenger-ios>application page</a>.
