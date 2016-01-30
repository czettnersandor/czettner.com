---
layout: post
title: Ruby tpasztalatok
created: 1257980400
comments: true
---
Ruby. Régi kedvenc, de most éles bevetésben is bizonyított. Egy adatbázis import scriptet írtam benne, ami adott csv-k adatait olvasta be, rakta össze és írta egy másik csv fájlba, amit később Drupalból importálni tudtam.

A nyelv alapjai és sajátosságai figyelemre méltóak. A Ruby nyelvet Yukihiro Matsumoto kezdte el fejleszteni 1995 környékén. Jól olvasható és könnyen érthető nyelv. Objektumorientáltsága alkalmassá teszi arra, hogy az alkalmazásokat csapatmunkában fejlesszék benne. A ruby nyelven minden objektum, értelme van például annak is, hogy

100.to_s

Ezt irb-be (interactive ruby shell) kiadva a visszaadott érték "100" karakterlánc lesz. Rubyban lehetőség van arra is, hogy már meglévő osztályokat egészítsünk ki saját funkcióval. Az adatfeldolgozó scriptemhez szükség volt egy olyan eljárásra, ami a csv rekodjaiból kiszedi az idézőjeleket, de kizárólag a karakterláncok elejéről és végéről. Így egészítettem ki a String osztályt:

 
<ruby>
class String
  require 'iconv'
  def to_utf8!
    Iconv.conv('utf-8', 'ISO-8859-2', self)
  end
  def stripq
    self.strip.sub(/^\"|\"$/, "")
  end
end
</ruby>

Látszik, hogy létrehoztam egy to_utf8! függvényt is, aminek a végén lévő felkiáltójel jelzi, hogy az osztály példányán változtat. Ezt ruby-ban prédikátumoknak nevezik. a stripq függvény egy értéket ad vissza. PHP programozóknak talán nem világos, hogy miért nincs return a függvényben. Lehetne benne, de így olvashatóbb és a végeredmény ugyanaz. Az end sor előtt lévő visszaadott érték  lesz a return értéke a stripq-nak. Használhattam volna a sub függvényt is az itt megadott paraméterekkel minden olyan esetben, amikor egy karakterláncot szerettem volna megtisztítani az idézőjelektől, de nem szeretem magam ismételni. Ezzel a megoldással ráadásul ha valami hibát találok a regex-ben, csak egy helyen kell módosítani a forrást.

Remélem ezzel a rövid írással sikerült érdeklődést keltenem tanulni vágyó programozóknak a ruby felé, én mindenképpen szeretném a PHP-t kiváltani ezzel, ahol csak lehetséges.
