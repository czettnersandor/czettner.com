---
layout: post
title: Determine if apache process pid is running
created: 1271665283
comments: true
---
One of my small VPS was often running out it's memory. When this occur, some processes terminating. The easiest way to find out if a daemon is running is run ps aux command and grep process name. If you got output along with process name/pid, your process is running.

The pidof command combines the ps aux and the grep command. Here is a sample script what can keeping alive your apache webserver:

{% highlight html %}
#!/bin/bash
RESTART="/etc/init.d/apache2 restart"
PGREP="/usr/bin/pgrep"
HTTPD="apache2"

# find httpd pid
$PGREP ${HTTPD}

if [ $? -ne 0 ] # if apache not running
then
 # restart apache
 $RESTART
fi
{% endhighlight %}

You can define a cron job per minute to run your script:
{% highlight html %}
*/1 * * * * /home/zoner/keepalive.sh >/dev/null 2>&1
{% endhighlight %}
