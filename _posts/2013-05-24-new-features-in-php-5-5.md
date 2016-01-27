---
layout: post
title: New features in PHP 5.5
created: 1369425457
comments: true
categories: [php]
---
The latest minor version, PHP 5.4 was released more than a year ago, it's time to finish and release PHP 5.5. Actually it's in Release Candidate phase, but there are no new features coming. Let's see what's new in PHP 5.5 and how can we use them:

<h3>Generators</h3>

It's an exciting new feature, a generator function looks just like a normal function, except that instead of returning a value, a generator yields as many values as it needs to. The main benefit of this we don't need to create an array to pass it to a foreach loop, it's possible to generate the return values while the loop is running. By such this way, it is saving a lot of memory and CPU time. A quick real world example:

<code class="php">
// In the block class:
function formatRows($collection)
{
    foreach($collection as $item) {
        $row['name'] = sprintf("%s %s %s", $item->title, $item->firstName, $item->lastName);
        $row['order_id'] = $item->orderId;
        $row['products'] = array();
        foreach($item->getProducts() $product) {
            /* Complicated code */
            $row['products'][] = $productRow;
        }
        yield $row;
    }
}
</code>

<code class="php">
// In the template:
<div class="orderlist">
    <?php php foreach($this->formatRows($orders) as $order): ?>
        <div class="order">
            <span class="name"><?php echo $order['name'] ?></span>
            <span class="orderid"><?php echo $order['order_id'] ?></span>
            <?php echo productListTemplate($order['products']); ?>
        </div>
    <?php endforeach; ?>
</div>
</code>

Technically the formatRows function is returning with an array and can be used to give a value for an array variable. But in a for loop, it is yielding a value without allocating memory for a big array. In such way we can separate different logics into different places. Once there are no more values to be yielded, then the generator function can simply exit, and the calling code continues just as if an array has run out of values. Mixing return and yield is not allowed with an exception of an empty return statement which will exit the function and terminate the generator. The idea is a lightweight implementation of Java's Iterator classes without of the overhead of the class inheritance.

<h3>finally</h3>

Finally we have a finally keyword in the try..catch blocks. Awesome. We cried for it since years ago: http://www.czettner.com/blog/10/12/10/why-there-no-finally-statement-php

<h3>'String'[2]</h3>

'String'[2] now will return 'r', I have a feeling they have stolen this feature from ruby.

<h3>ClassName::class</h3>

ClassName::class now will return the proper fully qualified class name. I have python or ruby feeling again.

<h3>GD library improvements</h3>

Flipping support using the new imageflip() function. Advanced cropping support using the imagecrop() & imagecropauto() functions. This was the feature every application should have implemented, now it will be supported natively. WebP read and write support using imagecreatefromwebp() & imagewebp() respectively.
