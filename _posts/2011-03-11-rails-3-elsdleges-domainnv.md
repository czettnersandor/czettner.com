---
layout: post
title: Rails 3, elsődleges domainnév
created: 1299882741
comments: true
categories: [ruby, rails]
---
Szükségem volt egy olyan átirányításra, ami mindent átirányít egy bizonyos domainre, ami nem arra a domainre megy. Egyszerű dolgom lett volna .htaccess segítségével, de ezt az oldalt nem apache szolgálja ki, hanem Unicorn, ami egy, a twitter által is használt Rack alapú webszerver. Az egyik megoldás egy egyszerű kis gem lett volna, a <a href="https://github.com/tylerhunt/rack-canonical-host">rack-canonical-host</a>, de unicorn alatt nem indult el vele az alkalmazás (A fejlesztői gépen Webrick fut, azzal működik). Nem akartam kísérletezgetni túl sokat, ezért született egy ötsoros hack, amit a routes.rb fájlba írtam, íme:

<code class="ruby">
  if(RAILS_ENV=='production')
    constraints(:host => /^(?!turaindex.hu).+/) do
      root :to => redirect("http://turaindex.hu")
      match '/*path', :to => redirect {|params| "http://turaindex.hu/#{params[:path]}"}
    end
  end
</code>

Biztosan van szebb megoldás, de gyorsabbat nem tudok elképzelni, ez összesen 1 db rövid regex minden lekérésnél. Mint látszik, az egész előtt megvizsgálja, hogy production környezetben fut-e, így a fejlesztői gépen (development, testing) nem irányít át sehova.

Remélem hasznos lesz ez másnak is.

És hogy indulás előtt legyen mit indexelni a keresőnek: http://turaindex.hu
