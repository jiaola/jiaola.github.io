---
layout: "post"
title: "Install Docker on Mac OSX El Capitan"
date: "2016-03-18 14:04"
tags: [docker]
---

[Docker](http://www.docker.com) is an open source technology that allows building, running, testing and
deploying applications inside software containers. You can package your software
in a standardized software development environment and deploy the the software
consistently.

I tested set up docker on my OSX (El Capitan). Here are the steps:

<!-- more -->

### Download and Install Docker

Following the instructions to download and [install docker toolbox](https://docs.docker.com/mac/step_one/).

### Hello World!

After installation, the Docker Quickstart Terminal failed to start as it is instructed on the [installation instructions](https://docs.docker.com/mac/step_one/). However, don't panic. It's not necessary to use the Docker Quickstart Terminal and it's straightforward to simply run the commands from any terminal. I use [iTerm 2](https://www.iterm2.com/).

* First start docker-machine

```
$ docker-machine start default
Starting "default"...
(default) Waiting for an IP...
Machine "default" was started.
Waiting for SSH to be available...
Detecting the provisioner...
Started machines may have new IP addresses. You may need to re-run the `docker-machine env` command.
```

* Set up environment as instructed

```
$ eval $(docker-machine env default)
```

* Now we can load the hello-world image into a container and run it

```
$ docker run hello-world

Hello from Docker.
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker Hub account:
 https://hub.docker.com

For more examples and ideas, visit:
 https://docs.docker.com/userguide/
```

Here is an explanation of the terms: [image and container](https://docs.docker.com/mac/step_two/)

*Note*

_Some errors you might see_

```
$ docker run hello-world
docker: Cannot connect to the Docker daemon. Is the docker daemon running on this host?.
See 'docker run --help'.
(reverse-i-search)`docker-': docker-start
```

This means that there's no daemon running at the moment. To solve the problem, simply start the docker machine and set the environment.
