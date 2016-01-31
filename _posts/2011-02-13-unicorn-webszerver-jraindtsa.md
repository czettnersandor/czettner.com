---
layout: post
title: Unicorn webszerver újraindítása
created: 1297618995
comments: true
categories: [rails]
---
Pontosan azért használja a Twitter is az Unicorn webszervert, mert alapból nagyon jól skálázható és anélkül indítható újra, hogy a szolgáltatás kiesne akár rövid időre is. Ilyen módon akár új Unicorn binárist is lehet telepíteni leállítás nélkül. Nézzük meg, hogyan.

Indításkor létrejön egy pid file, ahonnan kiolvashatjuk a master process id-jét, majd ennek a processznek kell küldeni egy kill signal-t így:

{% highlight bash %}
PID=`cat /var/www/turaindex/shared/pids/unicorn.pid`
kill -USR2 $PID
{% endhighlight %}
A példában a $PID a process id, amit a pid fájlból olvastunk ki. A továbbiakban is használjuk majd, mivel az -USR2 szignál után létrejött még egy master példány, ami felülírta ezt a fájlt.

A ps faux kimenetén látszik, hogy létrejött a master alatt egy új master webszerver:

{% highlight html %}
18:10   0:07 unicorn_rails master (old) -c /etc/turaindex.rb -E production -D
18:11   0:00  \_ unicorn_rails worker[0] -c /etc/turaindex.rb -E production -D
18:11   0:00  \_ unicorn_rails worker[1] -c /etc/turaindex.rb -E production -D
18:12   0:07  \_ unicorn_rails master -c /etc/turaindex.rb -E production -D
18:12   0:00      \_ unicorn_rails worker[0] -c /etc/turaindex.rb -E production -D
18:12   0:00      \_ unicorn_rails worker[1] -c /etc/turaindex.rb -E production -D
{% endhighlight %}

Miután megvan, meg kell mondani az eredeti master-nek, hogy az ő worker processzei már ne szolgálják ki a látogatókat. Fontos, hogy az új master worker processzeinek indítását meg kell várni, ezért ha ezt scriptből futtatjuk, néhány másodperc szünet kell a WINCH előtt:

{% highlight html %}
kill -WINCH $PID
{% endhighlight %}

Ezután az eredeti master alól törlődnek a worker processzek, ami a ps faux kimenetén is jól látható:

{% highlight html %}
18:10   0:07 unicorn_rails master (old) -c /etc/turaindex.rb -E production -D
18:12   0:07  \_ unicorn_rails master -c /etc/turaindex.rb -E production -D
18:12   0:00      \_ unicorn_rails worker[0] -c /etc/turaindex.rb -E production -D
18:12   0:00      \_ unicorn_rails worker[1] -c /etc/turaindex.rb -E production -D
{% endhighlight %}

Nincs más hátra, mint az eredeti master kiléptetése. A -QUIT signal hatására az új master fogja átvenni a helyét.

{% highlight html %}
$ kill -QUIT $PID
$ ps faux
18:12   0:07  unicorn_rails master -c /etc/turaindex.rb -E production -D
18:12   0:00   \_ unicorn_rails worker[0] -c /etc/turaindex.rb -E production -D
18:12   0:00   \_ unicorn_rails worker[1] -c /etc/turaindex.rb -E production -D
{% endhighlight %}

Meg is volnánk. A látogatók az újraindításból semmit nem vettek észre. Nekem egy capistrano alapú deploying után kellett kicserélnem az Unicorn-t, ezért egy deploy task-ba tettem ezt a sort:

{% highlight ruby %}
run "HI=`cat /var/www/turaindex/shared/pids/unicorn.pid`; #{try_sudo} kill -USR2 $HI; #{try_sudo} kill -WINCH $HI; sleep 4; #{try_sudo} kill -QUIT $HI"
{% endhighlight %}
