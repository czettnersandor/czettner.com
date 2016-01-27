---
layout: post
title: Zend Certificate
created: 1308768219
comments: true
categories: [zend]
---
Elkezdtem felkészülni a Zend PHP Certificate tanúsítvány megszerzésére, majd ha az megvan, megcsinálom a Zend Framework Certificate-et is. Azért kezdtem ezt a blogtémát, hogy tapasztalatot oszthassak meg azokkal, akik szintén úgy döntenének, mint én. A tanúsítványok megszerzésével én első sorban a szakmai tudásomat akarom helyre tenni illetve próbára tenni. Aki szintén szeretné a vizsgákat letenni a PHP-t fejlesztő vállalatnál, az jó ha tudja: nem azért nehéz ez a vizsga, mert a PHP olyan nagyon összetett nyelv lenne, hanem mert valóban ismerni kell a PHP nyelv belső működését, ismerni kell az alapvető programozási algoritmusokat, az MVC tervezési mintán kívül még elengedhetetlen a mérnöki szemlélet is.

A Zend tanúsítványok nemzetközileg elismert, a papírral a kezünkben eladhatóbbá tesszük magunkat nem csak külföldön, de Itthon is. Bár ez a külföld-itthon dilemma az internet korában okafogyott, eddig is és ezután is vállaltam külföldi munkákat.

A véleményem az, hogy aki szabadúszóként kíván tevékenykedni, annak szinte kötelező ez a vizsga. De a Magento webáruház is erre épül, ami egy iparági szabvány ma már a webáruházak területén, ezért ha ezzel szeretne valaki foglalkozni, a Zend Tanúsítványok megszerzése ajánlott.

Jómagam nem szeretnék 1000$ feletti összeget fizetni az oktatásért, mivel szerintem a tudás egy részével már rendelkezem és csak a vizsgára kell felkészülnöm. A vizsga ára 180€ körül van, de ha nem sikerül, ezt a pénzt nem adják vissza, lehet újra próbálkozni. A hozzám legközelebbi vizsgaközpont az IQSOFT John Bryce Oktatóközpont. A vizsga angol nyelvű.

A Zend PHP Certificate leginkább a PHP 5.3 nyelvről szól, tehát a vizsga is nagy valószínűséggel az ebben foglalt újdonságokat kéri majd számon, a részletes változások listája itt található: http://www.php.net/ChangeLog-5.php#5.3.0 

Ezek közül a legfontosabbak:
<ul>
<li>Névterek kezelése</li>
<li>Késői statikus kötés</li>
<li>Névtelen függvények</li>
<li>Új mysql adatbáziskezelés (mysqlnd)</li>
<li>Új garbage collector</li>
<li>Új operátorok</li>
<li>i18n kiterjesztés</li>
<li>új hibaszint az E_DEPRECATED</li>
<li>PHP Standard Library bővítése az SPL adatstruktúrával és ez mindig engedélyezett</li>
</ul>

Tehát ezeket alaposan meg kell ismerni, nem elég már csípőből tudni, hogy ez a pár soros mit ír ki:

<code class="php">
$a = 'name';
$$a = 'Lord';
echo $name;
</code>
