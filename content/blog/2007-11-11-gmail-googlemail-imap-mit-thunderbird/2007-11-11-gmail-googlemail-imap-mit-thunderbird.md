---
title: Gmail (Googlemail) IMAP mit Thunderbird
author: Sebastian
date: 2007-11-11T15:45:30+00:00
categories:
  - Anletungen und Tipps
tags:
  - Gmail
  - IMAP
  - Thunderbird
import: sebastian.thiele.me
---
**Problem:**

Ich habe es nun auch mal gemacht und habe meinen Gmail (ich bleibe mal dabei auch wenn es doch Googlemail heißt (oder doch nicht? ) ) auf Imap umgestellt. (wie man das macht wird auch weiter unten erklärt). Dabei trat bei mir aber ein kleines Problem auf&#8230;
  
Wenn ich dann meine Mails im Thunderbird gelöscht habe waren sie im Gmail immer noch da. Also was muss man machen? &#8230;


**Lösung 1 (Wie aktiviere ich IMAP unter GMAIL):**

Zu erst einmal überprüfen ob man die Option schon hat. Dazu geht man auf **Einstellungen** (de) und schaut ob bei <span class="psm"><strong>Weiterleitung und POP</strong> schon ein IMAP steht. Wenn ja gehts einfach weiter unten im Text weiter, wenn nicht dann muss man einen kleinen Trick anwenden.</span>

Trick: man stelle für das aktivieren von IMAP die Sprache von Gmail auf English (US) ein dann sollte die Option da sein. Einfach dann mal IMAP aktivieren.

Nach der Aktivierung richtet man nun Gmail IMAP bei Thunderbird ein. [Wie hier beschrieben.][1]

**Lösung 2 (Wie löse ich das Problem mit den Mails)**

Danach muss man noch ein paar weiter Einstellungen vornehmen um kein durcheinander mit der Online Version zu bekommen.

Unter den Kontoeinstellungen muss man noch den Ordner &#8220;Gesendet&#8221; und &#8220;Entwurf&#8221; mit den Ordnern von GMAIL verbinden. Dazu stellt man bei &#8220;Kopien & Ordner&#8221; die entsprechenden Sachen ein.

Dann muss man noch den Papierkorb / Trash mit dem von Gmail verbinden, dazu muss man etwas anders vorgehen. Dazu muss man die Konfiguration (über Extras -> Einstellungen -> Allgemein -> Konfiguration bearbeiten) dort filtert man nach **mail.server.server**. Dann merkt man sich die Zahl, die dem Konto zugewiesen wurde. (mail.server.server**X**.name X ist die Zahl)
  
Durch rechtsklick und **Neu** -> **String** kommt eine Eingabemaske in die man folgendes eingibt **mail.server.serverX.trash\_folder\_name** wobei das X natürlich durch deine eben gemerkte Zahl ersetzt wird. Nach OK kommt eine weitere Eingabe in die man folgendes eingibt (je nach passendem Kriterium)

  * [Gmail]/Trash (Wenn man Gmail hat und die Englische Sprache verwendet)
  * [Gmail]/Papierkorb (Wenn man Gmail hat und die Deutsche Sprache verwendet)
  * [Google Mail]/Trash (Bei Google Mail undEnglisch)
  * [Google Mail]/Papierkorb (Bei Deutsch)

Was man hat kann man der Orderstruktur entnehmen.

Nach dem OK Klicken und dem neustart von Thunderbird kann man im Gmail (online) das Label [Imap/Trash] löschen.

Nun sollte alles so sein wie man es will.


 [1]: http://mail.google.com/support/bin/answer.py?answer=77662&topic=12761