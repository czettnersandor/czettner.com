---
layout: post
title: Feladat programozóknak
created: 1284049423
comments: true
---
Kaptam egy játékos feladatot, ami annyira megizzasztott, hogy közzéteszem.

Adott egy ilyen háromszög:

<pre>
    3
   7 4
  2 4 6
 8 5 9 3
</pre>

Fentről indulva találjuk meg azt az utat, amit bejárva a legnagyobb összegű számsort kapjuk. Számoljuk ki, hogy mennyi ez a legnagyobb összeg. Mi sem egyszerűbb ennél, írjunk egy rekurzív függvényt. A rekurzív függvényem, amit írtam, egészen 20 sorig tökéletes volt. 20 sornál már 1 másodperc volt a számítás.

A lehetőségek száma (N a sorok száma): 2<sup>N-1</sup> azaz kicsit programozói szintaktikával: 2^(n-1). Nos ha ez hatványozódik, akkor 50 sornál már kb 5 millió éves számítással kell számolni az asztali gépemen, amit most nincs időm kivárni.

Kezdjünk neki. (16:29-kor)

A kedvenc programozási nyelvemet fogom használni, ami a ruby. Létrehozzuk a projekt könyvtárát:

<code>
$ mkdir webfeladat
$ cd webfeladat
$ touch webfeladat.rb fa.rb
$ chmod +x webfeladat.rb fa.rb
$ gvim fa.rb webfeladat.rb
</code>

Itt megszerkesztjük azt a programot, ami létrehozza a fát, ezt egyszer fogjuk használni:

<code class="ruby">
#!/usr/bin/env ruby
# fa.rb - copyright Sándor Czettner

# megnyitjuk a fájlt
treeFile = File.new("tree.txt", "w")

# 50 soros fa
50.times do |i|
    tline = ""
    (i+1).times do |n|
        # Random numbers from 0 to 9
        tline += Random.new.rand(0..9).to_s + " "
    end
    treeFile.puts(tline)
end

treeFile.close
</code>

Következik az adatfeldolgozó script. A feladat szerint a standard bemenetről fog táplálkozni, ami azt jelenti, hogy linux alatt valami ilyen pranccsal lehet átadni neki egy fájlt:

$ cat tree.txt | ./webfeladat.rb

Tehát a program:

<code class="ruby">
#!/usr/bin/env ruby
# webfeladat.rb

def find_path
    w=0
    ($lines.length).times do |r|
        v = 0
        $a[r] = 0 unless $a[r]
        for p in 0..r do
            t = $lines[r][p]
            w = $a[p]
            $a[p] = t + [v, w].max
            v = w
        end
    end
    $a.max
end

$lines = Array.new

STDIN.each_line do |l|
    $lines << Array.new(l.split(" ").map{|s|s.to_i})
end

$a = Array.new

puts "The solution: "+find_path.to_s

</code>

Hogyan működik?

Hozzáteszem, a megoldáson órákig gondolkodtam, rengeteg sikertelen próbálkozásom volt, de nem hagytam annyiban. Már azt hittem, nincs megoldás. A google segítségével megtaláltam a kérdést, de választ már nem: http://projecteuler.net/index.php?section=problems&id=67

A megoldás nem triviális, de mégis annyira egyszerű, mint a faék. Induljunk ki a legelemibb, 3 elemből álló kétsoros háromszögből:

<pre>
 x
y z 
</pre>

valós számokkal:

<pre>
 2
4 6
</pre>

Erre a legnagyobb értékű út 2 > 6, az összege 8. A program fentről lefelé haladjon, a nulladik sorban egyértelmű, hogy a 2. elemet vesszük kezdőértéknek, tehát az első elem a 2. Eltároljuk a <strong>v</strong> értékét, hiszen erre szükség lesz később. 

A kiértékelés továbbmegy, szükség lesz még egy változóra. Azért csak egyre, mert lefelé csak két lehetőség van. Legyen ez <strong>w</strong>. A sorok ciklusának kezdetén szükséges nullázni <strong>v</strong> értékét. Ebben a lépésben lehet számolni egy olyan értéket, ami minden egyes oszlop és a felette álló utolsó legnagyobb érték összege. Ez a háromszög második sora esetében : 6 és 8. Ezt a két számot eltároljuk egy tömbbe, ami most két elemű lesz, a következő ciklusban fogjuk felhasználni, ahol ez a tömb már három elemű lesz, stb.

ha az utolsó sorra értünk, nincs más dolgunk, mint megnézni azt, hogy melyik érték a legnagyobb és ez lesz a legnagyobb összegű bejárható útvonal összege. Szemmel láthatóan 6 sornál is működik, 50 sornál már nem volt szemem a teszteléshez, de bízom a logikámban. 1000 sort tud ez a script 1 másodperces idővel az asztali gépemen:

<pre>
$ uname -a
</pre>
Linux turulpc 2.6.35-ARCH #1 SMP PREEMPT Fri Aug 27 17:14:28 CEST 2010 x86_64 AMD Phenom(tm) 8450 Triple-Core Processor AuthenticAMD GNU/Linux

Ugyan már 22:40 van, de ez a pár óra programozás kiváltott 5 millió évet és rendkívül memóriaszegény :)
