---
title: Fast Docker multi-platform builds
tags:
  - server
  - release
date: "2022-12-21"
description: Using buildx and remote machine to significantly improve build times of multi-platform images
categories:
  - blog
header:
  overlay_image:
---

Nowadays CPUs based on ARM architecture are gaining more and more popularity. Be that in the form of computing units of popular cloud providers or personal computers with the lead of the Apple M1/M2 chipsets.

Docker is a very convenient abstraction layer that allows ease and consistency of deployment of software. However, it requires that the image platform matches the platform on which we want to run the software. Luckily, docker has multi-platform support which allows to both run images from different platform and, via `buildx`, create images for variety of platforms. There is a small downside to it - it uses QEMU to, as name may suggest, emulate other platforms, which entails one downside - performance penalty, both when running the image as well as when creating it.

During recent preparation of Tigase XMPP Server 8.3 release I faced an issue where, due to introduction of `jlink` into our build pipeline to make the images smaller and more lean, additional processing made creation of multi-platform images virtually impossible.

Fortunately, `buildx` tool is very versatile an allows using multiple builders to create images and in addition, those builders can be remote so it's possible to take advantage of computing instances from cloud providers to build images for platforms not native to the machine on which we run the build making the build speed native-like. 

![](/assets/posts/docker-multi-platform-ssh-remote.png)

### Preparing remote environment

There is nothing all that special when it comes to remote machine preparation - it has to have docker installed (follow [Install on Linux](https://docs.docker.com/desktop/install/linux-install/) guide or one dedicated to partiular distribution used). One caveat is to make sure that it's possible to use docker without `sudo` which is easily accheved by adding user to `docker` group: 
```shell
$sudo gpasswd -a $USER docker
```
(restart of the shell session required afterwards).

Machine has to be accessible via `ssh`. Another caveat - because it's not possible to specify key used it has to either be one available via SSH Agent or one of the following files: `id_rsa`, `id_ed25519`, `id_ecdsa`, `id_dsa` or `identity` under `~/.ssh`.

After everything is set up it's possible to check if everything is correct byt executing `info` command: 

```shell
docker -H ssh://<usernane>@<hostname> info
```

which should give output similar to the one below:

```shell
Client:
 Context:    default
 Debug Mode: false
 Plugins:
  buildx: Docker Buildx (Docker Inc., v0.9.1)
  compose: Docker Compose (Docker Inc., v2.12.2)
  dev: Docker Dev Environments (Docker Inc., v0.0.3)
  extension: Manages Docker extensions (Docker Inc., v0.2.13)
  sbom: View the packaged-based Software Bill Of Materials (SBOM) for an image (Anchore Inc., 0.6.0)
  scan: Docker Scan (Docker Inc., v0.21.0)

Server:
 Containers: 2
  Running: 1
  Paused: 0
  Stopped: 1
 Images: 2
 Server Version: 20.10.22
 Storage Driver: overlay2
  Backing Filesystem: extfs
  Supports d_type: true
  Native Overlay Diff: true
  userxattr: false
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Cgroup Version: 1
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
 Swarm: inactive
 Runtimes: io.containerd.runc.v2 io.containerd.runtime.v1.linux runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 78f51771157abb6c9ed224c22013cdf09962315d
 runc version: v1.1.4-0-g5fd4c4d
 init version: de40ad0
 Security Options:
  apparmor
  seccomp
   Profile: default
 Kernel Version: 5.11.0-1022-aws
 Operating System: Ubuntu 20.04.3 LTS
 OSType: linux
 Architecture: x86_64
 CPUs: 2
 Total Memory: 1.901GiB
 Name: ip-172-31-48-98
 ID: X6BF:XU3T:QUBB:M5NY:IGEV:UAQL:6PJ5:GTGO:EBDX:73AS:UD5X:IY5A
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Registry: https://index.docker.io/v1/
 Labels:
 Experimental: false
 Insecure Registries:
  127.0.0.0/8
 Live Restore Enabled: false
```

### Preparing local `buildx` 

The catch here is to create a builder that has two nodes: one local for one architecture and then another one for another architecture.

Let's add new local builder for arm64 platform (I'm running the build on MacBook with M1 chipset):

```shell
docker buildx create --node local --name local-remote-builder --driver docker-container --platform linux/arm64
```

Checking available builders with `docker buildx ls` should give following output
```shell
NAME/NODE            DRIVER/ENDPOINT             STATUS   BUILDKIT PLATFORMS
local-remote-builder docker-container
  local              unix:///var/run/docker.sock inactive          linux/arm64*
```

Let's add remote machine to builds for x86 architecture. The caveat is to use `--append`  parameter to add remote node to just created builder:

```shell
docker buildx create --append --name local-remote-builder --node remote --driver docker-container --platform linux/amd64 ssh://<usernane>@<hostname>
```

Available nodes at this time should include our remote builder:
```shell
$ docker buildx ls
NAME/NODE            DRIVER/ENDPOINT                                                 STATUS   BUILDKIT PLATFORMS
local-remote-builder docker-container
  local              unix:///var/run/docker.sock                                     inactive          linux/arm64*
  remote             ssh://<usernane>@<hostname>                                     inactive          linux/amd64*
```

Builders are still inactive so it's essential to make sure they are properly booted before execution with

```shell
docker buildx inspect --bootstrap --builder local-remote-builder
```

command, yielding following ouput if everything went correctly

```shell
[+] Building 12.2s (2/2) FINISHED
 => [local internal] booting buildkit               2.9s
 => => pulling image moby/buildkit:buildx-stable-1  2.3s
 => => creating container buildx_buildkit_local     0.5s
 => [remote internal] booting buildkit              7.7s
 => => pulling image moby/buildkit:buildx-stable-1  1.1s
 => => creating container buildx_buildkit_remote    6.4s
Name:   local-remote-builder
Driver: docker-container

Nodes:
Name:      local
Endpoint:  unix:///var/run/docker.sock
Status:    running
Buildkit:  v0.10.5
Platforms: linux/arm64*, linux/amd64, linux/amd64/v2, linux/riscv64, linux/ppc64le, linux/s390x, linux/386, linux/mips64le, linux/mips64, linux/arm/v7, linux/arm/v6

Name:      remote
Endpoint:  ssh://<usernane>@<hostname>
Status:    running
Buildkit:  v0.10.5
Platforms: linux/amd64*, linux/amd64/v2, linux/amd64/v3, linux/386
```

The only remaining thing is to set newly created and configured builder to be used by default via

```shell
docker buildx use local-remote-builder
```

and with that our builder list should look like this (nodes listed as `running` and builder annotated with asterisk indicating it's the default one):

```shell
$ docker buildx ls
NAME/NODE              DRIVER/ENDPOINT                                                 STATUS  BUILDKIT PLATFORMS
local-remote-builder * docker-container
  local                unix:///var/run/docker.sock                                     running v0.10.5  linux/arm64*, linux/amd64, linux/amd64/v2, linux/riscv64, linux/ppc64le, linux/s390x, linux/386, linux/mips64le, linux/mips64, linux/arm/v7, linux/arm/v6
  remote               ssh://<usernane>@<hostname>                                     running v0.10.5  linux/amd64*, linux/amd64/v2, linux/amd64/v3, linux/386
```

### Building Tigase XMPP Server image

With everything in place, building latest version of Tigase XMPP Server 8.3 is a matter of executing `buildx build` command with desired target platforms: 

```shell
docker buildx build --platform linux/amd64,linux/arm64  -t tigase/tigase-xmpp-server:${VERSION} -f ${VERSION}/Dockerfile  --no-cache ${VERSION}/
```

giving us image in less than a minute.