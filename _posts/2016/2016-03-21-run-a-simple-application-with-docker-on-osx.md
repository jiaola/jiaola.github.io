---
layout: "post"
title: "Run a simple web application in Docker on OSX"
date: "2016-03-21 11:41"
tags: [docker]
comments: true
---

After I [installed docker on OSX](http://wenku.ws/2016/03/18/install-docker-on-mac-osx-el-capitan/), I tried to follow another example in Docker documentation to [run a simple Python Flask web application](https://docs.docker.com/engine/userguide/containers/usingdocker/). The instructions are pretty straightforward. However, after I had the application running with this command

```
$ docker run -d -P training/webapp python app.py
439076d6f1322c6dd1567f32ef7d2104674753403df6e23a57417ea4fc200021
$ docker ps -l
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                     NAMES
439076d6f132        training/webapp     "python app.py"     5 seconds ago       Up 5 seconds        0.0.0.0:32770->5000/tcp   drunk_colden
```

I couldn't get to the page at http://localhost:32770 as promised by the instructions. All I got was nothing but a "This site can't be reached" page. Apparently either the app failed to start or the port wasn't mapped correctly.

<!-- more -->

After some investigation, the problem turned out to be that Docker toolbox on OSX is actually creating a virtual box as the docker host when docker-machine was started and the port was mapped to the virtual box instead of localhost. One can find out the IP of the real docker host (the virtual box) by:

```
$ docker-machine ip
192.168.99.100
```

Therefore, in this example, you can go to http://192.168.99.100:32770 to see the webpage.
