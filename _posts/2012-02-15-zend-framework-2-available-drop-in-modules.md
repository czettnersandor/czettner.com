---
layout: post
title: Zend Framework 2 available drop-in modules
created: 1329340984
comments: true
categories: [zend]
---
Zend Framework 2 will be released soon, the beta versions are in the round of the corner. The new version provides a programmer-friendly module development architechture and there are some modules available at present. ZF2 module system is very young; and as such, there are not many modules yet.

<li>
<span><a href="https://github.com/doctrine/DoctrineModule">Doctrine</a></span>
Well discussed issue on the forums is the Doctrine issue. As the Doctrine2 will not packaged with ZF2, this module provides a bridge between the framework and Doctrine. By the way, its more than a bridge. Now supports <a href="https://github.com/doctrine/DoctrineMongoODMModule">MongoDB</a> and an easy-to-use <a href="https://github.com/doctrine/DoctrineORMModule">ORM</a> is available for them. It is a great leap forward, it isn't?
</li>
<li>
<span><a href="https://github.com/DASPRiD/Bacon">Bacon</a></span>
Provides a unidecoder/slugifier, as well as a PHP port of 'highlight'.
</li>
<li>
<span><a href="https://github.com/EvanDotPro/EdpSuperluminal">EdpSuperluminal</a></span> Provides a cache for your classes and a package mechanism to a single include file which will boost your application.
</li>
<li>
<span><a href="https://github.com/ZF-Commons/ZfcUser">ZfcUser</a></span>
The really drop-in module which provides a quick and secure user management solution for your application. It is designed by Evan Coury, the lead developer behind the ZF2 module system. It has a very elegant <a href="https://github.com/ZF-Commons/ZfcUserDoctrineORM">MongoDB</a> integration as well.
</li>
<li>
<span><a href="https://github.com/widmogrod/zf2-assetic-module">Assetic Module</a></span>
Provides a great asset management framework with integration with the well-known <a href="https://github.com/kriswallsmith/assetic">Assetic</a>.
</li>

And at the end, here is some great slides about the ZF2 module architechture from Evan Coury: http://evan.pro/zf2-modules-talk.html
