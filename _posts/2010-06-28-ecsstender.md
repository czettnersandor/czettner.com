---
layout: post
title: eCSStender
created: 1277712810
comments: true
---
Többször előfordult már, hogy kellemetlenül éreztem magam, amikor böngészőspecifikus CSS szabályokat alkalmaztam. Valójában például a lekerekítéseket képként kellene betölteni a böngészőbe, de létezik egy olyan megoldás, amit ugyan már minden böngésző implementált, de a régebbiek nem jelenítik meg jól. A CSS szabvány szerint így:
{% highlight html %}
.lekerekites {  
  border-radius: 10px;
}  
{% endhighlight %}

Ehhez sajnos hacket kellett használnom, mivel még a legfrissebb firefox sem rajzolja ki a kerekítést:

{% highlight html %}
.lekerekites {  
  border-radius: 10px;
  -moz-border-radius: 10px;
  -webkit-border-radius: 10px;
}  
{% endhighlight %}

Nem tetszik. Átláthatatlan és csúnya. Viszont van megoldás, a napokban fedeztem fel egy rövid javascriptet, ami CSS szabványos kódból készít böngészőspecifikus kódot kliensoldalon. Azt hiszem innentől ezt minden oldalamon használni fogom:

http://ecsstender.org/
