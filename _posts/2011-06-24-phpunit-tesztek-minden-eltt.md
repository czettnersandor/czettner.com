---
layout: post
title: PHPUnit - tesztek minden előtt
created: 1308947640
comments: true
categories: [zend]
---
Tesztekre szükség van, hiszen a jó programozót nem az különbözteti meg a rossz programozótól, hogy hibátlan kódot ír, hanem attól lesz valaki jó programozó, hogy a saját hibáit felismeri és képes javítani rajta. A Test Driven Development (Tesztvezérelt fejlesztés) paradigmája a felismerés folyamatát automatizálja. A rossz programozó var_dump-okat és echo-kat tesz az osztályaiba és reménykedik, hogy az ezredik futásra képes kijavítani a hibás működést. Jobb lenne ezt automatikusan, nem? Létezik erre egy keretrendszer, ami széles körben használt. A neve PHPUnit. Ez egy olyan keretrendszer, mely kifejezetten a tesztekhez íródott és erre kitűnően használható is. Nem PHP találmány, először Java-hoz jelent meg JUnit néven, de számos más programnyelven is implementálták egyszerűsége és népszerűsége miatt.

Egy példán keresztül mutatom be, hogy hogyan kell használni. Tegyük fel, hogy szeretnénk egy Log nevű PHP osztályt implementálni, ami egyszerű naplózási műveleteket valósít meg. Mielőtt elkezdenénk a Log osztály kódolását, először meg kellene határozni, hogy mit csináljon ez az osztály. Jobb fejlesztők első lépése az UML diagram megrajzolása, ami alapján később a teszteket és a végleges kódot is írják. A Log osztály legyen képes naplófájlokat létrehozni és ha a naplófájl már létezik, csak nyissa azt meg példányosításkor, ne ürítse ki a tartalmát. A Log osztály legyen képes a példányosításkor megnyitott fájlba naplóüzenetet írni. Számos elvárást fogalmazhatunk még meg, de a példa kedvéért csak ezt a kettőt fogom bemutatni. A project manager már megrajzolta az UML diagramokat, a fejlesztők pedig tudnak róla, hogy a cég minőségbiztosítása megköveteli a tesztek írását.

A teszt osztály neve minden esetben így néz ki. ClassTest, tehát ebben az esetben LogTest. Két tesztet is szeretnénk elvégezni, ezért az alábbi funkciókat is hozzáadhatjuk a PHPUnit osztályhoz:

{% highlight php %}
<?php  
require_once('Log.php');  
class LogTest extends PHPUnit_TestCase  
{
  private $log;
  public function setUp(){ }  
  public function tearDown(){ }
  public function testLogFileExists(){ }
  public function testDontPurgeLogFile(){ }
}
{% endhighlight %}

A setUp() függvény a teszt indításakor fog lefutni és ha elindult, a PHPUnit egyszerűen az összes test-el kezdődő függvényt végrehajtja, majd lezárásként a tearDown() következik, ahol jellemzően a megnyitott fájlok, megnyitott kapcsolatok zárhatóak le. Mi most itt a létrehozott naplófájlt fogjuk törölni.

A setUp()-ban létrehozzuk a Log osztály egyik példányát. Fontos, hogy a __constructor egyetlen paramétere már tartalmazza a naplófájl nevét is:

{% highlight php %}
public function setUp(){
	$this->log = new Log('/tmp/unittest.log');
}
{% endhighlight %}

A tearDown()-ban a létrehozott osztály példányát illik törölni. Aki programozott már valaha C64-et, annak berögzült, hogy ha megnyitjuk a floppy meghajtót, akkor azt valahol le is zárjuk, különben folyamatosan világítani fog a lámpája, meg egyébként is tiszta kódot eredményez, ha megtesszük, a garbage collectornak is kevesebb dolga lesz. Ezen kívül itt törölni is kell a létrehozott logfájlt, a fájlnevet a Log osztály eltárolja, ezért az onnan nyerjük ki, hogy ne kelljen feleslegesen mégyegyszer begépelni itt is a fájlnevet:

{% highlight php %}
public function tearDown(){
	unlink($this->log->filename);
	unset($this->log);
}
{% endhighlight %}

Mivel a testLogFileExists() a setUp() után fog lefutni, itt kell ellenőrizni, hogy a fájl valóban létrejött-e. A PHPUnit szintaktikája nagyon egyszerű, mindössze ennyit kell tennünk:

{% highlight php %}
public function testLogFileExists(){
	$this->assertTrue(file_exists($this->log->filename));
}
{% endhighlight %}

Ha a fájl ezen a ponton nem létezik, a teszt lefuttatásakor hibát fogunk kapni, mivel a teszt egyik állítása (assert) nem lett igaz. A következő teszt annak a vizsgálata, hogy a fájlba írás után és újra megnyitva a fájlt ezzel az osztállyal nem egy üres fájlt kapunk. Bár én nem bíznám rá a banki tranzakciók logját, de ez a teszt arra is alkalmas, hogy megnézze, valóban beleírt-e a fájlba a Log osztály. A fájlnevet megint a példányból olvassuk ki:

{% highlight php %}
public function testDontPurgeLogFile(){
	$this->log->write("Hello world!");
	$filename = $this->log->filename;
	unset($this->log);
	$this->log = new Log($filename);
	$this->assertTrue(filesize($filename) > 0);
}
{% endhighlight %}

Ennyi. A teszt futtatását egy egyszerű php fájlból kezdeményezhetjük, ezt szokás feladatonként csoportosítani, vagy egy-egy nagyobb release után kibővíteni. Futtathatóvá téve gyorsan futtatható parancssorból:

{% highlight php %}
<?php
require_once 'PHPUnit.php';
require_once 'LogTest.php';

$suite  = new PHPUnit_TestSuite("LogTest");
$result = PHPUnit::run($suite);

echo $result->toString();
{% endhighlight %}


Ezek után a Log osztály implementációja gyerekjáték, hiszen nem csak az UML specifikáció adott, hanem a szükséges tesztek is, tehát tudjuk, hogy hogyan kell kinéznie az osztálynak és hogyan kell működnie. Így néz ki a Log.php:

{% highlight php %}
<?php

Class Log {
	public $filename;
	private $file;

	public function __construct($filename) {
		$this->filename = $filename;
		$this->file = fopen($filename, 'a');
	}

	public function write($line) {
		fwrite($this->file, $line.PHP_EOL);
	}

	public function __destruct() {
		fclose($this->file);
	}

}
{% endhighlight %}

Miért jó ez? Nem szükséges éles környezetbe tenni az osztályt ahhoz, hogy megtudjuk, helyesen működik-e. Ha később új képességekkel szeretnénk felruházni, kibővíthetjük a LogTest osztályt a megfelelő funkciókkal, majd anélkül tesztelhetjük a helyes működést, hogy a teljes alkalmazást el kelljen indítani és egyúttal lefutnak a korábbi tesztek is, így biztosak lehetünk abban, hogy az új feladat megvalósításával nem rontottunk el olyat, ami korábban már működött. A legfontosabb, hogy a felhasználó által végzett teszt sokkal lassabb, mint az automatizált tesztek és nem is biztos, hogy a fejlesztő észreveszi, hogy hol hibázott. Remélem sokakat sikerült most leszoktatni a var_dump és echo felesleges és bosszantó használatától és aki még nem kezdte el a saját kódja unit tesztelését, mostantól elkezdi, hiszen erősen javítja a fejlesztés hatékonyságát.

A példában használt összes kód letölthető a githubról:
https://github.com/czettnersandor/phpunit-log-tutorial
