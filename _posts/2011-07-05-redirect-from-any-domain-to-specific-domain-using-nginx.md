---
layout: post
title: Redirect from any domain to specific domain using nginx
created: 1309849524
comments: true
categories: [seo, nginx]
---
Let’s say you want to make a 301 redirect from any domain (for example: www, dev, w3) of your website to direct access via the non-sub-domain url. Nginx makes it easy to do.

Just make your own wildcard server_name directive and add this to your server{} block:

{% highlight html %}
if ($host != 'your_domain.com' ) {
  rewrite  ^/(.*)$  http://your_domain.com/$1  permanent;
}
{% endhighlight %}
