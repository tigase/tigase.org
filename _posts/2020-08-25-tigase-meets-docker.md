---
title: Tigase XMPP Server meets Docker 
tags:
  - server
  - distribution
  - docker
  - dockerhub
date: "2020-08-25"
description: Tigase XMPP Server is finally available as Docker image and you can grab it from DockerHub - setting the XMPP server was never easier
categories:
  - blog
header:
  overlay_image: /assets/posts/docker_logo.png
---
Tigase XMPP Server is finally available as Docker image and you can grab it from DockerHub - setting the XMPP server 
was never easier.

Running Tigase XMPP Server was never easier - you can have a full-fledged XMPP server in a matter of minutes.

# Benefits of Tigase XMPP Server docker image

Using containers offers various benefits - it helps bundle complete execution environment that's consistent, isolates various services and orchestrates them with ease. This makes setting up new service a breeze. At the same time Docker is only a thin layer with very little performance overhead.
In Tigase's case, even though normally only JVM is required, having single bundle with recommended version of the JVM and configured environment helps achieve the most compatible and stable setup.

# How to start

If you haven't already, [install Docker engine](https://docs.docker.com/engine/install/) on your desired operating system. Once this is done, starting Tigase is just two commands away (for up-to-date list of tags check out [our DockerHub](https://hub.docker.com/r/tigase/tigase-xmpp-server), by default `latest` is used):

```
$ docker pull tigase/tigase-xmpp-server
$ docker run --name tigase-server -p 8080:8080 -p 5222:5222 tigase/tigase-xmpp-server
``` 

And after a short Tigase will start and you'll be presented with option to setup the server by accessing http://localhost:8080 page. Once setup is completed, simply restart the container with `$ docker restart tigase-server` and connect your client.

![docker setup]({{ site.url }}{{ site.baseurl }}/assets/posts/docker-setup.png)

# More information

Of course above is the simplest deployment. There are many possibilities to adjust the container by mounting local volumes, exposing more ports or connecting to external database. It's even possible to run local Tigase cluster! For details please check out [Tigase in Docker guide](https://github.com/tigase/tigase-xmpp-server-docker#configuration)

# Java in Docker - will it work?

Our images are based on Java 11, which already supports Docker without any issues. 