---
layout: post
title: Intelligens legördülő menü
created: 1288348444
comments: true
---
Ugye milyen bosszantó, amikor a legördülő menüből véletlenül kimegy az egér és eltűnik az egész? Ha viszont késleltetjük az eltűnését, akkor a másik legördülő menü fölé navigálva nem reagál időben a menü. Az ideális megoldás az lenne, ha a másik menü fölé navigálva egyből legördülne az és eltűnne a régi. Nézzünk egy nagyon egyszerű és gyors megoldást:

<javascript>
  $('#block-menu-primary-links > .content > ul.menu > li.expanded').hover(
  // mouseover
  function(){
    $('#block-menu-primary-links ul li.expanded').removeClass('over');
    $(this).addClass('over');
    $(this).addClass('hover');
  },
  // mouseout
  function() {
    $(this).removeClass('hover');
    var el = this;
    setTimeout( function(){
      if(!$(el).is('.hover')) $(el).removeClass('over');
    }, 1000);
  }
  );
</javascript>

Az egész menü CSS-e így néz ki:
<code class="css">
#block-menu-primary-links /* "Primary links" block */ {
  background: #A99C6B;
  margin-left: 16px;
  margin-right: 16px;
}
#block-menu-primary-links ul {
  list-style: none;
  padding: 0;
  height: 26px;
}
#block-menu-primary-links ul li {
  list-style: none;
  list-style-image: none;
  display: block;
  float: left;
}
#block-menu-primary-links ul li a {
  color: white;
  text-decoration: none;
  padding: 4px 10px;
  display: block;
  height: 18px;
}
#block-menu-primary-links ul li a.active {
  color: white;
}

/* level 2 */
#block-menu-primary-links ul li ul {
  background: #A99C6B;
  height: auto;
  position: absolute;
  display: none;
}
#block-menu-primary-links ul li.over ul {
  display: block;
}
#block-menu-primary-links ul li ul li {
  float: none;
}
</code>
