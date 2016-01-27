---
layout: post
title: What's new in ruby 1.9.3?
created: 1317404512
comments: true
categories: [ruby]
---
Ruby 1.9.3 is still in preview state, but I think no new improvements will come to the final release. 1.9.2 is the complete rethinking of the 1.9 design. 1.9.3 is only a "better implementation" of it, as Yuki 'yugui' Sonoda said at RubyConf Taiwan (held August 26-27, 2011). Let's see what's new in 1.9.3.
<ul>
<li>Licence was changed to joint BSD, because GPLv3 changed to force a rethink</li>
<li>New "Lazy Garbage Collector" allow code to improve response time</li>
<li>Environment variables can tune Garbage Collector</li>
<li>test/unit parallelization to improve the testing speed from stdlib</li>
<li>Faster loading (thanks for the load.c patch)</li>
<li>Random generator tweaks and rand() now accepts ranges</li>
<li>io/console, a new library in the stdlib</li>
<li>A number of tweaks to formatting strings and date formats</li>
</ul>

Yugui said, the 1.8 era's time is up, so You should switch to 1.9. That means no more bugfixes will come to 1.8, but that will be supported for more few years.

I bought the Ruby 1.9 walkthrough, I feel like a Ruby Kung-Fu after viewed it :)

Recomended: http://rubyinside.com/19walkthrough/
