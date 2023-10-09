---
title: Apache Passwortschutz für Verzeichnisse
date: 2010-03-04T13:03:53+00:00
categories:
  - Linux/Unix
  - Webseiten
tags:
  - Apache
  - Einstellungen
  - htaccess
  - Linux
  - Nano
  - Server
  - web
  - Zugriff
import: sebastian.thiele.me
---
Es ist ja super einfach eine relativ vernünftige Passwortabfrage für ein zu sicherndes Verzeichnis über den Apache (Webserver) einzurichten. Aber dennoch gibt es da ein paar Stolperstellen. Wie so eine Passwortabfrage eingerichtet wird möchte ich hier mal ganz einfach zeigen.

Ich gehe im folgenden davon aus, dass euer www-root Verzeichnis im Verzeichnis _/var/www_ auf einem linuxrechner liegt un der Apache ordnungsgemäß installiert ist und arbeitet. Das zu schützende Verzeichnis soll in diesem Beispiel &#8220;save&#8221; heißen und liegt im www-root: _/var/www/save_

**Schritt 1: Die Datei .htaccess wird angelegt**

Im Verzeichnis &#8220;save&#8221; legt ihr mit eurem lieblings Editor die Datei mit namen .htaccess an. (Der Punkt am anfang bedeutet es ist eine versteckte Datei)

<pre>nano .htaccess</pre>

In diese Datei schreibt ihr folgenden Inhalt:

<pre>AuthType Basic
AuthName "Administrationsbereich"
AuthUserFile /var/www/save/.htpasswd
require valid-user</pre>

AuthType &#8211; gibt die Art der Anmeldung an. Basic ist dabei wirklich eine einfache Methode die <span style="color: #ff0000;">Usernamen und Passwort unverschlüsselt überträgt</span>.
  
AuthName &#8211; Ist frei wählbar. Es Wird dem Benutzer angezeigt wenn er/sie versucht sich anzumelden.
  
AuthUserFile &#8211; Hier muss der Pfad eingetragen werden den die Datei hat, die unter Schritt 2 angelegt wird.
  
require valid-user &#8211; sagt nur aus, dass sich nur ordentlich angemeldete (also bekannte) Benutzer rein dürfen.

**Schritt 2: Anlegen der Passwortdatei**

Die Passwortdatei in diesem Beispiel soll .htpasswd heißen. Aber der Name und der Ort ist frei wählbar. Es wird dazu geraten die Passwortdatei nicht in einem Ordner unterhalt des www-root zu speichern. In den meisten Fällen reicht es aber wenn sie im gleichen Verzeichnis wie die .htaccess liegt. Zum erstmaligen erstellen einer Passwortdatei verwendet man den Parameter -c (Create) des Programmes htpasswd, welches mit einer Apacheinstallation mitgeliefert wird. (ab und an auch htpasswd2 genannt). Bei allen weiteren Usern, die eingetragen werden sollen reicht es einfach nur htpasswd aufzurufen.

<pre>htpasswd -c /var/www/save/.htpasswd BENUTZER1</pre>

Im anschluss wird man nach einem Passwort gefragt und muss dieses wiederholen

<pre>htpasswd /var/www/save/.htpasswd BENUTZER2</pre>

Wie beschrieben, wenn die passwortdatei schon da ist ohne -c. Schaut man sie die Datei nun mit einem Editor an, sieht man den Inhalt im Muster:

Benutzername:VerschlüsseltesPasswort

Alles OK.

**Schritt 3: Das wars ja schon**

Jetzt sollte bei einem Aufruf des Ordners (http://server/save) Eine Passwortabfrage kommen.

**Dinge die Probleme machen könnten**

Es gibt da noch die ein oder andere Sache die dem ganzen im Wege stehen kann. Unter anderem kann bei einem ganz frisch aufgesetzten Apache die Einstellung **AllowOverride None** gesetzt sein. Das muss geändert werden in **AllowOverride All**, sonst werden .htaccess Einstellungen ignoriert. Diese Einstellung ist zu finden entweder in **/etc/apache2/httpd.conf**, **/etc/apache2/apache2.conf** oder **/etc/apache2/sites-enable/*** . Beim letzteren handelt es sich um die Einstellungen für Virtuelle Hosts. Wenn man keine hat ist es im Normalfall die Datei **000-default**. Wenn änderungen an einer dieser Dateien vorgenommen wurde, muss der Apache neu geladen (nicht unbedingt neu gestartet werden)

<pre>/etc/init.d/apache2 reload</pre>

sollte reichen.

Es sollte aber unbedingt daran gedacht werden, dass dies keine absolut sichere Sache ist. Der Benutzername und das Passwort werden unverschlüsselt (also im Klartext) über das Internet verschickt und können von allen, die wissen wie es geht mit gelesen werden.

Und wenn die passwort-Datei in einem webverzeichnis liegt sollte in diesem Verzeichnis auch eine .htaccess angelegt werden, in die folgendes Geschrieben werden sollt:

<pre>&lt;Files .ht*&gt;
   deny from all
&lt;/Files&gt;</pre>

Damit wird der Zugriff auf die Passwortdateien verboten.