---
layout: "post"
title: "Create a Docker image for Blacklight"
date: "2016-03-22 21:40"
tags: [docker, blacklight]
---

To make a Docker image for Blacklight, I started with [the official Rails image](https://hub.docker.com/_/rails/) on [Docker Hub](https://hub.docker.com/), and followed the steps in [this Docker tutorial](https://docs.docker.com/engine/userguide/containers/dockerimages/).

The Docker image I built is based on Ubuntu:14.04. I also tried to start with the Rails docker image, but there was some issues, which I will explain later. With Ubuntu, I installed Ruby using [RVM](https://rvm.io). I also installed NodeJS and other required packages first, and then installed Rails. I used [the Blacklight generator (aka "Creating a new application the easy way")](https://github.com/projectblacklight/blacklight/wiki/Quickstart) to create a new Blacklight applications. Here's the Dockerfile:

<!-- more -->

{% highlight bash linenos %}
# A Dockerfile for Blacklight
FROM ubuntu:14.04

MAINTAINER David Jiao <djiao@iu.edu>

# Install Ruby and nodejs
RUN apt-get update
RUN apt-get install git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev  python-software-properties libffi-dev liblzma-dev libgmp-dev -y
RUN apt-get install libgdbm-dev libncurses5-dev automake libtool bison libffi-dev -y
RUN gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
RUN curl -L https://get.rvm.io | bash -s stable
RUN /bin/bash -c "source /usr/local/rvm/scripts/rvm"
RUN /bin/bash -l -c "rvm install 2.2.3"
RUN /bin/bash -l -c "rvm use 2.2.3 --default"
RUN curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -
RUN apt-get install -y nodejs

ENV PATH /usr/local/rvm/gems/ruby-2.2.3/bin:/usr/local/rvm/gems/ruby-2.2.3@global/bin:/usr/local/rvm/rubies/ruby-2.2.3/bin:$PATH:/usr/local/rvm/bin

RUN gem install bundler

# # Install Rails
RUN gem install rails

# Download and install blacklight
WORKDIR /opt
RUN rails new search_app -m https://raw.github.com/projectblacklight/blacklight/master/template.demo.rb
WORKDIR /opt/search_app
CMD bundle exec rails s -p 3000 -b '0.0.0.0'

{% endhighlight %}

After the image is built, new containers can be created.

```
$ docker build -t jiaola/blacklight .
$ docker run -t -d -p 3000:3000 jiaola/blacklight
```

There was a few errors when the generator tried to start/stop the jetty server. This is because Java wasn't installed on the image. I just ignored them. 

On my OSX, I can visit the Blacklight site, using [the IP of the Docker virtual box](http://wenku.ws/2016/03/21/run-a-simple-application-with-docker-on-osx/).

Since there is no Solr container, the Blacklight app returns nothing but an error message. Next step I will create a Solr image, and use it as the backend for Blacklight.

![Error message from Blacklight](/public/img/screenshot_2016_03_27_01.png)

> PS, A few notes on the failure to build the Blacklight Docker image based on the Rails image: The error was when running the generator. Here is the Dockerfile:

```bash
# This Dockerfile didn't work.
FROM rails:4.2

# Download and install blacklight
WORKDIR /opt
RUN rails new search_app -m https://raw.github.com/projectblacklight/blacklight/master/template.demo.rb
WORKDIR /opt/search_app
CMD bundle exec rails s -p 3000 -b '0.0.0.0'

```

and the error message was:

```
...
apply  https://raw.github.com/projectblacklight/blacklight/master/template.demo.rb
/usr/local/bundle/gems/railties-4.2.6/lib/rails/generators/actions.rb:26:in `gem': can't modify frozen String (RuntimeError)
...
```

It looks like [a known bug](https://github.com/rails/rails/issues/23137). I am not sure why it's happening and I stopped investigating after I got the image working be manually install Ruby and Rails on an Ubuntu image.
