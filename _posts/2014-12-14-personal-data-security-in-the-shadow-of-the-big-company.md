---
layout: post
title: Personal data security in the shadow of the big company
created: 1418585987
comments: true
---
All my non-technical frineds or friends with limited technical abilities are very fed-up with how apple, android, or any other buzzword nonsense work. I constantly hear them chattering about how they would like to do something but stupid apple/android doesn't allow them to do that. A recent example of the other day was something about how itunes does not allow through parental control to disallow zombie apps to appear in their search results, showing zombies chewing people's heads in the app store for the kids.

I think the trend of walled-garden environments with app stores, the app-centric model of interaction, the fact that you are renting music streams rather than owning CD's/audio files and all the increasingly locked-down features of modern devices are evil, and it's one that it seem to be growing: with data being managed by and hidden behind apps and big companies, touted as a feature of convenience, the users of these new consumption-oriented devices are being distanced from direct control of their data, even hiding the fact that the data, music or application is consist of multiple files, therefore in theory could be shared between devices, re-encoded and modified for personal needs. But the app does not allow it. It tries to lock you down: you can only view the movie on your X branded phone, or an X branded TV device, or an X branded tablet. You can not transcode it to something else, for your own needs, you can not separate the audio from the movie and set it as a ringtone. However, you can buy the right to play a ringtone from a limited set of choices.

I can't believe what happened with a dead simple thing like music these days. I have a huge mp3/ogg/flac library. Some of it are illegal, but those will not survive the next spring cleaning. So if I like the music, I buy it anyway. My collection is carefully sorted and labelled. Other "normal people" instead have a lot of confusion of what is compatible with what and a lot of tears when people waste money on something that's incompatible. Say... a user without a PC who has a large iTunes in the Cloud library buying an Android phone. "Wait what do you mean I can't listen to my music any more?". From this point it's not only a security issue, but what the big company saying is if you don't buy my product, but you buy a product from somewhere else, you can't enjoy your music any more. This is blackmailing, but legally supported and pushed. This makes their content completely useless and worth no money at all for those who can think a little.

Apple can push updates silently to apple devices, without any question or even without notifying the user about it. Skype produces encrypted traffic even when you are not actively using Skype. It's binary is disguised against decompiling, so nobody is (still) able to reproduce what it really does. Dropbox confirmedly opens and reads your documents. These are serious security problems.

But there's a way out from this trap. Here's my setup anybody can use easily, even without hardcore technical or computer skills:

<ul>
<li>Video calls and chat: use a SIP client, I recommend <a href="http://www.linphone.org/">Linphone.</a> SIP is more mature and secure than Skype and really peer to peer with hard encryption. So nobody, unauthorised will listen in to your call.</li>
<li>Folder sharing: use <a href="http://www.getsync.com/">BitSync</a> or <a href="http://owncloud.org/">OwnCloud</a>.</li>
<li>Music: use FLAC, OGG or even MP3 file formats. These formats are not only compatible with all of your devices, but provides much better bitrate and quality than streamed music. There are tons of players available to play and sort your library.</li>
<li>Web search: <a href="https://duckduckgo.com/">DuckDuckGo</a></li>
<li>Always use a VPN or proxy to browse the web. I use <a href="https://www.torproject.org/">Tor</a> for everything. It's not only rendering the stupid UK site blockers useless, but encrypting and anonymising my traffic.</li>
<li>Encrypt all of your hard drives especially if it's a laptop. But don't use closed source software to do it as surely there is a reason why they are hiding it from hackers. Bitdefender, for example will decrypt your data for court orders, they don't need your password. I use Linux's built-in dm-crypt and ZFS's encryption in FreeBSD. These encryption methods proved to be safe even against secret agencies.</li>
<li>Don't let your children use DRM only, big company locked devices more than 30 minutes a day. Well, if you want to grow hackers rather than stupid consumers. I don't have any of these devices at home, so my kids are safe :)</li>
<li>QUESTION EVERYTHING!</li>
</ul>
