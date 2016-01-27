---
layout: post
title: Raspberry Pi USB Hard Drive stand by problem
created: 1365804245
comments: true
categories: [raspberry pi]
---
I formatted my 2TB drive with ext3 file system, then realised it was a bad idea. Since ext3 and ext4 is a journaling filesystem, there is a strange activity in every 5-10 seconds, making it impossible to spin down the hard drive when not in use. To turn an ext3 file system to ext2, I simply did this:

<pre>tune2fs -O ^has_journal /dev/sda1</pre>

at terminal after unmounted the drive first.

The description of the -O switch from the man page:

<code class="no-highlight">
-O [^]feature[,...]
              Set or clear the indicated filesystem features (options) in the  filesystem.   More  than
              one  filesystem  feature  can  be  cleared  or  set  by  separating features with commas.
              Filesystem features prefixed with a caret character ('^') will be cleared in the filesys‐
              tem's  superblock; filesystem features without a prefix character or prefixed with a plus
              character ('+') will be added to the filesystem.
</code>

This will make the ext3 filesystem to ext2 (non journalling ext3) in around 10 minutes.

Then I set the suspend timeout to 180s:

hdparm -S 180 /dev/sda

Now the drive spins down during the night and keep warm when needed, because Samba is continuously reading the mounted network shares.
