---
layout: post
title: avi -ból DVD lemez gyártása parancssorból
created: 1275237364
comments: true
---
Valamiért az esküvői videónkat az operatőrtől DivX avi formátumban kaptuk, amit a rokonoknak jobban szerettem volna DVD-ben odaadni. Ki tudja, milyen lejátszójuk van, a régebbiek közül nem mind támogatja a DivX lejátszását.

{% highlight html %}
ffmpeg -i eskuvo.avi -target pal-dvd -ac 2 -aspect 16:9 -sameq eskuvo.mpg -threads 2
{% endhighlight %}

Egy kis magyarázat: a -target pal-dvd paraméter azért volt szükséges, mivel a framerate a videón nem volt megfelelő, így nem lehetett megállípítani, hogy milyen formátum legyen a kimeneten. Az -ac 2 pedig azt jelenti, hogy a 6.1 csatornát 2-re integráljuk. Alapból a studioprogram ilyet adott ki, holott a kamera egyértelműen 1 csatornás. Bár lehet, hogy az aláfestő zene miatt lett 6.1. A -threads 2 kicsit gyorsított a folyamaton, mivel 2 processzorra helyezte az átkódolást. Még mindig maradt 1 mag a böngészésre és email olvasásra.

Következő lépés a VIDEO_TS struktúra létrehozása:
{% highlight html %}
mkdir DVD
dvdauthor --title -f my_dvd_video.mpg -o DVD
dvdauthor -T -o DVD
{% endhighlight %}

Itt lehetett volna fejezeteket is gyártani a templomi jelenetnek és a további részeknek, de ezzel most nem foglalkoztam.

A következő lépés a DVD-re írás:
{% highlight html %}
growisofs -dvd-compat -dvd-video -speed=4 -Z /dev/dvd ./DVD/*
{% endhighlight %}

Még valami: Az esküvő nem most volt, hanem évekkel ezelőtt :)
