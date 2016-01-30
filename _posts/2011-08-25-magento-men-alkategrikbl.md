---
layout: post
title: Magento - menü alkategóriákból
created: 1314257851
comments: true
categories: [magento]
---
Egy egyszerű blokk Magentohoz, ami az aktuális kategória alkategóriáit írja ki. Ha nincs, akkor a szülő alkategóriáit.

<code class="php">
<?php
$_category = $this->getCurrentCategory();
$collection = Mage::getModel('catalog/category')->getCategories($_category->entity_id);
$helper = Mage::helper('catalog/category');

/**
 * Ha nincs alatta kategória, akkor a szülő kategóriáit írja ki
 */
if (!$collection->count()) {
    $collection = Mage::getModel('catalog/category')->getCategories($_category->getParentCategory()->entity_id);
}
    
?>

<?php if ($collection->count()): ?>

    <div class="block block-rightcategories">
        <div class="block-title">
            <strong><span>Menü</span></strong>
        </div>
        <div class="block-content">
            <ul>
                <? foreach ($collection as $cat): ?>
                    <?php if ($_category->getIsActive()): ?>
                        <?php
                        $cur_category = Mage::getModel('catalog/category')->load($cat->getId());
                        ?>
                        <li class="item">
                            <a href="<?php echo $helper->getCategoryUrl($cat); ?>">
                                <?php echo $cat->getName(); ?>
                            </a>
                        </li>
                    <?php endif ?>
                <?php endforeach; ?>
            </ul>
        </div>
    </div>
<?php endif; ?>
</code>
