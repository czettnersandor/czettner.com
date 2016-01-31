---
layout: post
title: Why there is no finally statement in PHP?
created: 1292016045
comments: true
categories: [php]
---
Put the case that we would like to follow our learned design patterns while working on a well OOP'ed project, but in PHP. We need the finally statement to delete a lockfile in all cases, including an exception. I think it's a good idea to implement this like the above:

{% highlight php %}
$lockfile = "/temp/appname.lock"
try {
  // create the lockfile
  touch($lockfile)
  // some if's, writing to another file.
} catch(Exception $e) {
  // error handling
  echo 'Caught exception: ',  $e->getMessage(), "\n";
} finally {
  // remove the lockfile
  unlink($lockfile);
}
{% endhighlight %}

Today is 2010, one year after the release of the PHP 5.3, we still don't have real OOP feeling when coding in PHP. The feature request of the finally statement is dated to <a href="http://bugs.php.net/bug.php?id=32100">2005</a>, and today, a PHP developer wrote to this request the following:

<quote>The lack of finally causes us some crufty hard to debug code.</quote>

LOL, I can't understand why this language is the most popular in web development field.

UPDATE: finally we have finally statement in PHP 5.5: http://www.czettner.com/blog/13/05/24/new-features-php-55
