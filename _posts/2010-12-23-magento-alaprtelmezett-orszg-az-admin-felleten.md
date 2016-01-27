---
layout: post
title: Magento alapértelmezett ország
created: 1293100138
comments: true
categories: [magento]
---
Az ügyfélszolgálatos munkatárs dolgának megkönnyítése, hogy ne kelljen kiválasztania országot, így az alapértelmezettként beállított ország lesz kiválasztva mindenképpen.

<pre>app/design/adminhtml/default/default/template/sales/order/create/form/address.phtml</pre> fájl végére:

<javascript>
<script type="text/javascript">
//<![CDATA[
function _set_default_country() {
<?php 
echo '    
    var default_country="'.Mage::getStoreConfig('general/country/default').'";
    ';
?>
    if ('' == $('order-billing_address_country_id').value) {
        $('order-billing_address_country_id').value = default_country;
    }
    if ('' == $('order-shipping_address_country_id').value) {
        $('order-shipping_address_country_id').value = default_country;
    }
  setTimeout(_set_default_country,1000);
}
setTimeout(_set_default_country,1000);
//]]>
</script>
</javascript>

<pre>app/design/adminhtml/default/default/template/customer/tab/addresses.phtml</pre> 312-es sor környékén lévő setActiveItem() funkció végére:

<javascript>
        <?php 
        echo '    
        var default_country="'.Mage::getStoreConfig('general/country/default').'";
        ';
        ?>
        var item_html_id = '';
        if ('address_item_' == item.id.substr(0,13)) {
            item_html_id = 'id'+item.id.substr(13);
        } else if ('new_item' == item.id.substr(0,8)) {
            item_html_id = '_item'+item.id.substr(8);
        }
        if ('' == $(item_html_id+'country_id').value) {
            $(item_html_id+'country_id').value = default_country;
        }
</javascript>

