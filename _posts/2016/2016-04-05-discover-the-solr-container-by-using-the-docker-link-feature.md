---
layout: "post"
title: "Discover the Solr Container by Using the Docker Link Feature"
date: "2016-04-05 07:01"
tags: [docker, blacklight, solr]
---

As I stated earlier, the issue with [the Blacklight image I built earlier](http://wenku.ws/2016/03/22/create-a-docker-image-for-blacklight/) is the lack of Solr  in the backend. Now that [I had created the Solr image](http://wenku.ws/2016/03/27/build-a-blacklight-solr-docker-image/), I tried to let the Blacklight container discover the Solr container by using [the Docker link feature](https://docs.docker.com/engine/userguide/networking/default_network/dockerlinks/#connect-with-the-linking-system).

First I made [a small change to the Blacklight Dockerfile](https://github.com/jiaola/blacklight-docker/commit/c4792a93eaa24e4a9a862facf8bb1f58c66484a7#diff-9e9411e24c36b7e8c796ee9a4926ad0f). It sets [the Blacklight `SOLR_URL` environment variable](https://github.com/projectblacklight/blacklight/wiki/Solr-Configuration) that's going to be used by Blacklight to find Solr.

```
ENV SOLR_URL http://solr:8983/solr/blacklight
```

Here are the steps to start the containers and link them from command line:

<!-- more -->

```bash
$ # Start the solr container
$ docker run --name app_solr -d -p 8983:8983 jiaola/blacklight-solr

$ # Start the blacklight container and link the solr container
$ docker run --name app_blacklight -d -p 3000:3000 --link app_solr:solr jiaola/blacklight

$ # Index sample data
$ docker exec app_blacklight rake solr:marc:index_test_data
```

A few notes:

* In `SOLR_URL`, the alias `solr` in the URL `http://solr.8983` needs to be the same as the alias used in the `--link` flag in the command. For example, the container id is `app_solr` in my example above, and the alias is `solr` as in `--link app_solr:solr`. The `--link` flag also takes the form `--link <container name or id>`, in which case the alias is automatically set to the container name or id. Just make sure the same is used in `SOLR_URL`.
* In the last command, it uses the [`docker exec` command line tool](https://docs.docker.com/engine/reference/commandline/exec/). Note that the container name (`app_blacklight` in this example) has to match the name used in the `--name` flag when starting the container.

Open the url http://localhost:3000 (Or if you are on a Mac like me, [use the IP of the virtual box instead of localhost](http://wenku.ws/2016/03/21/run-a-simple-application-with-docker-on-osx/)). Voila, the blacklight website with the testing data is loaded!

![Blacklight default look](/public/img/screenshot_2016_04_05_01.png)
