---
layout: post
title: NetBeans aborts when adding remote connection
created: 1314183218
comments: true
categories: [java]
---
I'm trying to add a new remote server connection for one of my PHP project in NetBeans. I'm using NetBeans on Arch Linux 64bit, but the problem still exists on x86.

I right click on the project, go to properties, "Run Configuration" > "Manage..." > "Add", fill up fields. Every time I hit "Ok", it automatically crashes. Here is the console output when this occurs:

<code>
$ netbeans
process 6063: arguments to dbus_message_iter_append_basic() were incorrect, assertion "*bool_p == 0 || *bool_p == 1" failed in file dbus-message.c line 2549.
This is normally a bug in some application using the D-Bus library.
  D-Bus not built with -rdynamic so unable to print a backtrace
/usr/share/netbeans/bin/../platform/lib/nbexec: line 548:  6063 Aborted                 "/opt/java/bin/java" -Djdk.home="/opt/java" -classpath "/usr/share/netbeans/platform/lib/boot.jar:/usr/share/netbeans/platform/lib/org-openide-modules.jar:/usr/share/netbeans/platform/lib/org-openide-util.jar:/usr/share/netbeans/platform/lib/org-openide-util-lookup.jar:/usr/share/netbeans/platform/lib/locale/boot_ja.jar:/usr/share/netbeans/platform/lib/locale/boot_pt_BR.jar:/usr/share/netbeans/platform/lib/locale/boot_ru.jar:/usr/share/netbeans/platform/lib/locale/boot_zh_CN.jar:/usr/share/netbeans/platform/lib/locale/org-openide-modules_ja.jar:/usr/share/netbeans/platform/lib/locale/org-openide-modules_pt_BR.jar:/usr/share/netbeans/platform/lib/locale/org-openide-modules_ru.jar:/usr/share/netbeans/platform/lib/locale/org-openide-modules_zh_CN.jar:/usr/share/netbeans/platform/lib/locale/org-openide-util_ja.jar:/usr/share/netbeans/platform/lib/locale/org-openide-util-lookup_ja.jar:/usr/share/netbeans/platform/lib/locale/org-openide-util-lookup_pt_BR.jar:/usr/share/netbeans/platform/lib/locale/org-openide-util-lookup_ru.jar:/usr/share/netbeans/platform/lib/locale/org-openide-util-lookup_zh_CN.jar:/usr/share/netbeans/platform/lib/locale/org-openide-util_pt_BR.jar:/usr/share/netbeans/platform/lib/locale/org-openide-util_ru.jar:/usr/share/netbeans/platform/lib/locale/org-openide-util_zh_CN.jar:/opt/java/lib/dt.jar:/opt/java/lib/tools.jar" -Dnetbeans.dirs="/usr/share/netbeans/nb:/usr/share/netbeans/ergonomics:/usr/share/netbeans/ide:/usr/share/netbeans/java:/usr/share/netbeans/bin/../xml:/usr/share/netbeans/apisupport:/usr/share/netbeans/bin/../webcommon:/usr/share/netbeans/websvccommon:/usr/share/netbeans/enterprise:/usr/share/netbeans/mobility:/usr/share/netbeans/profiler:/usr/share/netbeans/bin/../ruby:/usr/share/netbeans/bin/../python:/usr/share/netbeans/php:/usr/share/netbeans/bin/../visualweb:/usr/share/netbeans/bin/../soa:/usr/share/netbeans/bin/../identity:/usr/share/netbeans/bin/../uml:/usr/share/netbeans/harness:/usr/share/netbeans/cnd:/usr/share/netbeans/dlight:/usr/share/netbeans/groovy:/usr/share/netbeans/bin/../extra:/usr/share/netbeans/bin/../javafx:/usr/share/netbeans/javacard:" -Dnetbeans.home="/usr/share/netbeans/platform" '-Dnetbeans.importclass=org.netbeans.upgrade.AutoUpgrade' '-Dnetbeans.accept_license_class=org.netbeans.license.AcceptLicense' '-XX:MaxPermSize=384m' '-Xmx656m' '-client' '-Xss2m' '-Xms32m' '-XX:PermSize=32m' '-Dapple.laf.useScreenMenuBar=true' '-Dapple.awt.graphics.UseQuartz=true' '-Dsun.java2d.noddraw=true' org.netbeans.Main --userdir "/home/zoner/.netbeans/7.0" "--branding" "nb" 0<&0
</code>

Solution:

We should disable this dbus operation while Java not supports correctly. The problem is in the keyring's dbus call. Open the file /usr/share/netbeans/etc/netbeans.conf and add this option to the end of netbeans_default_options variable:

<code>
-J-Dnetbeans.keyring.no.native=true
</code>

Mine looks like this after this hack:

<code>
netbeans_default_options="-J-client -J-Xss2m -J-Xms32m -J-XX:PermSize=32m -J-Dapple.laf.useScreenMenuBar=true -J-Dapple.awt.graphics.UseQuartz=true -J-Dsun.java2d.noddraw=true -J-Dnetbeans.keyring.no.native=true"
</code>
