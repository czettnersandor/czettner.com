---
layout: post
title: Munin mysql fix for Ubuntu 12.04
created: 1379532960
comments: true
---
Munin is an open source server monitoring software and it's a must have for every system administrator. On Ubuntu, it's very easy to install:

<code class="no-highlight">
sudo apt-get install munin
</code>

By default, it's showing just the required graphs, but missing mysql. The error message is the following:

<code class="no-highlight">
Missing dependency Cache::Cache at /usr/share/munin/plugins/mysql_ line 716.
</code>

This is due to a missing perl library and usually we relalise this after we installed it. This is a quick fix for the problem:

<code class="no-highlight">
sudo su -
apt-get install libcache-cache-perl
munin-node-configure --suggest --shell | sh
/etc/init.d/munin-node restart
</code>
