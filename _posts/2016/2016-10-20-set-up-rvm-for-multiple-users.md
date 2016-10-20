---
layout: "post"
title: "Set up RVM for multiple users"
date: "2016-10-20 13:38"
---

[RVM (Ruby Version Manager)][b49e701c], is a command-line tool to manage the installation of Ruby. It makes working with multiple Ruby versions really easy, and it allows an application level of "set of gems".

  [b49e701c]: https://rvm.io/ "rvm.io"

To set up RVM for multiple users on a linux system, do the following:

```
\curl -sSL https://get.rvm.io | sudo bash -s stable
```

Then add users to the group `rvm`.

```
sudo usermod -a -G rvm username
```

Each user that has been added to the `rvm` group needs to logout and login again, and run the following to be able to use rvm.

```
source /etc/profile.d/rvm.sh
```

That's it! Now you don't need to `sudo` any rvm command. For example, you can run the following to install Ruby 2.3.0.

```
rvm install 2.3.0
```

It actually would be installed in `/usr/local/rvm/src/ruby-2.3.0`.
