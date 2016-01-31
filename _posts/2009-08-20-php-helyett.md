---
layout: post
title: PHP helyett
created: 1250779630
comments: true
---
A napokban volt szerencsém néhány, PHP-t helyettesíteni tudó programnyelv tanulmányozására. Nos azt kell mondanom, a PHP egy bughalmaz és a logikus programozás ellentettje, ami csak azért maradhatott fenn, mert túl sokan használják. Az évek alatt nem lecserélték, hanem rétegenként újabb, egymástól független hibákkal javították ezeket a tévedéseket a visszafelé kompatibilitás miatt.Pedig bőven volt verziószámváltás, PHP4-ről PHP5-re váltáskor nyugodtan lehetett volna from scratch újraírni a nyelvet.

Gondoljunk csak a <a href = "http://developers.slashdot.org/comments.pl?sid=1008291&cid=25522773">visszaperjelre</a>, mint névtér elválasztóra vagy a stringműveleteknél használatos paraméterek összevisszaságára. De elég csak annyit mondani, hogy a változókat nem szükséges deklarálni, viszont az problémát jelent, ha egy osztályban nincs deklarálva. Kaporszakállú profok vitatkoznak ezen, ők általában python vagy perl mellett teszik le a voksukat, így egy próbát én is tettem, mindkét nyelven megpróbáltam egy "Hello world" szerű valamit létrehozni a lehető legszabványosabb módon. Amikor belekezdtem, az volt a célom, hogy objektumorientált módon írjam ki a képernyőre a mondandómat, amit lehetőleg előzetesen egy változóban tároltam. Tehát az egysoros <php> echo "Hello World"; </php> kiesett.

<h3>Python</h3>

Kezdjük a Pythonnal. Ami elsőre furcsa lehet, azok a behúzások. Nincs C-ben megszokott hullámos zárójel a kódblokkok megnyitására és lezárására, hanem az ilyesmit behúzással oldották meg. Elsőre ez a C-hez szokott szem számára olvashatatlan, de végül is gyorsan megszokható és ezután jóval szebbnek is láttam az egészet. Például:

<python>
if "nem" == "igen":
    print "Hello" + "world" + "!!!"
elif "nem" == "nem":
    print "Nem, nem, nem!"
</python>

Valójában a Python nem ad akkora OOP élményt, mint például a Ruby (régi kedvencem), ahol minden egy objektum, de mivel én mindenáron objektumokkal szeretnék építkezni, vágjunk bele. Az objektumok gyk zárt egységek, amik deklarálás után különböző interfészek segítségével, pl metódusokkal kommunikálnak. Nem szeretnék éjszakába nyúlóan cikkeket írni, pythonról rengeteg online cikk olvasható még OOP téren is, jöjjön tehát maga az objektumorientált kód némi megjegyzéssel az érthetőség miatt:

<python>
#!/usr/bin/python
# -*- coding: utf8 -*-

class Hello:
    def __init__(self,kiirando):
      # az aláhúzás (_) priváttá teszi a változót
      self._Parameter=kiirando
    def getParams(self):
      return self._Parameter
    # Aláhúzás nélkül a változó kívülről is elérhető lesz
    Parameter=property(getParams);

#Osztály létrehozása
a=Hello("Hello Világ!")
print a.getParams()
print a.Parameter
</python>

A második # jellel kezdődő megjegyzés sorban beállítottam a fájl karakterkódolását, mivel ékezetes karaktereket is hasznláltam egy sztringben és a pythonnak tudnia kell a karakterkódolását, ha nem us-ASCII karakterek is kerülnek a kódba, ellenkező esetben az értelmező hibát ír ki.

<h3>Perl</h3>

Ez a nyelv szinte kötelező tanulmány, hiszen az összes *BSD és Linux parancsértelmező használja. Az egyszerű felhasználó szemszögéből nézve ha egy könyvtárban, ahol létezik a hello fájlnevű perl script és kiadjuk a ./hello parancsot, szinte biztos, hogy lefut az adott perl program. Első ránézésre C, awk és persze sh programozási nyelvekből merít, a wikipédia szerint pedig "könnyű benne olvashatatlan kódot készíteni". Lássuk a pythonnál is kezdett elágazás (if) paranccsal:

{% highlight html %}
if( $d eq "kakukk\n" ){
  print "Helló világ!\n";
  }else{ print "Ma nincs jó napod!\n" }
{% endhighlight %}

 És valóban. Mint ahogy például PHP-ban és C-ben megszokhattuk, itt sem kell a lezáró kapcsos zárójel előtt lévő parancs végére pontosvesszőt tenni. Az operátorok pedig némiképp eltérnek a C-s, PHP-s gyakorlattól. Sajnos a goto is megengedett, ami további lehetőséget nyújt barkács programozók számára a továbbfejleszthetetlen és ronda forrás írásához. Osztályt a Package paranccsal lehet létrehozni, amihez nem létezik lezárás, tehát az osztály definíciója a fájl végéig vagy a következő Package parancsig terjed. Modern megoldásként a programkódot a Package main után kell írni. Csalódottságom gombócát lenyelve folytatom az objektumorientált példával:

{% highlight html %}
#!/usr/bin/perl
package Hello;
#constructor
sub new {
    my $self = {};
    bless $self, 'Hello';
    return $self;
}

sub Szoveg {
    return "Hello Világ!\n";
}

package main;
{
    my $hello = Hello->new();
    print $hello->Szoveg;
}
{% endhighlight %}

Szép, ugye? Sajnos nem. Következő&nbsp; leírásom a ruby nyelvről fog szólni. Érdemes elolvasni a <a href="http://hu.wikipedia.org/wiki/Ruby">wikipedia leírását</a> ezzel kapcsolatban.
