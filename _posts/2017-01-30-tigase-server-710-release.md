---
title: Tigase XMPP Server v7.1.0 Released!
tags:
  - server
  - server7
  - release
date: "2017-01-30T22:40:32.169Z"
description: Tigase XMPP Server 7.1.0 has been released! Please review the change notes below to see what has changed since our last release.
categories:
  - blog
header:
  overlay_image: /assets/posts/tiger_cleanup_darker.png
---

**Tigase XMPP Server 7.1.0** has been released! Please review the change notes below to see what has changed since our last release.

Introducing **Tigase XMPP Server 7.1.0!** We have been working hard to improve and implement new features to the Tigase Seve to give you a more secure, leaner, and better working XMPP server. We have a few new features, components, and lots of fixes to share. Please note that not all issues are accessible as submitted notes may contain sensitive information. Binaries are available from the project's [files section](https://github.com/tigase/tigase-server/releases/tag/tigase-server-7.1.0). Sources are available in our [repository](https://github.com/tigase/tigase-server/tree/tigase-server-7.1.0). Maven artifacts have been deployed to our [maven repository](http://maven-repo.tigase.org/#artifact/tigase/tigase-server/7.1.0). Test results are located on our [test page](http://build.tigase.org/~tigase/).

## Major Changes

Tigase has undergone a few major changes to our code and structure. To continue to use Tigase, a few changes may be needed to be made to your systems. Please see them below:

### HTTP Component renamed

The HTTP component has been renamed, if you still have the old tigase.rest.RestMessageReciever in your init.properties file, please update the component name to:

```
tigase.http.HttpMessageReceiver
```

### New JDK v8 required

As Oracle has dropped support for version 7 of it's Java runtime environment and developer kit, we have moved to version 8 of the JDK. Furthermore, some new features and fixes for Tigase Server now require the use of JDK v8 or later. Please upgrade your Java packages from [this link](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).

### Changes to Database Schemas

