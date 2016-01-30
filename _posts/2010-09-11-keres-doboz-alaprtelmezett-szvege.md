---
layout: post
title: Kereső doboz alapértelmezett szövege
created: 1284217146
comments: true
---
Biztosan mindenki látott már olyan keresődobozt, aminek nincs címkéje, hanem a címke bele van írva a beviteli mezőbe. Ez rendkívül helytakarékos ráadásul ha jól van megcsinálva, a keresőrobotok a "Keresés" szót nem indexelik be. Sok megoldás létezik erre, de a legtöbb nem azt csinálja, amit kell vagy nem működik megfelelően. És egy jó részük óriási javascript vízfej. Nem keresgélek tovább, megírtam a saját megoldásomat egy jQuery plugin formájában. Pár soros, egyszerű, csak annyit tesz, ami a dolga. Itt a forrása:

<code class="javascript">
jQuery.fn.searchDefault = function(message) {
  element = $(this);
  if(element.attr("value") == '' || element.attr("value") == message) {
    element.attr("value", message)
    element.addClass('search-default');
  }
  element.focus(function(){
    if($(this).attr("value") == message) {
      $(this).attr("value", "");
      $(this).removeClass('search-default');
    }
  });
  element.blur(function(){
    if($(this).attr("value") == "") {
      $(this).attr("value", message);
      $(this).addClass('search-default');
    }
  });
}
</code>

Ha az alapértelmezett szöveg van a mezőben, egy "search-default" CSS osztályt tesz a beviteli mezőre, amit tetszőlegesen lehet formázni. Például:

<code class="css">
input.search-default {
  color: #999;
  text-align: center;
}
</code>
