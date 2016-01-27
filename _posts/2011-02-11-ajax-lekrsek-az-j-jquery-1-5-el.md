---
layout: post
title: jQuerz AJAX
created: 1297444290
comments: true
categories: [javascript, jquery]
---
Könnyíti a kód átláthatóságát az 1.5-ös jQuery új képessége, ami az AJAX lekérések kezelését hivatott elvégezni. Ilyen egyszerű, már el is felejtettem, hogy régen ez hogyan működött:

<code class="javascript">
var jax = $.ajax({
  url: '/valami/url'
})

jax.success(function() {
  alert("Működik!");
});
</code>

Mint látható, a jax változó egy jQuery objektumot kap értékül és az objektum egyik attribútumát később állítjuk be. Rendszerint rögtön az értékadás után. Miért jó ez? Csökkenti az egymásba ágyazott blokkok számát és a logikailag egyébként is egymástól elkülönülő lekérést és feldolgozást különválasztja. Azt nem is említve, hogy akár egy változó figyelésétől függően különböző feldolgozó blokkokat rendelhetünk hozzá az AJAX művelethez. Például akkor, amikor az url végén van egy .json, egy JSON feldolgozót használhatunk, az összes többi esetén pedig csak egyszerűen beillesztjük valahova a DOM fába a lekért tartalmat.
