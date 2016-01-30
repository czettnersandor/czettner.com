---
layout: post
title: Exposed .git folders on the internet
created: 1437944313
comments: true
categories: [php]
---
One of a common novice mistake of web developers is exposing the .git folder to the world, making the whole git repository silently downloadable by anyone. Often the git repository contains hashes, passwords, database dumps, even FTP details to the webserver which hosts it.

80% of websites I took over had this security hole, basically it's the best chance to prove the customer that the decision they made to switch agency was a good one.

Fortunately I'm not involved, but one prominent gay lobby group exposed every person who had signed up to a gay lobby campaign, publicly downloadable from their website. Including full names and email addresses.

So developers, please check if the .git folder is accessible by the public and protect it if needed. Please don't even use git hooks to deploy to the production server, but use <a href="http://capistranorb.com/">Capistrano</a> or <a href="http://deployer.org/">Deployer</a>. There are many commercial products out there as well if you need support.
