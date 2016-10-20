---
layout: "post"
title: "Setup Passenger with Apache2 on Ubuntu 16.04"
date: "2016-10-20 14:27"
---

To set up Passenger with Apache2, do the following:

#### First install the Passenger gem

```
gem install passenger --no-rdoc --no-ri
```

#### Then run the Passenger Apache module installer

```
passenger-install-apache2-module
```

You might see any error that some prerequisites are missing, such as

```
* Checking for Apache 2 development headers...
     Found: no
```

Or

```
* Checking for Apache Portable Runtime (APR) development headers...
     Found: no
* Checking for Apache Portable Runtime Utility (APU) development headers...
     Found: no
```

The easiest solution is to simply install apache2-dev

```
sudo apt-get install apache2-dev
```

At the end of the module installation, you'll see a message like this:

```
--------------------------------------------
Almost there!

Please edit your Apache configuration file, and add these lines:

   LoadModule passenger_module /path/to/your/apache2/mod_passenger.so
   <IfModule mod_passenger.c>
     PassengerRoot /path/to/your/pasenger/gem
     PassengerDefaultRuby /path/to/your/ruby
   </IfModule>

After you restart Apache, you are ready to deploy any number of web
applications on Apache, with a minimum amount of configuration!

Press ENTER when you are done editing.
```

Don't press ENTER yet! Go to apache2 config directory. Create a file `passenger.load` and another file `passenger.conf`. Put the following into the two files:

```
# passenger.load
LoadModule passenger_module /path/to/your/apache2/mod_passenger.so
```

```
# passenger.conf
<IfModule mod_passenger.c>
  PassengerRoot /path/to/your/pasenger/gem
  PassengerDefaultRuby /path/to/your/ruby
</IfModule>
```

Then enable the Passenger module.

```
sudo a2enmod passenger
sudo service apache2 restart
```

Now go back to your passenger installation console, and press ENTER.

All done!

#### After the installation, you may choose to validate to make sure everything is correctly installed.

```
rvmsudo passenger-config validate-install
```

Another useful command is this:

```
rvmsudo passenger-memory-stats
...
---------- Apache processes ----------
PID    PPID   VMSize    Private  Name
--------------------------------------
27916  1      9.1 MB    0.3 MB   /usr/sbin/apache2 -k start
27942  27916  226.4 MB  2.3 MB   /usr/sbin/apache2 -k start
27943  27916  226.4 MB  2.3 MB   /usr/sbin/apache2 -k start
### Processes: 3
### Total private dirty RSS: 4.98 MB
...
```
