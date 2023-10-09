---
title: Apache UserDir (public_html) im Home Verzeichnis
date: 2009-01-14T12:44:26+00:00
categories:
  - Anletungen und Tipps
  - Linux/Unix
tags:
  - Apache
  - Apache UserDir
  - Server
  - user
  - web
import: sebastian.thiele.me
---
Benutzern einen eigenen Webspace auf einem Linux-/ Unix-System bereit zu stellen ist bei vielen Multiuser-Systemen eine defacto Standard. Erreichbar ist diese Webpacge dann unter der URL http://serveradresse/~username/. Wie man dies einrichtet und für welche Zwecke dies nützlich sein kann wird im folgenden beschrieben.

Eigentlich wollte ich auf einigen Servern ein paar Log-Files veröffentlichen um direkten Zugriff darauf zu haben. Dazu wollte ich dem Benutzer einen eigenen Webspace zur verfügung stellen um die Logs darauf abzulegen. Damit wollte ich mir viel administrativen Aufwand sparen. Doch sitze ich jetzt schon eine ganze Weile daran dieses Problem zu Lösen. Ich konnte mich auch nicht mehr daran erinnern, dass es so schwer war dem Benutzer eines Servers ein eigenes Webverzeichnis in sein Home Verzeichnis zu legen. 

(Im weiteren Verlauf gehe ich davon aus, das das Homeverzeichnis unter /home/USER/ liegt, und das USER der entsprechende Benutzer ist) 

Es ist auch eine wirklich leichte Sache, wenn man nicht eine Sache vergisst wie ich. Im folgenden das Vorgehen.

Im Grunde ist nichts anderes zu erledigen als das setzen von 2 Symbolischen Links.
  
Im Verzeichnis /etc/apache2/mods-available sollten zwei Dateien enthalten sein:

<pre lang="bash"># ls -la | grep userdir
-rw-r--r-- 1 root root  293 2008-03-22 10:24 userdir.conf
-rw-r--r-- 1 root root   66 2008-03-22 10:24 userdir.load</pre>

Diese Beiden Dateien müssen mit hilfe von symbolischen Links ins Verzeichnis /etc/apache2/mods-enabled (wie sagt man) verlinkt werden.

<pre lang="bash">ln -s /etc/apache2/mods-available/userdir.conf /etc/apache2/mods-enabled/userdir.conf
ln -s /etc/apache2/mods-available/userdir.load /etc/apache2/mods-enabled/userdir.load</pre>

am ende sollte es dann so aussehen:

<pre lang="bash"># ls -la | grep userdir
lrwxrwxrwx 1 root root   40 2008-08-21 15:27 userdir.conf -&gt; /etc/apache2/mods-available/userdir.conf
lrwxrwxrwx 1 root root   40 2008-08-21 15:29 userdir.load -&gt; /etc/apache2/mods-available/userdir.load</pre>

Im grunde sollte es das dann (nach einem neustart des Servers) (/etc/init.d/apache2 restart) gewesensein.

Wie mir heute mitgeteilt wurde (leider wurde es mir nicht begründet) sollte man alle Einstellungen für die Module tunlichst NICHT in der /etc/apache2/apache2.conf ändern. Wenn man änderungen vornehmen möchte, dann sollte das in der .conf Datei des Moduls machen. in dem fall dann /etc/apache2/mods-enabled/userdir.conf (was ja ein Link ist)

So das sollte dann wohl gehen!