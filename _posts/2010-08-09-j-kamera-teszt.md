---
layout: post
title: Új kamera - teszt
created: 1281344734
comments: true
---
Hong-Kongból megérkezett két nagyon apró kamera. Kerékpárra és motorra fogom használni. Ma reggel ez nem hagyott dolgozni, felszereltem és kipróbáltam. Itt az eredmény.

A videó egész jó minőségű, elvileg 45 percet bír az akku is, a 8GB-os micro SD ennél jóval többet. flv-be konvertáltam:

{% highlight html %}
ffmpeg -i camtest.avi -ab 56k -ar 22050 -ac 1 -b 2000k -g 160 -cmp 3 -subcmp 3 -mbd 2 -s 480x360 -vcodec flv camtest.flv
{% endhighlight %}

Sajnos a youtube nagyon lerontja a videót:
<object width="480" height="385"><param name="movie" value="http://www.youtube.com/v/LlU5Vad544c&amp;hl=hu_HU&amp;fs=1"></param><param name="allowFullScreen" value="true"></param><param name="allowscriptaccess" value="always"></param><embed src="http://www.youtube.com/v/LlU5Vad544c&amp;hl=hu_HU&amp;fs=1" type="application/x-shockwave-flash" allowscriptaccess="always" allowfullscreen="true" width="480" height="385"></embed></object>
