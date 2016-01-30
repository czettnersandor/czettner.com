---
layout: post
title: Rails 3, MySQL és UTF-8
created: 1290291847
comments: true
---
Elkezdted használni a Rails 3-at? Biztosan észlelted, hogy a mysql adatbázisod nem működik megfelelően. Ez azért van, mert a mysql gem ASCII-8BIT kódolást, a Ruby 1.9, Ruby On Rails 3 és a MySQL adatbázisod is UTF-8-as kódolást használ. A model létrehozása nem fog problémába ütközni, de amint a model betolná az adatbázisba az első UTF-8 stringet, az alkalmazás egy szép exception-el elhasal. mi a megoldás?

A Gemfile fájlba tedd a következőt:

<code>
gem "mysql2"
</code>

A databases.yml fájlt ezután hasonlóan írd át:
<code>
development:
  adapter: mysql2
  database: adatbazisnev
  username: adatbazisuser
  password: jelszo
  encoding: utf8
</code>

A "mysql" gem egy másik alternatívája a "ruby-mysql" gem, de ezt azért nem ajánlom, mert a "mysql2" bináris kódjával ellentétben az interpretált ruby nyelven írták, ezért nagyobb terhelés mellett több erőforrást fog használni. Akkor lehet jó, amikor arra az architektúrára, amire fejlesztünk, a mysql2 nem elérhető.

A harmadik megoldás kicsit csúnya, de nagyon jól működik. Helyezzük el az alábbi kódot valahol a /lib könyvtáron belül:

<code class="ruby">
require 'mysql'

class Mysql::Result
  def encode(value, encoding = "utf-8")
    String === value ? value.force_encoding(encoding) : value
  end

  def each_utf8(&block)
    each_orig do |row|
      yield row.map {|col| encode(col) }
    end
  end
  alias each_orig each
  alias each each_utf8

  def each_hash_utf8(&block)
    each_hash_orig do |row|
      row.each {|k, v| row[k] = encode(v) }
      yield(row)
    end
  end
  alias each_hash_orig each_hash
  alias each_hash each_hash_utf8
end
</code>

Mit is csinál ez? Kiegészíti a Mysql::Result osztályt azokkal a függvényekkel, amik az UTF-8 konverziót végzik, majd az eredeti függvényekre egy aliast gyárt. Az each, each_has függvény így az új kódot fogja futtatni, ami már kezeli az UTF-8 karaktereket. Ha másra nem is jó ez a megoldás, a ruby nyelv szépségének és olvashatóságának bemutatására mindenképpen. Itt üdvözlök minden PHP-only programozót!
