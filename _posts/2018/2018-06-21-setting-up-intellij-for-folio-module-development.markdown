---
layout: post
title: Setting up IntelliJ for FOLIO Module development
date: '2018-06-21 14:58'
---

To develop a module in [FOLIO](https://www.folio.org), I used [RMB (Raml module builder)](https://github.com/folio-org/raml-module-builder) to create the module. I was able to build the module with Maven and deploy it with a local [Okapi](https://github.com/folio-org/okapi/) installation.

Here I demonstrate how to set up the module in IntelliJ for development and debugging.

First run the command

```
mvn clean install
```

RMB will generate source codes based on raml files and the generated code is in the target directory. IntelliJ is doing a good job to pick it up so when you try to access the source of the generated classes it'll automatically open the correct files.

<!-- more -->

To set up the Run/Debug configuration, follow the set up in this screen shot.

![Run/Debug configuration for a Folio module](/images/2018/06/Screen Shot 2018-06-21 at 1.26.44 PM.png)

After starting Run/Debug, if a class is recompiled (CMD+Shift+F9 on Mac/CTRL+Shift+F9) or the whole project is rebuilt (CMD+F9/CTRL+F9), the changes will be automatically loaded. However, IntelliJ doesn't automatically compile on save.
