---
layout: post
title: Working with Vagrant Chef and XDebug
created: 1375726852
comments: true
---
We have a huge selection of ways to create our web development environment. Beginners can install ready-to-use environments locally like XAMPP, MAMP, WAMP, etc., or you can install the components yourself from source, or via package management systems like Apt, Pacman, and Yum. And then we can follow the operation system's manual to define virtualhosts and create databases for each site we are working on.

The biggest problem is each of the above is time consuming and can make you a big headache because of the difference between the development environment and the live server. It's also a wasted time especially when every developer in the team should do the same boring thing, which could be easily automated. Everybody knows the excuse "it's working for me". What is it? It can be a configuration error, a PHP bug which was fixed on the latest version, file permission error or a missing Apache module. This is ridiculously common and it happens when environments differ by even a wee trivial detail.

The solution these days for the problems is virtualisation. When people think about virtualisation they usually think of performance and memory usage. But stop here for a moment. Is mysql using less memory in a single instance when holds thirty databases for every project you are working on rather than a single database in a virtual machine? Most of my VM's have only 512MB RAM for the full LAMP stack, which could hardly fill a proper development machine with 8-16GB RAM. I even can't see speed problems except a minor 5% overhead of the virtualisation when working with large log files as example.

So how to install Vagrant?

<h3>Step 1: Install Virtualbox</h3>

First <a href="https://www.virtualbox.org/wiki/Downloads">Download VirtualBox</a> and install it. In most Linux systems, it can be done from the official repositories, for example on Ubuntu it's easy as:

{% highlight no-highlight %}
apt-get install virtualbox
{% endhighlight %}

<h3>Step 2: Install Vagrant</h3>

Ubuntu is still supplying a package for Vagrant, but I don't recommend it as it's outdated. Don't worry, Vagrant has a pre-built, up to date package for most systems: http://downloads.vagrantup.com/

If done, fire up a terminal and download a base box which will be used as a base for every virtual machine. I choose the latest LTS version of Ubuntu:

{% highlight no-highlight %}
vagrant box add precise64 http://files.vagrantup.com/precise64.box
{% endhighlight %}

How it works? Vagrant is configuring a virtual machine in Virtualbox and it's creating a synchronised folder inside the virtual machine, so every time you save a file in your text editor, it will be visible from the VM. This directory is your document root. It can serve Magento, Drupal or anything.

<h3>Step 3: Vagrant Up!</h3>

You can follow the Vagrant documentation about how to create a VM and install the required software manually, but the real power of Vagrant is this process can be automated, so when a new developer is joining the project, the only thing He should do is a single command. Which generally does the following
<ul>
<li>installing packages (apt-get install vim)</li>
<li>updating configuration files (/etc/apache2/httpd.conf)</li>
<li>setting up user accounts</li>
<li>running various scripts</li>
</ul>
Download my ready to use configuration to serve Magento and Drupal websites and start the virtual machine. Note this will take 5-10 minutes first time because Chef will install the required software and modules:
{% highlight no-highlight %}
git clone git@github.com:healthywebsites/vagrant-lamp.git
cd vagrant-lamp
vagrant up
{% endhighlight %}

Now you have to add this to your /etc/hosts file:

{% highlight no-highlight %}
127.0.0.1 www.dev-site.com dev-site.com 
{% endhighlight %}

If done, then you will be able to access the site from this url: http://www.dev-site.com:8080/

You can copy this folder to a Magento installation and then you have a working Magento developer environment when you perform "vagrant up" again here. Do not copy the .vagrant folder and index.php indeed.

<h3>Step 4: (Optional) XDebug with Sublime Text</h3>

I'm not a big fan of var_dump in the code, we have a nice tool for debugging in PHP called XDebug, which is well integrated in most IDE and my favourite code editor, Sublime Text has a nice <a href="https://github.com/martomo/SublimeTextXdebug">XDebug integration</a> too. If you have followed the above, you have a working Vagrant instance with XDebug configured. Appreciate this, because I spent almost 5 hours on it from my free time, it was really hard to figure out how to get the host's IP address from inside the VM. Install this Chrome extension, so you can send the right cookie to PHP which turns on XDebug: <a href="https://chrome.google.com/webstore/detail/xdebug-helper/eadndfjplgieldjbigjakmdgkmoaaaoc">XDebug Helper</a>. Or you can install the Firefox equivalent too. Turn it on, then refresh the page:

<img src="https://czettner.com/sites/default/files/leftgallery/xdebughelper.png">

There is already an index.php in your document root, it's content is:

{% highlight php %}
<?php
// Add your breakpoint after the next line to test the debugger
$testvar = "something";
phpinfo();
{% endhighlight %}

Place a breakpoint on the phpinfo(); line and refresh the page. You will get something similar:

<img src="https://czettner.com/sites/default/files/leftgallery/sublime.png">
