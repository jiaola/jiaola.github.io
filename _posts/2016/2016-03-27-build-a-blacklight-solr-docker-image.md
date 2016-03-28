---
layout: "post"
title: "Build a Blacklight Solr Docker Image"
date: "2016-03-27 19:28"
tags: [solr, docker, blacklight]
---

A few days ago, I created a Docker image for the Blacklight app. Even though the image is successfully built, the blacklight website is not functioning since there's no Solr backend. Today I tried to build a Solr image based on the default Solr configuration that shipped with Blacklight.

The process is very straightfoward. Below is the Dockerfile:

<!-- more -->

{% highlight bash linenos %}
FROM solr:5.5

MAINTAINER David Jiao <dazhi.jiao@gmail.com>

USER root

ENV PATH /opt/solr/bin:${PATH}
ENV SOLR_HOME /var/solr/data/
ENV BLACKLIGHT_CORE ${SOLR_HOME}blacklight/

RUN mkdir -p ${BLACKLIGHT_CORE}

ADD solr.xml ${SOLR_HOME}
ADD core.properties ${BLACKLIGHT_CORE}

ADD ./gitdownloader-1.0.jar /opt/solr/gitdownloader-1.0.jar

RUN java -jar /opt/solr/gitdownloader-1.0.jar https://api.github.com/repos/projectblacklight/blacklight/contents/solr/conf ${BLACKLIGHT_CORE}conf

# Add start-up script
ADD ./start.sh /opt/solr/start.sh

VOLUME ["/var/solr"]

WORKDIR /opt/solr
CMD ["sh", "-c", "solr start -f"]
{% highlight %}

The Dockerfile is pretty straightfoward, just a few notes:

* There are some local files are copied over to the image by the `ADD` instruction. You may find these files in the github repo.
* Line 16~18 uses the [GitDownloader](http://wenku.ws/2016/03/26/a-java-based-tool-for-downloading-a-directory-in-any-github-repository/) tool ([GitHub repo](https://github.com/jiaola/gitdownloader)) to download the Blacklight Solr config files from the Blacklight GitHub repository. You may also [use SVN to do the same](http://stackoverflow.com/questions/7106012/download-a-single-folder-or-directory-from-a-github-repo), but you'll have to install SVN on the image.

Next step is to link the Solr container and [the Blacklight container](http://wenku.ws/2016/03/22/create-a-docker-image-for-blacklight/) together to have a fully-functioning application.

After the image is built, we can create a container and tested it with the right IP and port. For example, it'd be `http://192.168.99.100:8983/solr` based on the following commands and output. 

```
$ docker build -t jiaola/solr .
$ docker run -t -d -p 8983:8983 jiaola/solr
$ docker-machine ip
192.168.99.100
```
