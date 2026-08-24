---
title: Using STUN & TURN server with Tigase XMPP Server with XEP-0215 (External Service Discovery) 
tags:
  - server
  - installation
  - stun
  - turn
  - audio-video
  - calls
  - VoIP
date: "2020-09-15"
description: Setting STUN and TURN server with Tigase XMPP Server is extremely easy and takes only a couple of minutes.
categories:
  - blog
header:
  overlay_image: /assets/posts/web-admin-add-new-stun-item.png
---
Setting STUN and TURN server with Tigase XMPP Server is extremely easy and takes only a couple of minutes.

Communication with your family and friends is not only about instant chats. Audio and Video calls are quite important and sometimes, under unfavourable network configurations establishing a call may prove difficult. Luckily, with the help of [STUN (Session Traversal Utilities for NAT)](https://en.wikipedia.org/wiki/STUN) and [TURN (Traversal Using Relays around NAT )](https://en.wikipedia.org/wiki/Traversal_Using_Relays_around_NAT) servers it's no longer a problem

In the following guide we will show how to setup TURN and STUN servers with Tigase XMPP Server, so that compatible XMPP clients will be able to use them. Our [xmpp.cloud installation](https://xmpp.cloud) supports not only them, but also [XMPP MIX](https://tigase.net/tigase-im-mix/)

## Assumptions
We are assuming that you have installed your preferred TURN server and created an account on the TURN server for use by your XMPP server users and that you have installed and configured Tigase XMPP Server.

At the end of the article there is a short guide how to quickly setup CoTURN server.

## Enabling external service discovery

> **_NOTE_**: It is required **only** for Tigase XMPP Server **8.1.0 and earlier**

First you need to edit `etc/config.tdsl` file and:
1. Add following line in the main section of the file:
```
'ext-disco' () {}
```
2. Add following line in the `sess-man` section of the file:
```
'urn:xmpp:extdisco:2' () {}
```

so that your config file would look like this:
```
'ext-disco' () {}
'sess-man' () {
    'urn:xmpp:extdisco:2' () {}
}
```

## Start Tigase XMPP Server
After applying changes mentioned above, you need to start Tigase XMPP Server or, in case if it was running, restart it.

## Open Admin UI
Open web browser and head to `http://<your-xmpp-server-and-port>/admin/` (for example: [https://localhost:8080](https://localhost:8080)). When promped, log in by providing admin user credentials: bare JID (i.e.: `user@domain`) as the user and related password. Afterwards you'll see main Web AdminUI screen:

![]({{ site.url }}{{ site.baseurl }}/assets/posts/web-admin-main-page.png)

and on that screen open **Configuration** group on the left by clicking on it.

## Add external TURN service
After opening **Configuration** group (`1`) click on **Add New Item** (`2`) position which has **ext-disco@…** in its subtitle.

In the opened form you need to provide following detail:
![]({{ site.url }}{{ site.baseurl }}/assets/posts/web-admin-add-new-turn-item.png)

* **Service** - ID of the service which will be used for identification by Tigase XMPP Server *(eg. `turn@example.com`)*
* **Service name** - name of the service which may be presented to the user *(eg. `TURN server`)*
* **Host** - fully qualified domain name of the TURN server or its IP address *(eg. `turn.example.com`)*
* **Port** - port at which TURN server listens *(eg. `3478`)*
* **Type** - type of the server, enter `turn`
* **Transport** - type of transport used for communication with the server `udp` or `tcp` *(usually `udp` but item can be added for both)*
* **Requires username and password** - for notifying XMPP client that this service requires its username and password for XMPP service *(leave unchecked)*
* **Username** - username required for authentication for TURN server *(ie. `turn-user`)*
* **Password** - password required for authentication for TURN server *(ie. `turn-password`)* 

After filling out the form, press `Submit` button (`3`) to send form and add a TURN server to external services for your server. Admin UI will confirm that service was added with the following result
![]({{ site.url }}{{ site.baseurl }}/assets/posts/web-admin-add-new-item-confirmation.png)

## Add external STUN service
While adding a TURN server is usually all what you need, in some cases you may want to allow your users to use also STUN. Steps are quite similar like on TURN server - after opening **Configuration** group (`1`) click on **Add New Item** (`2`) position which has **ext-disco@…** in its subtitle and in the opened form you need to provide following detail:
![]({{ site.url }}{{ site.baseurl }}/assets/posts/web-admin-add-new-stun-item.png)

* **Service** - ID of the service which will be used for identification by Tigase XMPP Server *(ie. `stun@example.com`)*
* **Service name** - name of the service which may be presented to the user *(eg. `STUN server`)*
* **Host** - fully qualified domain name of the STUN server or its IP address *(eg. `stun.example.com`)*
* **Port** - port at which TURN server listens *(eg. `3478`)*
* **Type** - type of the server, enter `stun`
* **Transport** - type of transport used for communication with the server `udp` or `tcp` *(usually `udp` but item can be added for both)*
* **Requires username and password** - for notifying XMPP client that this service requires its username and password for XMPP service *(leave unchecked)*
* **Username** - username required for authentication for STUN server *(if required)*
* **Password** - password required for authentication for STUN server *(if required)* 

### Note
If you are using the same server for STUN and TURN (you usually will as TURN servers usually contain STUN functionality) you will fill the following form with almost the same details *(only use different **Service** field value,  **Type** will be `stun` and most likely you will skip passing **Username** and **Password** - leaving them empty, the rest of the field values will be the same).

After filling out the form, press `Submit` button (`3`) to send form and add a STUN server to external services for your server. Admin UI will confirm that service was added with the following result
![]({{ site.url }}{{ site.baseurl }}/assets/posts/web-admin-add-new-item-confirmation.png)

## And now what?
Now you have fully configured your STUN/TURN server for usage with Tigase XMPP Server allowing XMPP clients connected to your server and compatible with [XEP-0215: External Service Discovery](https://xmpp.org/extensions/xep-0215.html) to take full advantage of your STUN/TURN server ie. by providing better VoIP experience.

## CoTURN installation

You can quickly setup CoTURN server using Docker. Please follow Docker installation on your operating system and then install CoTURN using [Docker Hub](https://hub.docker.com/r/instrumentisto/coturn) (instrumentisto/coturn). The bare minimum required to run it looks like that (please update `realm` with your domain and `external-ip` with IP on which server should be accessible):
````
sudo docker run \
    --name coturn \
    -p 3478:3478 \
    -p 3478:3478/udp \
    -p 5349:5349 \
    -p 5349:5349/udp \
    -p 49160-49200:49160-49200 \
    coturn/coturn \
    --log-file=stdout \
    --min-port=49160 \
    --max-port=49200 \
    --realm localhost \
    --user tigase:tigase \
    --lt-cred-mech \
    --fingerprint \
    --external-ip=$$(detect-external-ip)

````

> _**NOTE**_: It uses `tigase` as username/password and `localhost` as realm - please adjust if needed 

### Tigase XMPP Server and CoTURN in Docker Compose

Alternatively, you can use Docker Compose to quickly spin up complete Tigase XMPP Server with CoTURN configured - see our [Docker Compose guide](https://tigase.dev/tigase/_server/tigase-server/~files/master/src/main/docker/README.md#docker-compose) for details.