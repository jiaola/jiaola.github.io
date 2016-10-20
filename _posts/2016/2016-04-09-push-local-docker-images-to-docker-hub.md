---
layout: "post"
title: "Push Local Docker Images to Docker Hub"
date: "2016-04-09 23:47"
tags: [docker]
comments: true
---

After the Docker images for [Blacklight](https://github.com/jiaola/blacklight-docker/tree/master/images/blacklight) and [Solr](https://github.com/jiaola/blacklight-docker/tree/master/images/solr) are created, they need to be pushed to [Docker Hub](https://hub.docker.com/) so others can use them.

Here's [how](https://docs.docker.com/mac/step_six/):

```
$ docker login --username=jiaola --email=dazhi.jiao@gmail.com
$ docker push jiaola/blacklight
$ docker push jiaola/blacklight-solr
```

Now if anyone else wants to use the images, just use the pull command:

```
$ docker pull jiaola/blacklight
$ docker pull jiaola/blacklight-solr
```
