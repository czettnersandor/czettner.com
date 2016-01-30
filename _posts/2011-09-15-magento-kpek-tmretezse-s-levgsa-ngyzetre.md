---
layout: post
title: Magento képek átméretezése és levágása négyzetre
created: 1316074675
comments: true
categories: [magento]
---
Sajnos ez a hasznos funkció hiányzik belőle. Alapból nem tud úgy átméretezni képeket, hogy az arányokat megtartva kockára vágja le azokat. Létezik a Varien_Image osztályban egy crop() funkció, de valamiért az nem elérhető a Mage_Catalog_Helper_Image osztályból. Tehát a kezdeményezés valószínűleg megvan, de a közösség nem valósította még meg.

Nézzük, mely fájlokat kell módosítani:

app/design/frontend/default/default/template/catalog/product/list.phtml
app/code/core/Mage/Catalog/Helper/Image.php 
app/code/core/Mage/Catalog/Model/Product/Image.php 
lib/Varien/Image.php 
lib/Varien/Image/Adapter/Gd2.php

az /app/code/core és lib könyvtárban lévő dolgokat egyszerűen másoljuk ki az app/code/local alá és ott szerkesszük meg, ne közvetlenül a Magento-s fájlokat írjuk át.

Az első fájl a saját, éppen használt template könyvtárunkból jön, tehát nem biztos, hogy pont ugyanez az elérési út.

app/code/local/Mage/Catalog/Model/Product/Image.php fájlba:
<code class="php">
    public function adaptiveResize()
    {
        if (is_null($this->getWidth()) && is_null($this->getHeight())) {
            return $this;
        }
        $this->getImageProcessor()->adaptiveResize($this->_width, $this->_height);
        return $this;
    }
</code>

app/code/local/Varien/Image.php fájlba:

<code class="php">
    /**
    * Adaptive resize an image
    *
    * @param int $width
    * @param int $height
    * @access public
    * @return void
    */
    public function adaptiveResize($width, $height = null)
    {
        return $this->_getAdapter()->adaptiveResize($width, $height);
    }
</code>

app/code/local/Varien/Image/Adapter/Gd2.php fájlba:
<code class="php">
    /**
     * Change the image size down to a best match then crop from the center
     *
     * @param int $frameWidth
     * @param int $frameHeight
     */
    public function adaptiveResize($frameWidth = null, $frameHeight = null) {
        if (empty($frameWidth) && empty($frameHeight)) {
            throw new Exception('Invalid image dimensions.');
        }
        $widthDistance = $this->_imageSrcWidth - $frameWidth;
        $heightDistance = $this->_imageSrcHeight - $frameHeight;
        if (($frameWidth / $frameHeight) > ($this->_imageSrcWidth / $this->_imageSrcHeight)) {
            $this->resize($frameWidth, null);
        } else {
            $this->resize(null, $frameHeight);
        }
        $cropX = 0;
        $cropY = 0;
        if ($this->_imageSrcWidth > $frameWidth) {
            $cropX = intval(($this->_imageSrcWidth - $frameWidth) / 2);
        } elseif ($this->_imageSrcHeight > $frameHeight) {
            $cropY = intval(($this->_imageSrcHeight - $frameHeight) / 2);
        }
        $isAlpha = false;
        $isTrueColor = false;
        $this->_getTransparency($this->_imageHandler, $this->_fileType, $isAlpha, $isTrueColor);
        if ($isTrueColor) {
            $newImage = imagecreatetruecolor($frameWidth, $frameHeight);
        } else {
            $newImage = imagecreate($frameWidth, $frameHeight);
        }
        $this->_fillBackgroundColor($newImage);
        imagecopyresampled($newImage, $this->_imageHandler, 0, 0, $cropX, $cropY, $frameWidth, $frameHeight, $frameWidth, $frameHeight);
        $this->_imageHandler = $newImage;
        $this->refreshImageDimensions();
    }
</code>

app/code/local/Mage/Catalog/Helper/Image.php fájlba:
<code class="php">
    // A class első sora alá a többi változó deklarációhoz:
    protected $_scheduleAdaptiveResize = false;

    // A funkciókhoz
    public function adaptiveResize($width, $height = null)
    {
        $this->_getModel()->setWidth($width)->setHeight($height);
        $this->_scheduleAdaptiveResize = true;
        return $this;
    }
</code>

az alábbi fájlban van egy ilyen sor:
app/design/frontend/default/default/template/catalog/product/list.phtml
<code class="php">
<a href="<?php echo $_product->getProductUrl() ?>" title="<?php echo $this->stripTags($this->getImageLabel($_product, 'small_image'), null, true) ?>" class="product-image"><img src="<?php echo $this->helper('catalog/image')->init($_product, 'small_image')->resize(170); ?>" width="170" height="170" alt="<?php echo $this->stripTags($this->getImageLabel($_product, 'small_image'), null, true) ?>" /></a>
</code>

Ezt erre kell cserélni:
<code class="php">

</code>