Tigase has undergone a number or database schema changes, the current versions being main database schema v7.1 and pubsub schema v3.2.0. If you are upgrading to v7.1.0 from a previous version of tigase, it is recomended you visit [this section](http://docs.tigase.org/tigase-server/7.1.0/Administration_Guide/html/#v710notice) in the documentation to prepare your new installation.

### Presence Plugin Split

The plugin handling all presence processing has been split from one plugin (`Presence.java`) into separate plugins:

- `PresenceAbstract.java` handles most common presence-related methods, and is also used by the following two plugins.
  - `PresenceSubscription.java` to handle subscription presence processing like for roster updates.
  - `PresenceState.java` to handle initial presences from new logins.

## New Features & Components

### New HTTP API

Tigase now features an HTTP API that not only allows web client chat, but administrators can change settings, manage users, and even write and run scripts all from the comfort of a browser window. Furthermore, commands can be passed through this interface using REST to create and run custom scripts and commands. We plan on expanding on the look and feel of this interface as time goes on, but in the meantime enjoy the real-time XMPP experience now with a user-friendly GUI.

### New Admin HTTP interface

Tigase now comes with its own build-in web XMPP client! It can be accessed from http://yourhost.com:8080/ui/. For more details, see the Admin UI guide.

### Added support for XEP-0334

XEP-0334 is now supported. See [this section](http://docs.tigase.org/tigase-server/7.1.0/Administration_Guide/html/#nonBodyElements) for details.

### Kernel Bean Configurator has been Improved

Added aliases for bean properties to allow for a 'high level' of configuration. Instead of using

```
component/bean-name/property=value
```

The following easier to use method will work

```
component/property=value
```

### Support for XEP-0352

Client State Indication is now enabled by default on Tigase XMPP Servers. Details [here](http://docs.tigase.org/tigase-server/7.1.0/Administration_Guide/html/#sessManMobileOpts).

### One Certificate for multiple Vhosts

Tigase now allows for wildcards in setting server certificate per Vhosts. See more [in this section](http://docs.tigase.org/tigase-server/7.1.0/Administration_Guide/html/#onecertmultipledomain).

### Maximum users setting for MUC

Administrators can now set that maximum number of users allowed on specific MUCs. See [MUC Room Configuration](http://docs.tigase.org/tigase-server/7.1.0/Administration_Guide/html/#mucRoomConfig).

### HTTP Rest API Support

Tigase now supports REST commands via HTTP, they can be sent from ad-hoc commands, a web interface, or other REST tools. See [documentation](http://docs.tigase.org/tigase-server/7.1.0/Administration_Guide/html/#tigase_http_api) for more.

### Empty Nicknames

Tigase can now support users with empty nicknames. See [this](http://docs.tigase.org/tigase-server/7.1.0/Administration_Guide/html/#emptyNicks) for details.

### Offline Message Limits

Tigase now has support to enable and change Offline Message Limits as handled by AMP. [Documentation here](http://docs.tigase.org/tigase-server/7.1.0/Administration_Guide/html/#offlineMessageLimits).

### Offline Message Sink

A new way to store offline messages has been implemented, it may not replace standard offline messages, but can be used in other ways. [Documentation here](http://docs.tigase.org/tigase-server/7.1.0/Administration_Guide/html/#offlineMessageSink).

### Adding Components to trusted list

Components can now be added to trusted list and will be shared with all clustered servers. [#3244](https://projects.tigase.org/issues/3244)

### Tigase Mailer Extension now Included

Tigase Mailer extension is now included in distributions of Tigase server. This extension enables the monitor component to deliver E-mails to and from specified e-mail addresses when monitor are triggered. For more information see [monitor mailer section](http://docs.tigase.org/tigase-server/7.1.0/Administration_Guide/html/#monitorMailer).

### EventBus implemented

Tigase now has a simple PubSub component called EventBus to report tasks and triggers. More details are available [Here](http://docs.tigase.org/tigase-server/7.1.0/Administration_Guide/html/#eventBus).

### XEP-0191 Blocking Command Support added

Blocking Command support has been added to Tigase, all functions of [XEP-0191](http://xmpp.org/extensnions/xep-0191/html) should be implemented. See [Admin Guide](http://docs.tigase.org/tigase-server/7.1.0/Administration_Guide/html/#blockingCommand) for details.

### Stream management now has new settings available for stream timeout

Maximum stream timeout and default stream timtout times can now be set in init.properties. Details of these two settings can be found [here](http://docs.tigase.org/tigase-server/7.1.0/Administration_Guide/html/#streamResumptiontimeout).

### JVM Default configuration updated

Default tigase.conf file has been updated with the following change in JVM options:

```
PRODUCTION_HEAP_SETTINGS=" -Xms5G -Xmx5G " # heap memory settings must be adjusted on per deployment-base!
JAVA_OPTIONS="${GC} ${EX} ${ENC} ${DRV} ${JMX_REMOTE_IP} -server ${PRODUCTION_HEAP_SETTINGS} -XX:MaxDirectMemorySize=128m "
```

As the comment says, we recommend adjusting the heap memory settings for your specific installations. [#3567](https://projects.tigase.org/issues/3567)

### Java Garbage Collection Settings have been improved

After significant testing and investigation, we have improved the Java GC settings to keep memory usage from becoming too high on systems. [#3248](https://projects.tigase.org/issues/3248) For more information about JVM defaults and changes to settings, see [our Documentation](http://docs.tigase.org/tigase-server/7.1.0/Administration_Guide/html/#jvm_settings).

### New Rest API added to obtain a JID login time

`GetUserInfo` command has been expanded to obtain user login and logout times in addition to standard information. See [this section](http://docs.tigase.org/tigase-server/7.1.0/Administration_Guide/html/#getUserInfoREST) for full details.

### New init.properties properties

- `--ws-allow-unmasked-frames=false` - Allows for unmasked frames to be sent to Tigase server VIA Websocket and not force-close the connection when set to true. RFC 6455 specifies that all clients must mask frames that it sends to the server over Websocket connections. If unmasked frames are sent, regardless of any encryption, the server must close the connection. Some clients however, may not support masking frames, or you may wish to bypass this security measure for development purposes.
- `--vhost-disable-dns-check=true` - Disables DNS checking for vhosts when changed or edited. When new vhosts are created, Tigase will automatically check for SRV records and proper DNS settings for the new vhosts to ensure connectivity for outside users, however if these validations fail, you will be unable to save those changes. This setting allows you to bypass that checking.

### Connection Watchdog

A watchdog property is now available to monitor stale connections and sever them before they become a problem. More details [here](http://docs.tigase.org/tigase-server/7.1.0/Administration_Guide/html/#watchdog).

### Web Installer Setup Page now has restricted access

The Web Installer Setup Page, available through http://yourserver.com/8080/setup/ now requires an admin level JID or a user/password combo specified in init.properties. See the [Web Installer](http://docs.tigase.org/tigase-server/7.1.0/Administration_Guide/html/#webinstall) section for default settings. See [Component Properties](http://docs.tigase.org/tigase-server/7.1.0/Administration_Guide/html/#httpCompProp) section for details on the new property.

### Offline Message Receipts Storage now Configurable

Admins may now configure Offline Message Receipts Storage to specify filters and controls as to what they want stored in offline messages. See [more details here](http://docs.tigase.org/tigase-server/7.1.0/Administration_Guide/html/#offlineMessageReceipts).

### Account Registration Limits

In order to protect Tigase servers from DOS attacks, a limit on number of account registrations per second has been implemented. See [this link](http://docs.tigase.org/tigase-server/7.1.0/Administration_Guide/html/#accountRegLimit) for configuration settings.

### Enable Silent Ignore on Packets Delivered to Unavailable Resources

You can now have Tigase ignore packets delivered to unavailable resources to avoid having a packet bounce around and create unnecessary traffic. Learn how [here](http://docs.tigase.org/tigase-server/7.1.0/Administration_Guide/html/#silentIgnore).

### Cluster Connections Improved

Cluster commands now operate at CLUSTER priority, giving the packets higher status than HIGH which otherwise has caused issues during massive disconnects. New Configuration options come with this change. The first being able to change the number of connections for CLUSTER packets using the following init.property setting:

```
cl-comp/cluster-sys-connections-per-node[I]=2
```

Also a new class which implements the a new connection selection interface, but uses the old mechanism where any connection can send any command.

```
cl-comp/connection-selector=tigase.cluster.ClusterConnectionSelectorOld
```

### Cluster Connections Testing Implemented

Watchdog has now been added to test cluster connections by default. Watchdog sends an XMPP ping to all cluster connections every 30 seconds and checks to see if a ping response has been received in the last 3 minutes. If not, the cluster connection will be dropped automatically. Global watchdog settings will not impact cluster testing feature.

### Cluster Map implemented

Tigase can now generate cluster maps through a new API. See the [development guide](http://docs.tigase.org/tigase-server/7.1.0/Development_Guide/html/#clusterMapInterface) for a description of the API.

### New Licensing Procedures

With the release of Tigase XMPP server v7.1.0, our licensing procedures have changed. For more information about how to obtain, retain, and install your license, please see [this section](http://docs.tigase.org/tigase-server/7.1.0/Administration_Guide/html/#licenseserver).

### Message Archive expanded to include non-body elements

Message Archive can now be configured to store messages that may not have body element, this option is explained in [this section](http://docs.tigase.org/tigase-server/7.1.0/Administration_Guide/html/#nonBodyStore).

### New Ability to Purge Data from Unified Archive

Data from Unified Archive or Message Archive can be automatically or manually purged depending on age or expired status. Information on configuring this is available [here](http://docs.tigase.org/tigase-server/7.1.0/Administration_Guide/html/#maPurging).

### Server Statistics Expanded

Server Statistics for Tigase XMPP Server have been expanded, and now will print at the close of a server session, or may be obtained in the normal way. Note that some statistics have changed since previous versions, and may have different formatting. See [the Statistics Description](http://docs.tigase.org/tigase-server/7.1.0/Administration_Guide/html/#statsticsDescription) section of the Administration guide for all current server statistics.

### Force Redirection

It's possible now to redirect connections on one port to be forced to connect to another port using the `force-redirect-to` setting. [Details here](http://docs.tigase.org/tigase-server/7.1.0/Administration_Guide/html/#_enforcing_redirection).

### Dual IP installtions

Tigase now has a Dual IP setup which can now use a separate internal and external IP and use a DNS resolver for the connection redirection. Setup instructions are [Located here](http://docs.tigase.org/tigase-server/7.1.0/Administration_Guide/html/#_configuring_hostnames).

### Error counting

It is now possible to conduct error counting and collect it from statistics. This feature is explained in more detail [here](http://docs.tigase.org/tigase-server/7.1.0/Administration_Guide/html/#errorCounting).

### New Database Disconnections Counter

3 new statistics were added to `basic-conf` to help monitor database connection stability, and how often the XMPP Server needs to reconnect to the database. The list of new statistics are listed [here](http://docs.tigase.org/tigase-server/7.1.0/Administration_Guide/html/#repo-factoryStatistics).

### New Known Cluster Statistic

A new statistic has been added to cl-comp displaying the number of connected Cluster Nodes if there are more than one. Displayed as an INFO level statistic.

### New Documentation Structure

There has been a lot of changes and fixes to our documentation over the last few months. If you have links to any of our documentation, please update them as the filenames may have changed.

### Full XML of last available presence may be saved to repository

A more detailed last available presence can now be made from some configuration changes, along with a timestamp before the entire presence stanza is saved to the repository. More information is available [here](http://docs.tigase.org/tigase-server/7.1.0/Administration_Guide/html/#storeFullXMLLastPresence).

### Setting available to enable automatic subscriptions

Tigase supports enabling automatic presence subscriptions and roster authorizations. For more information on these settings, check the [Automatic Subscriptions](http://docs.tigase.org/tigase-server/7.1.0/Administration_Guide/html/#autoSub) section.

### Stacktrace on Shutdown

Tigase will now dump the stacktrace upon shutdown by default. For more information, check [this description](http://docs.tigase.org/tigase-server/7.1.0/Administration_Guide/html/#shutDownStackTrace).

### New logic handling re-delivery of packets

Previously, Tigase would retry deliverying command packets that failed to send after a brief delay of 60 seconds. This new method can provide relief in sutations where command packet queues can get full. The new logic works like this: The delay for retries, after the first delay of 60 seconds will increase by a factor of 1.5, so the 2nd retry will then be 90 seconds, and then 135 and so on, until the retry limit has been reached (default is 15). Included in this is a new setting for setting the retry count, available [here](http://docs.tigase.org/tigase-server/7.1.0/Administration_Guide/html/#packetRedelivery).

### New Program Defaults

Tigase has improved default settings to improve performance. These include:

- Increase the number of database connections in relation to the number of available CPUs (factor by 4 by default).
- Plugins code has been revised.
- Default thread pools have been increased for better performance.
- New option added to globally increase thread pool counts `sess-man/sm-threads-factor[I]=1` Setting increases thread factor by specified factor.
- Improved JVM default settings.
- Improved JVM Garbage Collection.

## New Minor Features & Behavior Changes

- Old monitor component depreciated and turned off
- JTDS MS SQL Server driver updated to v1.3.1.
- `tigase-utils` and `tigase-xmltools` are now included in tigase-server builds
- Tigase Kernal has been updated and improved
- `tigase.stats.CounterDataFileLogger` file now includes timestamps
- Javadoc is no longer generated by installer as files are already included in distributions
- Node connection events to administrator have been improved and are more informative
- `ServerConnectionManager` has officially been depreciated from use in s2s.
- Logs now include information about respository reconnections.
- WebSocket PONG frams are now supported.
- [#163](https://projects.tigase.org/issues/163): [XEP-0012](http://xmpp.org/extensions/xep-0012.html) User `LastActivity` implemented.
- [#593](https://projects.tigase.org/issues/593): [XEP-0202 Entity Time](http://xmpp.org/extensions/xep-0202.html) has been implemented.
- [#788](https://projects.tigase.org/issues/788): End User Session from [XEP-0133 Service Administration](http://xmpp.org/extensions/xep-0133.html) implemented.
- [#811](https://projects.tigase.org/issues/811): Plugin API extended allowing more XML parameters to be considered for processing.
- [#813](https://projects.tigase.org/issues/813): Default number of connections between cluster nodes set at 5, default number of connections for CLUSTER level traffic set to 2.
- [#1436](https://projects.tigase.org/issues/1436): `ClusterConnectionManager` now sends ping packets every 30 seconds to check status of live cluster connections.
- [#1449](https://projects.tigase.org/issues/1449): Monitoring can now be run in OSGI mode.
- [#1601](https://projects.tigase.org/issues/1601): XMPPPresenceUpdateProcessorIFC interface has been removed and replaced with eventbus with dedicated threadpool.
- [#1783](https://projects.tigase.org/issues/1783): Cluster node connections/reconnections has been improved. Clusters now receieve a 'isclusterready?' query before connecting to avoid synchronization issues. Furthermore, an additional property has been added to change timeout of inactive clusters. See [Properties guide](http://docs.tigase.org/tigase-server/7.1.0/Properties_Guide/html/#clusterPortDelayListening) for more details.
- [#2426](https://projects.tigase.org/issues/2426): Support for [XEP-0334](http://xmpp.org/extensions/xep-0334.html) has been added.
- [#2530](https://projects.tigase.org/issues/2530): RosterFlat implementation now allows for a full element to be injected into presence stanzas instead of just a custom status.
- [#2561](https://projects.tigase.org/issues/2561): & [#85](https://projects.tigase.org/issues/85) Offline messages now consider sessions without presence & resources negative priority in delivery logic.
- [#2596](https://projects.tigase.org/issues/2596): Delivery errors are no longer run through preprocessors.
- [#2823](https://projects.tigase.org/issues/2823): `staticStr` element method now implemented.
- [#2835](https://projects.tigase.org/issues/2835): Allowing of `setPermissions` on incoming packets before they are processed by plugins.
- [#2903](https://projects.tigase.org/issues/2903): `see-other-host` has new option to make it configurable on a per vhost basis.
- [#3034](https://projects.tigase.org/issues/3034): Improved handling of data types and primitives within Tigase.
- #[3173](https://projects.tigase.org/issues/3173): Stanzas with unescaped XML special characters are now ignored instead of sending a force-close of connection to sender.
- [#3180](https://projects.tigase.org/issues/3180): Protected access to JDBC repository now enabled.
- [#3230](https://projects.tigase.org/issues/3230): Verification added to check against CUSTOM domain rules when submitted.
- #[3258](https://projects.tigase.org/issues/3258): Retrieval of PubSub/PEP based avatars using REST API now supported. [Command URLs here](http://docs.tigase.org/tigase-server/7.1.0/Administration_Guide/html/#avatarRetrievalRequests).
- #[3282](https://projects.tigase.org/issues/3282): VCard4 support added along with VCardTemp compatibility and integration.
- [#3285](https://projects.tigase.org/issues/3285): Stream Management changed to fully support XEP-0203.
- [#3330](https://projects.tigase.org/issues/3330): Error for adding users already in db now returns Error 409 with `User exists`.
- #[3364](https://projects.tigase.org/issues/3364): Clustering support has been re-factored to remove duplicate `nodeConnected` and `nodeDisconnected` methods.
- #[3463](https://projects.tigase.org/issues/3463): `offline-roster-last-seen` feature as a part of presence probe is now disabled by default.
- [#3496](https://projects.tigase.org/issues/3496): TigUserLogout has been improved to use `sha1_user_id = sha1(lower(_user_id))` instead of `"_user_id"`.
- [#3511](https://projects.tigase.org/issues/3511): Stream closing mechanism in SessionManager, new STREAM_CLOSED command has been added to organize shutdown of XMPP streams.
- #[3569](https://projects.tigase.org/issues/3569): Fixed error occuring when attempting to remove offline users from roster.
- #[3609](https://projects.tigase.org/issues/3609): Added new configuration option for BOSH to disable hostname attribute. [Details here](http://docs.tigase.org/tigase-server/7.1.0/Administration_Guide/html/#tip_1_bosh_in_cluster_mode_without_load_balancer).
- [#3670](https://projects.tigase.org/issues/3670): Hardened mode now uses long DH keys (2048) by default.
- [#3849](https://projects.tigase.org/issues/3849): New Roster size limit configurable setting. See info [Here](http://docs.tigase.org/tigase-server/7.1.0/Administration_Guide/html/#rosterLimit).
- [#3872](https://projects.tigase.org/issues/3872): PostgreSQL driver updated to v9.4.
- #[3892](https://projects.tigase.org/issues/3892): PEP plugin now supports processing of http://jabber.org/protocol/pubsub#owner.
- [#3908](https://projects.tigase.org/issues/3908): Logs now print whether components or plugins are depreciated, and recommend configuration settings changes.
- [#3937](https://projects.tigase.org/issues/3937): Windows setup given one-click solution to file initialization.
- [#3945](https://projects.tigase.org/issues/3945): SSLContextContainer has been replaced with JDKv8 extension version now known as SNISSLContextContainer adding support for SNI of SSL/TLS.
- [#3948](https://projects.tigase.org/issues/3948): Tigase PubSub now responds to +disco#info+ requests in line with [XEP-0060 - Discover Node Metadata](http://xmpp.org/extensions/xep-0060.html#entity-metadata).
- [#3950](https://projects.tigase.org/issues/3950): MongoDB driver updated to v2.14.1.
- #[3985](https://projects.tigase.org/issues/3985): Improved disco#info to return extended results for MUC component as per [XEP-0128 Service Discovery Extensions](http://xmpp.org/extensions/xep-0128.html).
- #[3986](https://projects.tigase.org/issues/3986): Added new index for tig_nodes collection in MongoDB Databases.
- [#4003](https://projects.tigase.org/issues/4003): VisualVM and statistics gathering has been improved and refactored for a lower footprint, and a number of new windows and features for program monitoring.
- #[4020](https://projects.tigase.org/issues/4020): SeeOtherHostSualIP implementation has been fixed to support MongoDB.
- [#4120](https://projects.tigase.org/issues/4120): Duplication of messages in offline storage when +Message+ and +OfflineMessage+ processors are used has been resolved.
- #[4127](https://projects.tigase.org/issues/4127): Database reconnection procedure improved, increased `SchuduledExecutorService` threads to 2, `ClusterConnectionManager` threads to 4, and added timeout parameter to timeout task.
- #[4162](https://projects.tigase.org/issues/4162): xml:lang attribute is now supported in Tigase MUC component.
- #[4248](https://projects.tigase.org/issues/4248): Changed `ErrorCounter` from `XMPPProcessorIFC` to `XMPPPacketFilterIfc` for more accurate functionality.
- [#4254](https://projects.tigase.org/issues/4254): Fixed `JabberIqPrivacy` to properly compare JID's instead of strings, improved partial JID matching, and added tests to add non-case sensitive comparison in MA/UA.
- [#4256](https://projects.tigase.org/issues/4256): Reduced Statistics memory usage by interning statistics labels and changing data types.
- [#4356](https://projects.tigase.org/issues/4356): Message Archive component has been converted to kernel and beans.
- [#4352](https://projects.tigase.org/issues/4352): Websocket implementation has been changed to properly parse HTTP headers with omitted spaces for HTTP 1.1 protocol.
- [#4358](https://projects.tigase.org/issues/4358): Several methods have been renamed or removed to prepare for v7.2.0 Kernel setup.
- #[4385](https://projects.tigase.org/issues/4385): Minor tweak to web-installer to skip unnecessary showing of blank init.properties file & enabled post-setup editing and saving.
- [#4665](https://projects.tigase.org/issues/4665): Full Stacktrace will be dumped to log file by default, see [this section](http://docs.tigase.org/tigase-server/7.1.0/Administration_Guide/html/#shutDownStackTrace) for more details.
- #[4739](https://projects.tigase.org/issues/4739): LetsEncrypt certficates are now included in dedicated keystore.
- [#4752](https://projects.tigase.org/issues/4752): New methods added to Tigase: _tigase.component.modules.Moduleprovider#getModules_ and _tigase.component.modules.ModuleProvider#getModulesId_ allowing for hooking of component modules. Fixed issue with reconnection of cluster nodes after a network lag.

## Fixes

- [#8](https://projects.tigase.org/issues/8): XML parser no longer passes malformed XML statements to server.
- [#1396](https://projects.tigase.org/issues/1396): & [#663](https://projects.tigase.org/issues/663) User roster behaves correctly. Tigase now waits for user authorization before users are added to a Roster.
- [#1488](https://projects.tigase.org/issues/1488): NPE in ad-hoc for managing external components fixed.
- [#1602](https://projects.tigase.org/issues/1602): Minor optimization in MessageCarbons with new functions added to `XMPPResourceConnection`.
- [#2003](https://projects.tigase.org/issues/2003): Fixed bug with C2S streams where server would not always overwrite from attribute with full JID in subcription-related presence stanzas.
- [#2118](https://projects.tigase.org/issues/2118): Username modification bugfix. Tigase now returns "" for blank usernames instead of string after a username has been made blank.
- [#2859](https://projects.tigase.org/issues/2859): & [#2997](https://projects.tigase.org/issues/2997) STARTTLS stream error on SSL sockets fixed.
- [#2860](https://projects.tigase.org/issues/2860): Fixed issue with SSL socket client certificate not working.
- [#2877](https://projects.tigase.org/issues/2877): Fixed issue in Message Carbons if message contains AMP payload.
- [#3034](https://projects.tigase.org/issues/3034): Streamlined primitive and Object array handling.
- [#3067](https://projects.tigase.org/issues/3067): Fixed Bug where if duplicate commands were sent to MS SQLServer a race condition would occur.
- [#3075](https://projects.tigase.org/issues/3075): Fixed error when compiling Tigase in Red Hat Enterprise Linux v6.
- [#3080](https://projects.tigase.org/issues/3080): `--net-buff-high-throughput` now parses integers properly. Setting no longer reverts to default when new values are set.
- [#3126](https://projects.tigase.org/issues/3126): Calculation of percentage of heap memory used in Statistics now selects proper heap.
- [#3131](https://projects.tigase.org/issues/3131): Fixed messages with AMP payload bound for plugins getting redirected to AMP for processing.
- [#3150](https://projects.tigase.org/issues/3150): Default Log level changed for certain records. All log entries with skipping admin script now have log level `FINEST` instead of `CONFIG`
- [#3158](https://projects.tigase.org/issues/3158): Fixed issue with OSGi not reporting proper version, and PubSub errors in OSGi mode.
- [#3159](https://projects.tigase.org/issues/3159): User Privacy lists now activate properly and does not wait for presence stanza to filter packets.
- [#3164](https://projects.tigase.org/issues/3164): Fixed NPE in `StreamManagementIOProcessor` when <span></span> is processed after connection is closed.
- [#3166](https://projects.tigase.org/issues/3166): NPE in SessionManager checking SSL null connections fixed.
- [#3181](https://projects.tigase.org/issues/3181): S2S connection multiplexing now has consistent behavior.
- [#3194](https://projects.tigase.org/issues/3194): Fixed issue with single long lasting HTTP connection blocking other HTTP requests. Default timeout set to 4 threads after 60 seconds.
- [#3200](https://projects.tigase.org/issues/3200): Implemented a faster way to close stale connections using MS SQL server, reducing calm down time after large user disconnects.
- [#3203](https://projects.tigase.org/issues/3203): Correct presence status shows for contacts if authorization was accepted while user was offline.
- [#3223](https://projects.tigase.org/issues/3223): +GetUserInfo+ ad-hoc command no longer omits information about local sessions when a remote session is active.
- [#3226](https://projects.tigase.org/issues/3226): Fixed NPE & argument type mismatch in Pubsub.
- [#3245](https://projects.tigase.org/issues/3245): Fixed ClassCastException when Websocket is configured to use SSL.
- [#3249](https://projects.tigase.org/issues/3249): `JabberIQVersion` plugin now returns proper client information when requested from self.
- [#3259](https://projects.tigase.org/issues/3259): Websocket no longer loops when receiving stanzas between 32767 and 65535 bytes in size.
- [#3261](https://projects.tigase.org/issues/3261): Fixed issue with duplicate disco#info responses.
- [#3274](https://projects.tigase.org/issues/3274): NPE when removing roster nickname fixed.
- [#3307](https://projects.tigase.org/issues/3307): Rosters are no longer re-saved when a user logs in and roster is read resulting in a performance boost.
- [#3328](https://projects.tigase.org/issues/3328): Presence processing by PEP plugin optimized.
- [#3336](https://projects.tigase.org/issues/3336): Fixed issues with reloading vhosts in trusted after configuration change.
- [#3337](https://projects.tigase.org/issues/3337): `tls-jdk-nss-bug-workaround-active` is now disabled by default. This fix is disabled by default which may impact older OpenSSL versions that may no longer be supported. You may enable this using an init.properties setting.
- [#3341](https://projects.tigase.org/issues/3341): IQ Packet processing changed for packets sent to bare JID in Cluster mode.
- [#3372](https://projects.tigase.org/issues/3372): Fixed NPE when presence was re-broadcasted to users who did not exit server gracefully.
- [#3374](https://projects.tigase.org/issues/3374): PubSub Schema changed to be more compatible with MS SQL.
- [#3375](https://projects.tigase.org/issues/3375): Users removed VIA REST commands are now disconnected immediately.
- [#3386](https://projects.tigase.org/issues/3386): Fixed AMP logic to avoid querying for (default) Privacy list if user does not exist.
- [#3389](https://projects.tigase.org/issues/3389): Fixed issue of sending packets to connections that were closed, but connection write lock had not been acquired.
- [#3401](https://projects.tigase.org/issues/3401): Multiple issues fixed with Tigase.IM web client.
- [#3422](https://projects.tigase.org/issues/3422): UTC Timestamps now enforced inside cluster_nodes table.
- [#3440](https://projects.tigase.org/issues/3440): Fixed WebSocket error 12030 showing unexpectedly.
- [#3446](https://projects.tigase.org/issues/3446): Fixed Installer configuring MUC incorrectly.
- [#3449](https://projects.tigase.org/issues/3449): Wrapper.conf updated with current library folder for windows Service wrapper.
- [#3453](https://projects.tigase.org/issues/3453): Fixed NPE when using comparator when sorting messages.
- [#3485](https://projects.tigase.org/issues/3485): Fixed JDBCMsgRepository inserting duplicate user JID into table while using AMP.
- [#3489](https://projects.tigase.org/issues/3489): Various fixes to Tigase test suite. Fixed race condition from XMPPSession conflicts when new sessions and closing session events happen at the same time.
- [#3495](https://projects.tigase.org/issues/3495): Fixed messages being duplicated by message carbons.
- [#3499](https://projects.tigase.org/issues/3499): Various fixes to AMP component.
- [#3530](https://projects.tigase.org/issues/3530): Fixed +null cert chain+ error when connecting to other servers using S2S connection with StartTLS.
- [#3550](https://projects.tigase.org/issues/3550): Fixed NPE in sess-man when trying to delete all user information using Pidgin or Psi.
- [#3556](https://projects.tigase.org/issues/3556): JavaDoc updated to include documentation for `xmltools`, `tigase-extras`, and `tigase-util` packages.
- [#3559](https://projects.tigase.org/issues/3559): Fixed Web admin UI not updating Cluster node when it id disconnected.
- [#3579](http://projects.tigase.org/issues/3579): Fixed NPE in SimpleParser.
- [#3580](http://projects.tigase.org/issues/3580): Replaced misleading +feature not implemented+ error when SM attempts to put a packet to processor and queue is full.
- [#3598](https://projects.tigase.org/issues/3598): Fixed error in removing users from blocked list.
- [#3599](https://projects.tigase.org/issues/3599): Fixed `FlexibleOfflineMessages` not being delivered to connection due to lack of explicit connection addressing.
- [#3612](https://projects.tigase.org/issues/3612): Fixed issue when processing packets sent to full JID in cluster mode when user is connected to more than one cluster node at once.
- [#3619](https://projects.tigase.org/issues/3619): Fixed issue with non-presistent contacts being unable to be added to roster.
- [#3649](https://projects.tigase.org/issues/3649): Changed privacy list processing to always allow communication between XMPP connections with the same BareJID.
- [#3655](https://projects.tigase.org/issues/3655): Increased max loop in infinity loop detection logic to 100000 in order to aid larger transfers.
- [#3656](https://projects.tigase.org/issues/3656): Add option to BOSH output command without a timer task to avoid generation of packets to closed connections.
- [#3686](https://projects.tigase.org/issues/3686): XHTML-IM parser has been fixed, restoring [XEP-0071](http://xmpp.org/extensions/xep-0071.html) functionality.
- [#3688](https://projects.tigase.org/issues/3688): Issues with Eventbus in cluster mode fixed.
- [#3689](https://projects.tigase.org/issues/3689): Avoid using sender address when packets are returned from Cluster Manager using stream management.
- [#3717](https://projects.tigase.org/issues/3717): Support added to store messages without \<body/\> element if storage method other than \<body/\> is used. Support also added for JAXMPP to retrieve whole element from Message Archiving instead of only \<body/\>.
- [#3718](https://projects.tigase.org/issues/3718): Removed `DISCONNECTING!` debug stanza from AbstractWebSocketConnector.java that was causing NPE when user fails authentication in WebSocket.
- [#3753](https://projects.tigase.org/issues/3753): Fixed NPE when using Blocking command.
- [#3775](https://projects.tigase.org/issues/3775): Fixed +ThreadExceptionHandler+ error in Tigase mailer.
- [#3781](https://projects.tigase.org/issues/3781): Fixed issue with sending C2S message "The user connection is no longer active".
- [#3800](https://projects.tigase.org/issues/3800): Changed Jenkins to always pull latest binaries from repositories. Windows wrapper changed to use wildcards to load /jars folder.
- [#3848](https://projects.tigase.org/issues/3848): Changes made to JDBCMessageArchiveRepository to fix potential MySQL deadlocks when adding entries to repository.
- [#3902](https://projects.tigase.org/issues/3902): Fixed issue where wss:// connections were closed after 3 minutes of inactivity.
- [#3910](https://projects.tigase.org/issues/3910): Fixed NPE in SessionManager when session was closed during execution of everyMinute method.
- [#3911](https://projects.tigase.org/issues/3911): Fixed load distribution error between threads that could cause high CPU usage.
- [#3931](https://projects.tigase.org/issues/3931): Fixed error caused by AMP running in clustered installations.
- [#3966](https://projects.tigase.org/issues/3966): Changed type of msg & body columns in muc_history table for SQLServer to prevent loss of special characters.
- [#3973](https://projects.tigase.org/issues/3973): Adjusted throttling settings for S2S and cluster connections.
- [#3977](https://projects.tigase.org/issues/3977): Fixed MUC History to reflect messages from JID of room and not JID of original sender.
- [#3984](https://projects.tigase.org/issues/3984): Fixed distinct usage on large data which causes errors on lookup of PubSub nodes in MongoDB.
- [#3970](https://projects.tigase.org/issues/3970): Fixed duplication of messages with AMP payload in cluster mode.
- [#4044](https://projects.tigase.org/issues/4044): Fixed various web installer issues.
- [#4051](https://projects.tigase.org/issues/4051): Fixed NPE in java when processing message with no body.
- [#4052](https://projects.tigase.org/issues/4052): Fixed issue with ClusterRepoItem not properly resulting in `tigase.db.comp.RepositoryChangeListenerIfc.itemUpdated(Item)` being executed.
- [#4056](https://projects.tigase.org/issues/4056): Items removed from cluster repository are not removed from memory correctly.
- [#4071](https://projects.tigase.org/issues/4071): Updated groovy script to properly add owner to node creation VIA ad-hoc command.
- [#4142](https://projects.tigase.org/issues/4142): Updated wrapper.conf file to match tigase.conf default settings.
- [#4183](https://projects.tigase.org/issues/4183): Fixed issue where objects monitored by Ghostbuster.java in MUC could not be removed by it.
- [#4185](https://projects.tigase.org/issues/4185): Fixed issue with `PacketCounter` that caused duplicate messages to be sent on stream resumption.
- [#4188](https://projects.tigase.org/issues/4188): Standardized timestamp between `AbstractMessageArchiveRepository` and `TimestampHelper`.
- [#4262](https://projects.tigase.org/issues/4262): Fixed messages getting lost when StreamResumption is used when a disconnected user reconnects to the server. This issue is also fixed on servers using ACS component.
- [#4298](https://projects.tigase.org/issues/4298): ACS - Fixed messages getting dropped when sent to users offline or on unstable connections.
- [#4365](https://projects.tigase.org/issues/4365) Fixed direct presence not working with non-roster elements using barejid.
- [#4524](https://projects.tigase.org/issues/4524) Fixed NPE during startup causd by Cluster Connection Manager.
- [#4672](https://projects.tigase.org/issues/4672) Fixed `UnsupportedOperationException` error occuring during configuration change of `WebSocketClientConnectionClustered`.
- [#4747](https://projects.tigase.org/issues/4747) converter.groovy script is now depcreciated.
- [#4760](https://projects.tigase.org/issues/4760) Fixed NPE upon anonymous user conection, changed log notification to reduce clutter.
- [#4762](https://projects.tigase.org/issues/4762) Fixed delayed processing of WebSocket data frame if frame is both TCP and ping frame.
- [#4786](https://projects.tigase.org/issues/4786) Changed timeout for cluster repository connection during acceptence of incoming cluster connection from 15ms to 15s.
- [#4787](https://projects.tigase.org/issues/4787) Fixed possible NPE in ClientStateIndication when `GETFEATURES` command is executed before session is confirmed not null.
- [#4793](https://projects.tigase.org/issues/4793) Fixed NPE in statistics occuring when certain values are null.
- [#4812](https://projects.tigase.org/issues/4812) Fixed possible NPE in ClientStateIndication and mobile optimizations if packets are processed before resource binding is finished.
- [#4818](https://projects.tigase.org/issues/4818) Fixed StampComparitor not comparing packets stamped with XEP-0203 Delayed Delivery instructions.
- [#4820](https://projects.tigase.org/issues/4820) Now fixed issue where it was impossible to remove user from all groups.
- Patch added to fix ConcurrentModificationException in BlockingCommand plugin.
- Fixed negation in SASL mechanism selector.
- Fixed checking for user session without localpart in to address.
- Fixed `resourceDefPrefix` from accumulating resource names when components are added or removed in web console.
- Fixed `[F]` property not converting to float.
- Distributed EventBus improved to allow POJO based events to be fired locally.
- Added missing classes to IzPack installer.
- Tigase.xml removed from documentation and default tigase.conf file.
- Logs function added to eventbus publisher operations.
- Fixed responding to same hostname as sender as "to" in stream-error stanza.
- Fixed issue where attempts to delete empty MUC room would create and then destroy room.
- Added startup information to log to indicate when server is ready.
- Fixed a divide by zero error in Java Garbage Collection.
- Fixed `-see-other-host` causing 'Foreign key constraint is incorrectly formed' error while using `SeeOtherHostDB`.
- Fixed NPE occuring when attempting to load from repository that has not been initilized yet.
- Fixed NPE in CounterDataFileLogger by implementing Stringbuilder to create statistics.
- Certificate directories from old or improper installations will now be ignored if empty.

You can download the latest stable version from [distribution section](https://github.com/tigase/tigase-server/releases/tag/tigase-server-7.1.0).

Latest results of Tigase Testsuite are available for [Tigase Testsuite](https://build.tigase.net/tests-results/tts/) and [Tigase TTS-NG](https://build.tigase.net/tests-results/tts-ng/)
