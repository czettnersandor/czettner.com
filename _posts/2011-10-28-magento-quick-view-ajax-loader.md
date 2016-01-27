---
layout: post
title: Magento Quick View Ajax Loader
created: 1319802573
comments: true
categories: [magento]
---
<a href="https://github.com/czettnersandor/magento-quick-view-ajax">"Quick View Ajax Loader"</a> is an extension for the product list page in your Magento online store. Let your customers see the details of the products in your store without leaving the category page.

Compatible with Magento 1.4, Magento 1.5, Magento 1.6

Installation instructions

<li>Copy the content of the repo to the Magento installation folder</li>
<li>Copy app/design/frontend/default/default/* to the active design folder</li>
<li>Copy skin/frontend/default/default/* to the active skin folder</li>
<li>Modify your list.phtml image section with the following code:</li>

<code class="php">
<p class ="product-image">
    <a href="<?php echo $this->getUrl('ajax/product/quickview/id/' . $_product->getId()) ?>" title="<?php echo $this->htmlEscape($_product->getName()) ?>" class="ajax">Gyors nézet</a>
    
    <a href="<?php echo $_product->getProductUrl() ?>" title="<?php echo $this->htmlEscape($this->getImageLabel($_product, 'small_image')) ?>" class="product-image-img">
        <img src="<?php echo $this->helper('catalog/image')->init($_product, 'small_image')->resize(140); ?>" width="140" height="140" alt="<?php echo $this->htmlEscape($this->getImageLabel($_product, 'small_image')) ?>" />
    </a>
</p>
</code>

<li>Add this Javascipt before the close "body" tag:</li>

<code class="php">
<script type="text/javascript" src="<?php echo $this->getSkinUrl('js/productInfo.js') ?>"></script>
<script type="text/javascript">
Event.observe(window, 'load', function() {
    new ProductInfo('.ajax', '.product-image', {
        'loader': "<?php echo $this->getSkinUrl('images/ajax_loader.gif')?>",
        'loadingMessage': "<?php echo $this->__('Loading') ?>"
    });
});
</script>
</code>
