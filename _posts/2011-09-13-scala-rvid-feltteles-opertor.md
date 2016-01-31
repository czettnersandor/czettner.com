---
layout: post
title: Scala rövid feltételes operátor
created: 1315936496
comments: true
categories: [java]
---
A sok szöveg helyett itt egy példa arról, hogy mire gondolok:

{% highlight html %}
b == true ? x : y;
{% endhighlight %}

Ez több nyelven is értelmezhető, hogy csak néhányat említsek: Java, PHP, C. Egy if .. else feltételt valósít meg rövidebb szintaxissal. De hogy néz ki ez Scala-ban? Sehogy, ugyanis nincs ilyen. Szerintem azért, mert az alap is nagyon jól olvasható, sőt talán jobban is, mint a Java-s rövidített:

{% highlight html %}
if (b == true) x else y
{% endhighlight %}

Néhány karakterrel több, de nem éri meg a jobb olvashatóságért? Tulajdonképpen ez ugyanaz, mint a standard if .. else elágazás, tehát szintaktikailag nincs különbség.
