---
layout: post
title: Bash okosságok - parancsolj és uralkodj!
created: 1278756053
comments: true
---
Épp most fedeztem fel egy nagyon hasznos bash feature-t, amivel az utolsó parancs utolsó paraméterét tudom lekérdezni. Elérhető változóként is, ami egy a sok un. "<a href="http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_03_02.html#table_03_03">Special bash variables</a>" közül. De ami jóval használhatóbb, az az ESC + '.' egymást követő lenyomása. Tehát valahogy így:

<code>$ mkdir /home/zoner/Dokumentumok/honlapszerkesztes/betafence</code>
Itt az utolsó paraméter a könyvtár neve, ami kétféleképpen érhetző el. A változó neve '$_' (dollárjel, aláhúzás). A másik, amikor újabb parancs beírása közben ESC-et követően pontot nyomok és bemásolja az utolsó parancs utolsó paraméterét oda, ahova a kurzor áll. Hol hasznos ez? Például amikor a létrehozott könyvtárba szeretnék lépni:
<code>$ cd $_</code>

<h3>Bash autocompletion</h3>
Az előző példában nem szükséges a teljes elérési útvonalat begépelni, ugyanis a bash egyik leghasznosabb képessége az automatikus kiegészítés. A /home/zoner, ha éppen ez a home könyvtáram pedig helyettesíthető egy ~ jellel, tehát amit be kell írni az valahogy így néz ki (a szögletes zárójelben a billentyűk neve található): <code>mkdir ~/Dok[TAB]honl[TAB]betafence</code>, majd <code>cd [ESC].</code> Meg kell itt jegyezni, hogy többszöri lenyomásra az előző értékeket is ki lehet vele szedni.

Évek óta nem használtam Windows-t, de ott ugyanezért valami nagyon kényelmetlen GUI-t kell használni, ahol minimum 10x ennyi idő egy könyvtár létrehozása. Persze többféle fájlkezelő elérhető linuxra is, de mi értelme van, ha ilyen egyszerű parancsokkal jóval hatékonyabban lehet dolgozni.
