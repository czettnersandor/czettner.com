---
layout: post
title: Apache és fcgid
created: 1295034026
comments: true
categories: [php]
---
Jelentős gyorsulás érhető el fcgid segítségével és egy költözés kapcsán sikerült találni egy olyan optimális beállítást, ami az éppen aktuális terhelésnél remek teljesítményt ad. Íme a régi, kisebb szerveren futó config:

Debian Lenny, /etc/apache2/conf.d/php-fcgid.conf
<code>
  AddHandler fcgid-script .fcgi .php
  # Where to look for the php.ini file?
  DefaultInitEnv PHPRC        "/etc/php5/cgi"
  # Maximum requests a process handles before it is terminated
  MaxRequestsPerProcess       1000
  # Maximum number of PHP processes
  MaxProcessCount             10
  # Number of seconds of idle time before a process is terminated
  IPCCommTimeout              240
  IdleTimeout                 240
  #Or use this if you use the file above
  FCGIWrapper /usr/bin/php-cgi .php

  ServerLimit           500
  StartServers            3
  MinSpareThreads         3
  MaxSpareThreads        10
  ThreadsPerChild        10
  MaxClients            300
  MaxRequestsPerChild  1000
</code>

A költözés után ezzel tudtuk a napi terhelésnek megfelelően a memória kihasználtságot magasan tartani és még maradt bőven hely diskcache és buffer számára:

<code>
  AddHandler fcgid-script .fcgi .php
  # Where to look for the php.ini file?
  DefaultInitEnv PHPRC  "/etc/php5/cgi"
  # Where is the PHP executable
  FCGIWrapper /usr/bin/php-cgi .php
  # Maximum requests a process handles before it is terminated
  MaxRequestsPerProcess 1500
  # Maximum number of PHP processes.
  MaxProcessCount       45
  # Number of seconds of idle time before a process is terminated
  IPCCommTimeout        240
  IdleTimeout           240

  ServerLimit           100
  ThreadLimit            10
  StartServers           10
  MinSpareThreads        10
  MaxSpareThreads        50
  ThreadsPerChild        20
  MaxClients            800
  MaxRequestsPerChild  5000
</code>
