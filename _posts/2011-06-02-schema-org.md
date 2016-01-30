---
layout: post
title: schema.org
created: 1307041121
comments: true
categories: [seo]
---
A google, a bing és a yahoo összedolgozik. Érdekes dolog ez. A három keresőcég létrehozott egy közös oldalt, a <a href="http://schema.org">schema.org</a>-ot. Felfogható ez úgy is, mint valami segítség a keresőgépektől, hogy milyen html struktúrát várnak el bizonyos tartalmakra a jobb találatok érdekében. Webfejlesztőként fontos ezt áttanulmányozni, mert például megtudható az oldalból, hogy milyen html tagekkel kell ellátni egy eseménynaptárat vagy egy címlistát. A címlista egyik eleme valahogy így néz ki:
<code class="html">
<div itemscope itemtype="http://schema.org/LocalBusiness">
  <h1><span itemprop="name">Beachwalk Beachwear & Giftware</span></h1>
  <span itemprop="description"> A superb collection of fine gifts and clothing
  to accent your stay in Mexico Beach.</span>
  <div itemprop="address" itemscope itemtype="http://schema.org/PostalAddress">
    <span itemprop="streetAddress">3102 Highway 98</span>
    <span itemprop="addressLocality">Mexico Beach</span>,
    <span itemprop="addressRegion">FL</span>
  </div>
  Phone: <span itemprop="telephone">850-648-4200</span>
</div>
</code>

Miért jó ez a keresők számára? Nagyon egyszerű: lényegében áttolják a saját dolgukat a webmesterek vagy fejlesztők felé, tehát elvárják, hogy a tartalmat körülvevő html tagek megmondják, hogy mi van közöttük ahelyett, hogy nekik kelljen azt felismerni. Az nem valószínű, hogy innentől kezdve a Google nem parsolja a telefonszámokat vagy postacímeket, de azt el tudom képzelni, hogy előrébb helyezi a rangsorban, ha egy weblap ilyen plusz információkat tartalmaz és ráadásul az adatok is helyesek. Azaz "Address" attribútum valóban címet jelöl.

A szerkeszteni kifejezéssel ki lehetne kergetni a világból: egyik ügyfelem szekált ezzel a weblapja átadása után, hogy ő nem tudott az admin felületen keresztül bármit szerkeszteni. Az oldal fejlécébe például nem tudott beleírni, nem tudta átméretezni a sidebar-t, megnövelni a menü betűméretét, szóval hasonló kedves kívánságok voltak. A schema.org egyrészt újabb szög azok koporsójába, akik még mindig WYSIWYG szerkesztőkön keresztül "szerkesztik" a saját oldalukat, másrészt kis odafigyeléssel és a megfelelő tudás birtokában előnyre lehet szert tenni a találati listákban.

Még egy valamire jó lesz a schema.org: a SEO szakember felé hivatkozási alap, ha valami hihetetlen blődséget kér tőlem. Ahelyett, hogy utólag kelljen ujjakat törni a látogatók csökkenése miatt, még mielőtt követelné, hogy minden címsor legyen H1 (függetlenül a kontextus mélységétől), szépen megmutatom neki a schema.org ide vonatkozó részét, hogy azzal tudjon vitatkozni, ne engem fárasszon.
