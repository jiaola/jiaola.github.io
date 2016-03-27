---
layout: "post"
title: "Create a Docker image for Blacklight"
date: "2016-03-22 21:40"
tags: [docker, blacklight]
---

To make a Docker image for Blacklight, I started with [the official Rails image](https://hub.docker.com/_/rails/) on [Docker Hub](https://hub.docker.com/), and followed the steps in [this Docker tutorial](https://docs.docker.com/engine/userguide/containers/dockerimages/).

Here's the Dockerfile:

{% highlight shell linenos %}
# A Dockerfile for Blacklight
FROM ubuntu:14.04

MAINTAINER David Jiao <djiao@iu.edu>

# Install JDK 8
RUN apt-get update
RUN apt-get install software-properties-common -y
RUN add-apt-repository ppa:webupd8team/java -y
RUN apt-get update
RUN echo debconf shared/accepted-oracle-license-v1-1 select true | debconf-set-selections
RUN apt-get install oracle-java8-installer -y
RUN apt-get install oracle-java8-set-default

# Install Ruby
RUN apt-get install git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev  python-software-properties libffi-dev -y
RUN apt-get install libgdbm-dev libncurses5-dev automake libtool bison libffi-dev -y
RUN gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
RUN curl -L https://get.rvm.io | bash -s stable
RUN /bin/bash -c "source /usr/local/rvm/scripts/rvm"
RUN /bin/bash -l -c "rvm install 2.2.3"
RUN /bin/bash -l -c "rvm use 2.2.3 --default"
RUN /bin/bash -l -c "gem install bundler"
RUN curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -
RUN apt-get install -y nodejs

# Install Rails
RUN apt-get install liblzma-dev libgmp-dev -y
RUN /bin/bash -l -c "gem install rails"
RUN /bin/bash -l -c "rails -v"

# Install sqlite
RUN apt-get install sqlite3 libsqlite3-dev
{% endhighlight %}
