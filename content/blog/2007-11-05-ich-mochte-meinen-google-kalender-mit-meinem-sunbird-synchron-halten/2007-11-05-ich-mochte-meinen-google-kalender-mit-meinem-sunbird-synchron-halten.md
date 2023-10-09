---
title: Ich möchte meinen Google Kalender mit meinem Sunbird synchron halten
author: Sebastian
date: 2007-11-05T19:19:14+00:00
categories:
  - Anletungen und Tipps
tags:
  - Google
  - Kalender
  - Sunbird
  - Synchron
  - Thunderbird
import: sebastian.thiele.me
---

**Was ist mein Ziel?**

Ich habe einen Kalender bei [google][1] und möchte den nun in meinem [Sunbird][2] nutzen. Ich möchte das meine Daten sowohl in Google als auch lokal aktuell sind.

**Wie gehe ich vor?**

Als erstes sei gesagt man braucht eine permanente Internet verbindung um eine wirkliche Synchronität zu gewährleisten.

Was brauche ich noch?

Für Sunbird wird ein AddOn benötigt das &#8220;[Provider for Google Calendar][3]&#8221; (zur Zeit des schreibends aktuelle Version 0.3.1) AddOn. Das muss installiert werden. (Wie das geht hoffe ich weiß jeder)

Wie geht es weiter?

Man loggt sich in seinen Google Kalender ein und geht auf _[Einstellungen]_ -> _[Kalender]_ -> _[\*name des Kalenders der Synchronisiert werden soll\*]_

Ganz unten findet man &#8220;Privatadresse&#8221; und dort kopiert man sich die Adresse des XML Formates.

Anschließend geht man wieder in Sunbird (das nun einmal neugestartet werden musste) und fügt einen neuen Kalender hinzu. Dort wählt man _[Im Netzwerk]_ -> _[Google Calender]_ -> und dann gibt man die Adresse in das Feld ein und folgt den Anweisungen bis zum ende.

Das wars dann auch schon eventuell noch einmal durch nen rechtsklick aktualisieren und da sind die Daten.

**Nachteil**: Man kann nicht mehr so schön Leute einladen.

[1]: https://www.google.com/calendar
[2]: http://www.sunbird-kalender.de/
[3]: https://addons.mozilla.org/en-US/thunderbird/addon/4631
