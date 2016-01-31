---
layout: post
title: Concrete 5 default collation
created: 1306789868
comments: true
categories: [concrete5]
---
Előfordul, hogy elfelejti valaki adatbázis létrehozásakor az alapértelmezett collationt beállítani (persze nem én, mindig csak más :)). Jobb esetben ez azonnal kiderül, de van, hogy csak az első ő betűnél derül ki és bosszankodunk az eredményen. kiexportálni az egész mysql adatbázist, szövegszerkesztővel javítani a hibákat nagyon nagy és nem túl elegáns munka, helyette itt ez a rövid php script, ami fél perc alatt megoldja a problémát:

{% highlight php %}

$host='localhost'; // Az adatbázis-szerver
$user=' '; // mysql felhasználónév
$pass=' '; // jelszó
$dbname=' '; // adatbázis neve
$charset='utf8'; // character set
$collation='utf8_general_ci'; // collation

$db = mysql_connect("$host","$user","$pass") or die("mysql could not CONNECT to the database, in correct user or password " . mysql_error());
mysql_select_db("$dbname") or die("Mysql could not SELECT to the database, Please check your database name " . mysql_error());
$result=mysql_query('show tables') or die("Mysql could not execute the command 'show tables' " . mysql_error());
while($tables = mysql_fetch_array($result)) {
foreach ($tables as $key => $value) {
mysql_query("ALTER TABLE $value CONVERT TO CHARACTER SET $charset COLLATE $collation") or die("Could not convert the table " . mysql_error());
}}
mysql_query("ALTER DATABASE $dbname DEFAULT CHARACTER SET $charset COLLATE $collation") or die("could not alter the collation of the databse " . mysql_error());
echo "The collation of your database has been successfully changed!";

{% endhighlight %}

Használható bármilyen collation és charset cserére, de a kérdőjeleket nekünk kell majd kiszedni, hiszen azt már úgy tárolta el a mysql.
