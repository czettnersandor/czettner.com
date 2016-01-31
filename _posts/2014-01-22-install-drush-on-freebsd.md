---
layout: post
title: Install Drush on FreeBSD
created: 1390413966
comments: true
---
Drush is a widely used command line management tool for Drupal installations. It can quickly clear the cache, enable / disable modules. I recommend it to any developer who want to get his job done faster. One of it's advantage is the ability to automate. Like during automated tests or deployment scripts.

To install it on FreeBSD, follow these simple commands:

{% highlight html %}
wget https://github.com/drush-ops/drush/archive/master.zip --no-check-certificate
unzip master.zip -d /usr/local
mv /usr/local/drush-master /usr/local/drush
ln -s /usr/local/drush/drush /usr/local/bin/drush
{% endhighlight %}

The required libraries will be downloaded at first run, so don't forget to run drush as root after you downloaded it:

{% highlight html %}
drush
{% endhighlight %}
