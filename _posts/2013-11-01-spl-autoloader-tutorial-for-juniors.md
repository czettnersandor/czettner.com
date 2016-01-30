---
layout: post
title: SPL autoloader tutorial for juniors
created: 1383333176
comments: true
categories: [php]
---
The Standard PHP Library (SPL) is a collection of interfaces and classes that are meant to solve common problems. One of a most common problem in PHP is when you have a complex application and somehow you have to manage to load a lots of PHP class files. In a small application you simply do the following to make a class available for PHP:

<code class="php">
require_once('include/mailer.class.php');
require_once('include/addressbook.class.php');

$mailer = new Mailer();
$addresses = new Addressbook();
</code>

But if you have thousands of classes you can't simply do this. You don't want to add require_once() calls on the top of your every PHP files which you think it will probably use that class. Additionally, there is a performance overhead calling reuire_once() or include_once() or require() or include(). You probably want to implement a fallback model as well to make your application more flexible allowing other developers to override your code. Imagine a third party developer who wants to modify your include/mailer.class.php file in order to change the email sending method to use SMTP instead of sendmail. In this case, the Third party developer will need to add a new file into lib/thirpartycompany/mailer.class.php and modify every single require_once() call to include his class instead of the core application class. This is not a good application design and hard to maintain or track the future software updates.

You may have a global includes.php file somewhere where you execute require_once() on every single php class so every class would be available on execution time. But then it will cost a fortune on perfomance side to load those classes on every run.

The solution for this problem is in the PHP's SPL. SPL is a library which is compiled into PHP by default since PHP 5.3.0 and from this version it can no longer be disabled. We will see how SPL autoloader works with an easy example library which is supposed to be used to send emails.

In order to register an autoloader function we have to create a function first. This function can be a global function, but in this example I created a class for this purpose and defined a static method in this class. The only file you have to include in runtime is the php file which is registering this autoloader function. In our example is code/bootstrap.php. The skeleton looks like this:

code/core/autoloader.php:
<code class="php">
<?php
class Autoloader
{
    public static function loader($class)
    {
    }
}
</code>

code/bootstrap.php:
<code class="php">
<?php
require_once('core/autoloader.php');

spl_autoload_register('Autoloader::loader');
</code>

code/bootstrap.php is registering the loader static method from the Autoloader class. This Autoloader class has never been instantiated and I used a class only because our application will be object orientated and thus it's a good idea to make everything a class.

Now let's create the application logic by implementing the classes required to send emails:

code/core/mailer.php:
<code class="php">
<?php
class Mailer
{
    private $_recipients;
    public function addRecipient($email)
    {
        $recipient = new Addressbook($email);
        $this->_recipients[] = $recipient;
    }
    public function send($message)
    {
        foreach ($this->_recipients as $recipient) {
            mail($recipient->getEmail(), 'Alert', $message);
            echo sprintf("Mail sent to %s", $recipient->getEmail());
        }
    }
}
</code>

code/core/addressbook.php:
<code class="php">
<?php

class Addressbook
{
    private $_email;
    private $_name;

    public function __construct($email, $name = null)
    {
        $this->_email = $email;
        $this->_name = $name;
    }

    public function getEmail()
    {
        return $this->_email;
    }

    public function getName()
    {
        // TODO: implement
    }
}
</code>

As you can see, I'm using the Addressbook class from code/core/mailer.php without including the php file which contains this class. It's only possible if the autoloader is including it in execution time. SPL autoloader works the following:

<ul>
<li>it's trying to load the class</li>
<li>If class does not exist, then calling the first registered autoloader function with the class name</li>
<li>If the first autoloader returns with false, then it's calling the second autoloader function</li>
<li>And so on</li>
<li>If class still not found, it's dropping a Fatal error: Class '...' not found...</li>
</ul>

Now let's implement the autoloader function in code/core/autoloader.php:

<code class="php">
    public static function loader($class)
    {
        $filename = strtolower($class) . '.php';
        $file ='code/core/' . $filename;
        if (!file_exists($file))
        {
            return false;
        }
        include $file;
    }
</code>

Let's try to use it from send.php:

<code class="php">
<?php
include "code/bootstrap.php";

$mailer = new Mailer();

$mailer->addRecipient('test@example.com');
$mailer->addRecipient('foo@example.com');
$mailer->addRecipient('bar@example.com');

$mailer->send('This message has been sent to 3 recipients');
</code>

So how it works?

The only file I include from the top of my script is the bootstrap.php. bootstrap.php is registering the autoloader. When I call the Mailer class the autoloader is trying to load mailer.php from code/core directory. Autoloader is including the required file. When I call addRecipient() then the Mailer class would need the Addressbook class, which will trigger the autoloader function to run successfully and the Addressbook class is loaded and available from this point. As you can see, the various classes are loaded in runtime without having to deal with the require_once() function calls.

By registering an other autoloader before the Autoloader class it's possible to look into the local folder first, but for performance reasons it would be better to add this functionality to the current class.

Every serious PHP framework is using autoloader, so it's essential to know how it's working and it's useful to write your own autoloader to make sure you understood the basic principle of autoloading classes. We at Healthy Websites are using Magento with Zend Framework and Varien_Autoloader, which works very similar than the autoloader example above. The code is part of a learning program of our junior developers.

The code is available at: https://github.com/czettnersandor/spl_autoloader_tutorial
