---
layout: post
title: Symfony & Symfony reloaded
created: 1283776325
comments: true
---
Mint már korábban írtam, elkezdtem symfony-t tanulni. Kicsit bonyolultnak találtam Kohana után, de rá kellett jönnöm, hogy sokkal jobban skálázható kódot kapok, ha ebben csinálok valamit. Bár még egyik projektem sem tart ott sajnos, de simán át lehet vinni meglévő alkalmazást clusterbe.

Ez mondjuk nem a symfony keretrendszernek köszönhető, de tény, hogy kevésbé engedi elrontani a fejlesztést.

Archlinuxra elérhető a parancssoros verziója is, így a modul generálás csak ennyi:

<code>
$ symfony generate:module frontend content
</code>

A frontend az alkalmazás neve, a content pedig a modul neve. Modul itt lényegében controllert jelent, csak ugye minden máshol van benne, mint kohana-ban.

Nemrég fedeztem fel, hogy készül a Symfony 2 is: http://symfony-reloaded.org/ Az API egyszerűsödött és PHP 5.3 kell neki mindenképpen, erősen épít a névterekre, így még legalább 5 évig nem használható a legtöbb tárhelyszolgáltatónál. Bár PHP4-es tárhelyről is tudok még, nem is értem, hogyan maradhatott fenn.
