---
layout: post
title: Magento - úszó kategóriák
created: 1296555970
comments: true
categories: [magento]
---
Előfordul, hogy vannak olyan "úszó" kategóriák, amibe időnként kerülnek termékek és néha kiveszünk onnan néhányat. Ilyenkor jó lenne, ha a keresők akkor is visszatalálnának a termékre, ha a régi kategória URL-ével indexelték az oldalt. Ezt legegyszerűbben egy 301-es redirect-el lehet megoldani, ami a kategóriát észlelve mindig átirányít a termék általános URL-ére. Ebben az esetben az sem fordulhat elő, hogy a kategória könyvtára az URL-ben indexelve lesz, mert mindig a csupasz termék lesz látható a keresők számára. Egy rövid .htaccess szabályt kell csak alkalmazni:

{% highlight html %}
## Úszó kategóriák
    RewriteRule ^akciok/(.*)$ http://kodakmedia.hu/$1 [R=301,L]
    RewriteRule ^mai-ajanlat/(.*)$ http://kodakmedia.hu/$1 [R=301,L]
    RewriteRule ^mai-ajanlat-f/(.*)$ http://kodakmedia.hu/$1 [R=301,L]
{% endhighlight %}
