---
layout: post
title: Magento - small_image és thumbnail beállítása scriptből
created: 1311768416
comments: true
categories: [magento]
---
Egy import során rengeteg kép került bele egy termékadatbázisba, viszont a small_image és thumbnail kimaradt, ami a terméklistákban és a kapcsolódó termékek között így nem tudott megjelenni. Írtam egy rövid scriptet, ami néhány perc futás után megoldotta a problémát:

{% highlight php %}
require 'app/Mage.php';
Mage::app();

$products = Mage::getModel('catalog/product')->getCollection()->addAttributeToSelect('*');
foreach ($products as $product) {
    $subj = $product->getImage();
    $pattern = '/\/([^\/]*)/';
    preg_match($pattern, $subj, $matches, PREG_OFFSET_CAPTURE, 3);
    $image = $matches[1][0];
    if (!$subj) continue;
    if (!$product->hasSmallImage()) $product->setSmallImage($image);
    if (!$product->hasThumbnail()) $product->setThumbnail($image);
    $product->save();
}
{% endhighlight %}
