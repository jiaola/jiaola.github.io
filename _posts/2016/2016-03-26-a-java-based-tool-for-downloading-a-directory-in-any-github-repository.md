---
layout: "post"
title: "A Java-based Tool for Downloading a Directory in any GitHub Repository"
date: "2016-03-26 23:36"
tags: [github, java]
comments: true
---

During the process of creating a Solr Docker image for Blacklight, I needed to download a [directory that contains config files from the Blacklight GitHub repository](https://github.com/projectblacklight/blacklight/tree/master/solr). It turns out you may use svn to checkout any directory. However, I don't want to install otherwise-unnecessary SVN on my Docker image. There's [a shell script](https://github.com/ojbc/docker/blob/master/java8-karaf3/files/git-download.sh) to download a directory using shell commands but it's buggy and treats everything in the directory as a folder, and everything in the subdirectory as a file.

So I decided to roll my own code to do it. I wrote a Java-based tool since it's going to be used on a Solr Docker image.

<!-- more -->

The package is called GitDownloader. It uses [the GitHub API](https://developer.github.com/v3/) to recursively get the contents of a directory and download them to a local directory. The code is in [a GitHub repository](https://github.com/jiaola/gitdownloader). You may download [release 1.0](https://github.com/jiaola/gitdownloader/releases/tag/1.0) at [https://github.com/jiaola/gitdownloader/releases/download/1.0/gitdownloader-1.0.tar.gz]

To use it, simply run the following from command line. It needs Java 8.

```
java -jar gitdownloader.jar <URL> <DIR>
```

`<URL>` is the github API contents url. For example, The GitHub API URL for the folder https://github.com/jiaola/gitdownloader/tree/master/src/main, would be https://api.github.com/repos/jiaola/gitdownloader/contents/src/main?ref=master

`<DIR>` is the directory where you'd like to download the content into.

> Note that there is a rate limit by GitHub API. One can not request more than 5,000 times over an hour from the same IP. Because GitDownloader recursively request subdirectories and files in a directory, and each subdirectory and file cost a request, you may exceed this rate limit easily, and see an error message.
