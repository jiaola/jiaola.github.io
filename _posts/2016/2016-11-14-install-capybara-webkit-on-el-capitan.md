---
layout: "post"
title: "Install capybara-webkit on El Capitan"
date: "2016-11-14 22:52"
---

I had to jump through hoops to install the capybara-webkit gem on Mac OS X 10.11 (El Capitan). But it turned out to be pretty straight-forward. Here are the steps that I finally got to work:

```
brew install qt5 --with-qtwebkit
brew linkapps qt5
brew link --force qt5
gem install capybara-webkit
```

<!-- more -->

If you see a message similar to this, it means that qt wasn't correctly installed. Try remove it and install it again with the commands above.

```
sh: qmake: command not found
*** extconf.rb failed ***
Could not create Makefile due to some reason, probably lack of necessary
libraries and/or headers.  Check the mkmf.log file for more details.  You may
need configuration options.

Provided configuration options:
	--with-opt-dir
	--with-opt-include
	--without-opt-include=${opt-dir}/include
	--with-opt-lib
	--without-opt-lib=${opt-dir}/lib
	--with-make-prog
	--without-make-prog
	--srcdir=.
	--curdir
	--ruby=/Users/djiao/.rvm/rubies/ruby-2.3.1/bin/$(RUBY_BASE_NAME)
	--with-gl-dir
	--without-gl-dir
	--with-gl-include
	--without-gl-include=${gl-dir}/include
	--with-gl-lib
	--without-gl-lib=${gl-dir}/lib
	--with-zlib-dir
	--without-zlib-dir
	--with-zlib-include
	--without-zlib-include=${zlib-dir}/include
	--with-zlib-lib
	--without-zlib-lib=${zlib-dir}/lib
Command 'qmake LIBS\ \+\=\ -L/usr/local/opt/libyaml/lib\ -L/usr/local/opt/readline/lib\ -L/usr/local/opt/libksba/lib\ -L/usr/local/opt/openssl/lib' not available
```
