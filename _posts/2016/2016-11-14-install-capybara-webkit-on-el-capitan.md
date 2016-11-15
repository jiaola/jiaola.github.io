---
layout: "post"
title: "Install capybara-webkit on El Capitan"
date: "2016-11-14 22:52"
---

I had to jump through hoops to install the capybara-webkit gem on Mac OS X 10.11 (El Capitan). Here are the steps that I finally got to work:

```
brew install qt5 --with-qtwebkit
brew linkapps qt5
brew link --force qt5
gem install capybara-webkit
```
