---
layout: post
title: PHP konstruktorok
created: 1282122725
comments: true
---
Egy újabb szépségét fedeztem fel a PHP-nak, legalábbis szép abban a tekintetben, hogy előrehaladásnak tűnik abba az irányba, amit maga a nyelv soha nem fog elérni, az átgondoltságot. Egyik régebbi munkámhoz tartozó app a szerveren tökéletesen működött, a tesztkörnyezetemben (Archlinux, php 5.3.3) meg se nyikkant. A hiba oka, hogy egy php osztályom constructor metódusa sehogyan sem akart lefutni, ezért elkezdtem keresni a google segítségével, hogy mi lehet a gond. Nem volt nehéz, hiszen a php.net főoldalán rögtön az első hír hozza a hiba okát.

<code class="php">
namespace Foo;
class Bar {
    public function Bar() {
        // treated as constructor in PHP 5.3.0-5.3.2
        // treated as regular method in PHP 5.3.3
    }
}
</code>

Tehát ha egy névtérben vagyunk, akkor a Bar osztály Bar() metódusa nem konstruktorként, hanem egyszerű mezei metódusként viselkedik, a példányosításkor nem fut le. Persze használhattam volna __construct() nevet is hozzá, de valamiért amikor ez készült, nem így tettem. Ez bizony az én hibám, hiszen az osztály nevével azonos konstruktor még php 4-ben volt divat. Mellesleg C++ is ezt a stílust ajánlja. Hiba volt, hogy PHP-ban egyáltalán névtereket használtam. Minek is? Ha jól emlékszem, itt éppen az 5.3-as ezen új képességét akartam megtanulni, de végül nem vettem hasznát és ez nem az én képességeimet degradálja, mert ruby nyelven nagyon szépen használok névtereket. Ebben az esetben egy MVC struktúrában akartam elválasztani a modelt a controllertől. Szépnek találtam, hogy ugyanazt a nevet tudom nekik adni, mégis elkülönülnek egymástól:
<code class="php">
namespace AppControllers;
class Customer extends AbstractController {
    // controller osztály
}

namespace AppModels;
class Customer extends AbstractModel {
    // model osztály
}
</code>

És itt még egy apróság a névterekről: a névterek elválasztására (namespace separator) az osztályoknál megszokott twist (::, azaz dupla kettőspont) helyett backslahs-t (\) használnak. Elhiszem én, hogy nem lehet megoldani a különbségtételt ebben az esetben osztály és névtér között, de valahogy minden normális nyelven megoldják :) 2008-ban a hivatalos indoklás:

"PHP is finally getting support for namespaces. However, after a couple hours of conversation, the developers picked '\' as the separator, instead of the more popular '::'. Fredrik Holmström points out some problems with this approach. The criteria for selection were ease of typing and parsing, how hard it was to make a typo, IDE compatibility, and the number of characters."

Aham. tehát a gigahertz-ek és terrabájtok korában spóroljunk le egy elágazást és egy bájtnyi karaktert annak érdekében, hogy egy fokkal olvashatatlanabb és inkonzisztensebb legyen a kódunk.
