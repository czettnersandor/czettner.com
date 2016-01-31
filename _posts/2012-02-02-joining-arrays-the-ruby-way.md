---
layout: post
title: Joining arrays, the ruby way
created: 1328217262
comments: true
categories: [ruby]
---
Like PHP's join function but in more elegant ruby way. Let's see how can you do it:

{% highlight ruby %}
[1, 2, 3, 4, 5].join(',')
{% endhighlight %}

Or:

{% highlight ruby %}
[1, 2, 3, 4, 5] * ','
{% endhighlight %}
Yes, the multiply operator does the job as well.

Or:

{% highlight ruby %}
(1..5).to_a * ','
{% endhighlight %}

Each time the same result:

{% highlight ruby %}
=> "1,2,3,4,5"
{% endhighlight %}

I never play videogames, but with programming languages in My free time, hope You enjoy reading.
