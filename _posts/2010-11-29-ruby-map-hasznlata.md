---
layout: post
title: ruby - map használata
created: 1291063548
comments: true
categories: [ruby]
---
Talán másoknak is hasznára válik ez az apró kódrészlet, ami nem csak sokkal olvashatóbbá teszi a programot, de a végrehajtást is gyorsítja. Az eredeti így néz ki, ha egy egyszerűbb webáruházat írunk:

<code class="ruby">
# Erre itt szükségünk van, különben az << operátor nem értelmezhető lentebb
amount_array = []
# for ciklus
for order in account.orders
  amount_array << order.amount.some_operation
end
</code>

Ehyelyett használható a map is. Ilyen egyszerű egysoros kód lesz belőle:

<code class="ruby">
# A map visszatérési értéke mindig tömb, ezért a deklaráció fölösleges.
amount_array = account.orders.map { |order| order.amount.some_operation } 
</code>

Persze aki hosszabb ruby programot írt már, biztosan ismeri és használja ezeket a rövidítéseket, de talán van, aki az én bejegyzésem miatt fogja kipróbálni a rubyt. Ezen az oldalon meg is teheti a böngészőjében: http://tryruby.org/
