---
layout: post
title: Még több PHP gyorsítás
created: 1295089910
comments: true
categories: [php]
---
Nagy látogatottságú Drupal oldalt kell kiszolgálni és nem bírja a szerver? Nem meglepő, hiszen Drupal esetén a MyISAM táblákat nem nagy terhelésre tervezték. De tegyük fel, hogy sikerült az adatbázist InnoDB alapokra helyezni, most már csak egy szűk keresztmetszet van: a spagetti módjára tekeredő template függvények. Szerencsére létezik erre is megoldás, hiszen a PHP egy nagyon népszerű nyelv. Debian Lenny alatt nem is kell mást tennünk, mint telepíteni a php-apc nevű csomagot és újraindítani az apache-ot. Még csak nem is kell teljesen, elég neki a graceful:
<code>
apt-get install php-apc
apachectl graceful
</code>

A felsofokon.hu esetén a php processzek terhelése kb a felére esett vissza, a load pedig 5-ről 2 környékén áll meg. Figyelni kell az apc.php-t is, ami így telepíthető:
<code>
gzip -dc /usr/share/doc/php-apc/apc.php.gz > /path/to/apc.php
</code>
a /path/to  helyére egy jelszóval védett könyvtárat kell írni. Csatolom a figyelhető képernyőt, ideális esetben minél több cache kihasználás várható, a config fájl az /etc/php/conf.d/apc.ini útvonalon található. Itt kell változtatni, ha kevésnek látszana a cache méret vagy a TTL.
