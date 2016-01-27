---
layout: post
title: Magento trailing slash eltávolítása
created: 1292340230
comments: true
categories: [magento]
---
Előfordulhat, hogy a webáruházunkra mások záró perjellel hivatkoznak, miközben mi mindenhol záró perjel (trailing slash) nélkül hivatkozunk. Ez nem jó, mivel a Google duplikátumnak fogja venni a slash-ra végződő URL-t.

Ha viszont minden záró perjelet eltávolítunk, az admin felületen és a fizetési folyamatban problémákba ütközünk, mivel a POST kérések az átirányított oldalakra már nem lesznek érvényesek és sajnos a Magento záró perjellel hívja meg ezeket az URL-eket. Az alábbi kód segít helyrerakni a problémát, a problémás részeket meghagyja úgy, ahogy azt eredetileg meghívták.

<code>
#remove trailing slashes
    RewriteCond %{HTTP_HOST} !^\.kodakmedia\.hu$ [NC]
    RewriteCond %{THE_REQUEST} !^[A-Z]+\ /checkout
    RewriteCond %{THE_REQUEST} !^[A-Z]+\ /customer
    RewriteCond %{THE_REQUEST} !^[A-Z]+\ /admin
    RewriteCond %{THE_REQUEST} !^[A-Z]+\ /index.php
    RewriteCond %{THE_REQUEST} !^[A-Z]+\ /wishlist
    RewriteCond %{THE_REQUEST} !^[A-Z]+\ /newsletter
    RewriteCond %{THE_REQUEST} !^[A-Z]+\ /catalogsearch
    RewriteRule ^(.+)/$ http://%{HTTP_HOST}/$1 [R=301,L]
</code>

A <a href="http://kodakmedia.hu">kodakmedia.hu</a> lecserélendő a saját domainre a helyes működéshez. Ez egyébként ki is hagyható, de terhelés szempontjából jobb, mivel a feltétel futása ezen a ponton megszakad, ha a főoldalt töltöttük be.
