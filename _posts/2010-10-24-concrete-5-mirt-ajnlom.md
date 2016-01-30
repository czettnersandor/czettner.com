---
layout: post
title: Concrete 5 - miért ajánlom?
created: 1287936676
comments: true
---
Több népszerű, ingyenes CMS létezik abban a kategóriában, amibe a Concrete 5 készült, de most elfogulatlanság nélkül szeretném bemutatni a Concrete 5 (röviden C5) előnyeit a többivel szemben. Ehhez némi történeti áttekintés szükséges eme nagyszerű tartalomkezelő rendszer életútjáról:

Az első verzió 2003-ban jelent meg <a target="_blank" href="http://franzmaruna.com/">Franz Maruna</a> és <a target="_blank" href="http://andrewembler.com/">Andrew Embler</a> munkájaként. Korábban kisebb webshopok készültek benne, több éve fejlesztették saját oldalaik számára. Az első verzió ezeknek a fejlesztéseknek a teljes átirata PHP4 alapokra. Már az első verzió az agilis programozás tervezési mintáiból merített. Úgymint MVC felépítés. Az egész Concrete CMS v.1 ezen a szemléletmódon alapult és már akkor látszott, hogy ez a rendszer avatott kezekben nagyon hasznos eszköz lehet.

A Concrete 2008-ig még három nagyobb kiadást élt meg, az 5-ös verzió egy teljesen átgondolt tartalomkezelő lett az alapoktól újraírva. Franz egyébként Concrete alapokon futó webáruháza havi 40 ezer $ hasznot termelt, Andrew 2 gyermeke pedig egyre kevesebb időt hagyott neki a fejlesztésekre. A fiúk visszatértek a laborba és a Concrete 5 újraírása után azt <a href="http://www.opensource.org/licenses/mit-license.php">nyílt forráskódúvá</a> tették. 2008 júniusában az open source közösség egy robosztus, professzionális, egyszerűen használható, de könnyen fejleszthető CMS-t kapott. 2008 októberében a sourceforge.net a hónap projektjévé választotta, novemberben pedig napi átlagosan 1000 letöltője volt a c5 oldalról. Azt figyelembe véve, hogy ezeket a letöltéseket webfejlesztők generálták, ez nagy teljesítmény.

<h3>Előnyök, hátrányok</h3>

Mint a C5 oldalán is olvasható, ez egy olyan tartalomkezelő, ami marketing szempontok alapján készült valódi programozók számára, hogy azok ügyfeleik kezébe egy könnyen érthető és jól használható eszközt tudjanak adni. A C5 úgy tudott felhasználóbarát maradni, hogy közben professzionális programtervezési mintákat tudott magában tartani. Ez egy óriási probléma számomra is a választáskor, mivel a Wordpress felhasználóbarát ugyan, de a forráskódja csapnivaló és a rá való fejlesztés helyett legszívesebben inkább disznótrágyát lapátolnék. A Drupal pedig ugyan viszonylag tiszta felépítésű, - bár nem MVC és nem objektumorientált - az ügyfelek alapos oktatása nélkül egy darab kőkemény féltégla, nem adja magát. Azért hoztam fel ezt a két példát, mert általában ez a kettő, amit a legtöbbször kérnek a felhasználók, tehát nem azért, mert egyiket vagy másikat nem szeretem. Tehát a C5 előnyei:

<ul>
<li><span>Objektumnorientált felépítés:</span> bármi megoldható az eredeti forrás módosítása nélkül, egyszerűen csak kiegészítem egy másik PHP osztályban az eredetit és így felülírtam a régit.</li>
<li><span>MVC tervezés</span> nem véletlenül találták ki a legjobb programozók 1979-ben és használják azóta is. Könnyen átlátható forráskódot és könnyű módosíthatóságot eredményez a későbbiekben. Én nyugodtan rányomnám a HMVC bélyeget is, mivel a modulok segítségével megvalósul a hierarchikus felépítés is. Sőt ezt az alap C5 is nagyon jól kihasználja.</li>
<li><span>Felhasználóbarát:</span> Hihetetlen, de a fenti, leginkább programozóknak hasznos dolgok után az ügyfelek kényelmét is figyelembe vették a fejlesztés során. Az adminisztrációs felület egyszerűen zseniális. Ahogy képeket töltünk fel, különböző blokkokat adunk az előre elkészített helyekre a template-n belül, soha nem látott hatékonyságot ad a munkának ingyenes CMS rendszereken belül.</li>
<li><span>Fejlett template rendszer:</span> a site builder munkája gyerekjáték - mármint ha a C64-en, spektrumon és PC-n felnőtt és ezeket nem elsősorban játékra használó gyerekekre gondolunk :) Komolyra fordítva: egyértelműen elkülönülő template fájlok, DRY fejlesztést segítő hierarchia. A programozó munkája nem folyik össze a html kódot író fejlesztőjével.</li>
<li><span>SEO:</span> ilyen erős SEO képességeket Drupalon például csak egy rakás modul hozzáadásával tudok elérni, Wordpressen szintén. Minden benne van alapból, amire szükség van.</li>
</ul>

