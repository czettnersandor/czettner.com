---
layout: post
title: How to install DoctrineMigrationsBundle using composer in Symfony 2.3
created: 1370029277
comments: true
categories: [php]
---
DoctrineMigrationsBundle offers a safe way to migrate to newer versions of your database schema when deploying an application. It also help keeping track of your changes. Like git for the source code, but this is for database. The <a href="http://symfony.com/doc/master/bundles/DoctrineMigrationsBundle/index.html">official documentation</a> of DoctrineMigrationsBundle says in order to install the bundle to your application with composer, simply add this line to composer.json:

{% highlight json %}
{
    "require": {
        "doctrine/doctrine-migrations-bundle": "dev-master"
    }
}
{% endhighlight %}

I have tried the code above but it failed with this error message:

{% highlight no-highlight %}
composer update 
Loading composer repositories with package information
Updating dependencies (including require-dev)
Your requirements could not be resolved to an installable set of packages.

  Problem 1
    - Installation request for doctrine/doctrine-migrations-bundle dev-master -> satisfiable by doctrine/doctrine-migrations-bundle[dev-master].
    - doctrine/doctrine-migrations-bundle dev-master requires doctrine/migrations * -> no matching package found.

Potential causes:
 - A typo in the package name
 - The package is not available in a stable-enough version according to your minimum-stability setting
   see <https://groups.google.com/d/topic/composer-dev/_g3ASeIFlrc/discussion> for more details.

Read <http://getcomposer.org/doc/articles/troubleshooting.md> for further common problems.
{% endhighlight %}

The problem is, DoctrineMigrationsBundle depends on MigrationsBundle and the dependency cannot be satisfied because it's not in the composer.json. So let's add it then, this two lines should be added to the composer.json into the require section:

{% highlight html %}
        "doctrine/migrations": "dev-master",
        "doctrine/doctrine-migrations-bundle": "dev-master"
{% endhighlight %}

Then a php composer.phar update command should install the required dependencies as well.

Additionally, app/AppKernel.php should be changed too with:

{% highlight php %}
  // app/AppKernel.php
public function registerBundles()
{
    $bundles = array(
        //...
        new Doctrine\Bundle\MigrationsBundle\DoctrineMigrationsBundle(),
    );
}
{% endhighlight %}