Egy rövid bemutató arról, hogy milyen egyszerű egy oldal szerkesztése Concrete 5-ön:

<center>
<object width="640" height="505"><param name="movie" value="http://www.youtube.com/v/AGW2E84vy00?fs=1&amp;hl=hu_HU"></param><param name="allowFullScreen" value="true"></param><param name="allowscriptaccess" value="always"></param><embed src="http://www.youtube.com/v/AGW2E84vy00?fs=1&amp;hl=hu_HU" type="application/x-shockwave-flash" allowscriptaccess="always" allowfullscreen="true" width="640" height="505"></embed></object>
</center>

Hátrányok. Nos igen, ilyen is van. Ugye a Wordpress, Joomla ereje abban van, hogy bárki letöltheti az adott rendszereket, feltelepítheti a legolcsóbb webtárhelyre és mindenféle szaktudás nélkül működnek. Fejlesztő közbeavatkozása nélkül igénytelenek ugyan, de működnek. C5 esetén ez nem így van. Jóval gyorsabban elkészül benne egy weboldal, mint bármilyen rendszeren, de szakértelem nélkül ne kezdjen hozzá senki. Ezt én, mint programozó nem hátrányként jellemezném. Hosting oldalról is különleges beállításokat igényel. Zend Framework alapokat is használ, így általában elmondható, hogy ahol Zend alkalmazások futnak, ott a C5 is futni fog, de sajnos Magyarországon egy igen jelentős hosting réteg (10% körül) képtelen jól konfigurálni a rendszerét.

<h3>Mire jó?</h3>

Az előnyök és hátrányok figyelembevétele után dönthetünk, hogy az alábbi feladatokra használjuk-e a rendszert:

<ul>
<li><span>Egyszerű menü-almenü felépítésű oldalak</span> fejlesztésére a legalkalmasabb. Itt alternatívája a Wordpressnek, de a C5 valóban erre a célra készült.</li>
<li><span>Webshop 20-30 termékkel:</span> nagyon erős webáruház modulja van, jelenleg 100$ körül van az ára, így megfontolandó a használata. Drupal - Übercart illetve Joomla Virtuemart rendszerekhez képest jóval felhasználóbarátabb, ami növeli a vásárlásokat is. Természetesen nagyobb, professzionális webáruházakra Magento-t érdemes választani, de biztos, hogy annak a képességei szükségesek? Nem lesz benne túl sok fölösleges sallang, amit ha nem veszünk ki sok munkával, eltereli a vásárlók figyelmét?</li>
<li><span>Céges weblapok</span> fejlesztésében verhetetlen. Képgaléria, termékbemutató, többféle showcase közül lehet válogatni. Ha új dolgot kell benne fejleszteni, az nagyon gazdaságosan megtehető. Nem ismerek alternatívát, bár ez a feladat mindenre ráhúzható. A kérdés csak az, hogy megéri-e a ráfordított időt.</li>
<li><span>Blog</span> indítása talán Wordpress alapokon egyszerűbb és gyorsabb, de mi van, ha ennél több kell? Mi van, ha fejlett CMS kell köré később? Webáruház? És gondoljunk csak a trágyalapátolásra, amit fentebb említettem.</li>
<li><span>Közösségi oldalak</span> számára kiváló. Nem egy példa van a C5 weblapján arra, hogy vallási közösségek, diákszervezetek vagy éppen üzleti körök használják. Saját profil, ismerősnek jelölés, fórum, hozzászólás a hírekhez mind megvalósítható viszonylag egyszerűen. Ha valami ingyen nem elérhető hozzá, néha könnyebb lefejleszteni, mint megvenni a fizetős modult hozzá és a fejlesztéssel még valódi programozást is lehet tanulni.</li>
</ul>
