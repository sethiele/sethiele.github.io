+++
title = "DSL Speedtest mit dem Raspberry Pi"
description = "Es ist immer ärgerlich, wenn das Internet langsam wird. Mit einem Raspberry Pi und einem automatisierten Speedtest kann man das auch nachweisen."
date = 2016-02-26T00:00:00+01:00
author = "Sebastian Thiele"
aliases = "/dsl-speedtest-mit-dem-raspberry-pi"
categories = ["Maker"]
tags = ["raspberry pi", "anleitung", "diy", "howto", "programmieren", "technik", "iot", "heimautomatisierung"]
draft = false
+++
{{< import_note >}}

**[Update Januar 2020]: Es scheint, dass IFTTT den gezeigten Weg nicht mehr anbietet. In werde diesen Artikel überarbeiten müssen. Grundsätzlich funktioniert alles wie beschrieben, nur das Speichern in Google Sheets scheint so nicht mehr zu funktionieren.**

Vor ein paar Tagen habe ich von einem Kollegen seinen abgelegten Raspberry Pi (Generation 1) geschenkt bekommen. Nach dem ich schon viele Dinge gesehen habe die man mit solch einem kleinen Gerät machen kann, war das erste was ich (als Spielerei) umgesetzt habe eine Möglichkeit wie man seinen eigenen DSL speed mit Hilfe eines Raspberry Pi überwachen/testen kann. Das ist recht einfach und führt zu interessanten Ergebnissen, die man sicher auch mal gebrauchen kann, wenn es mal wieder darum geht ob ich auch das bekomme für das ich zahle.

Die Idee stammt nicht von mir, ich habe sie auf dem [makezine Blog](http://makezine.com/projects/send-ticket-isp-when-your-internet-drops/) (en) gefunden und an einigen Stellen etwas angepasst. Ziel des ganzen soll es sein, die (reale) Geschwindigkeit der eigenen Internetleitung zu messen und mittels [IF THIS THAN THAT (IFTTT)](http://ifttt.com/) visuell aufzubereiten. Was dazu benötigt wird, ist ein Raspberry Pi, ein wenig Ahnung wie man mit der Linux Konsole umgeht (Editoren verwenden, Grundlagen cron), einen IFTTT Account (kostenlos) und wenn ihr es wie ich in einer Google Doc-Tabelle speichern wollt, einen Google Account – das ist aber nicht zwingend notwenig dank IFTTT kann man mit den Daten auch viele andere tolle Dinge machen. Ich habe versucht die Anleitung für jeden verständlich zu machen. Wenn es mir an der einen oder anderen Stelle nicht gelungen ist, bitte ich um einen Hinweis, dann versuche ich das nachzubessern.

## Der Raspberry Pi für den Speedtest vorbereiten

Zunächst ein mal muss der Raspberry Pi dazu gebracht werden einen [bekannten Dienst](http://www.speedtest.net/) anzufragen, wie schnell die Daten durch die eigene Leitung kommen. Dazu hat jemand eine [Anwendung geschrieben](https://github.com/sivel/speedtest-cli), die genau das auf der Kommandozeile macht. Das muss erst ein mal installiert werden. Dazu loggt man sich auf der Konsole des Pis an.

Zuerst muss das System vorbereitet werden, damit es das Programm ausführen kann. Dazu muss sichergestellt werden, dass alle nötigen Komponenten installiert sind. *(Das $-Zeichen bitte nicht mit kopieren, es soll nur zeigen, das es ein Bash-Befehl ist.)*

```
$ sudo apt-get update && sudo apt-get install -y ruby python-pip
```

Wenn python-pip oder ruby bereits installiert ist, um so besser. Im Anschluss kann das eigentliche Speedtest Tool installiert werden.

```
$ sudo pip install speedtest-cli
```

Wenn das erfolgreich durchgelaufen ist, kann man es direkt mal testen. Der Aufruf in der Konsole sieht dann so aus:

```bash {linenos=false}
$ speedtest-cli
Retrieving speedtest.net configuration...
Retrieving speedtest.net server list...
Testing from INTERNETANBIETER (XXX.XXX.XXX.XXX)...
Selecting best server based on latency...
Hosted by Highspeed-Check.de (Berlin) [17.13 km]: 32.665 ms
Testing download speed........................................
Download: 41.78 Mbit/s
Testing upload speed..................................................
Upload: 8.02 Mbit/s
```

Jetzt könnt ihr auf der Konsole sehen, was euer Internet gerade her gibt. Das reicht aber noch nicht, wir wollen die Daten auch verarbeiten.

## Die Daten vom Speedtest weiter verarbeiten

**Achtung, mir wurde von mehreren Lesenden mitgeteilt, dass der Weg über IFTTT nicht mehr funktioniert. Ich werde in Zukunft hier ein Update einschieben.**

Wie ich am Anfang bereits geschrieben habe, möchte ich, dass meine Daten von dem Speedtest weiter verarbeitet werden. Dazu nehme ich den Dienst IFTTT und lass mir die Daten in eine Google Tabelle schreiben.

Zunächst müsst ihr bei IFTTT den [Channel Maker - neuer Name Webhooks](https://ifttt.com/maker_webhooks) aktivieren. Ein generell sehr nützlicher Channel, wenn ihr irgend welche Dinge aus Anwendungen holen wollt die mit dem Internet verbunden sind. Wenn ihr den Channel aktiviert habt, könnt ihr unter "**Your key is:**"" euren persönlichen Key (secret key) ablesen. den benötigt ihr gleich.

Nachdem der Channel aktiviert und eingerichtet ist, geht es auf der Konsole weiter. Ihr benötigt noch eine weitere Komponente, die euch die lange Ausgabe des Speedtest so umwandelt, dass sie weiter verarbeitet werden kann. Das Tool findet ihr im folgenden als [gist](https://gist.github.com/sethiele/e19ca737dbd96887f5a1).

Keine Angst davor. Alles was ihr machen müsst, ist euch auf der [gist-Seite](https://gist.github.com/sethiele/e19ca737dbd96887f5a1) die Datei herunter zu laden. Entweder ihr ladet euch die ZIP-Datei herunter und entpackt diese auf eurem Raspberry Pi, oder ihr kopiert euch den Inhalt heraus und erstellt auf eurem Pi eine Datei die `speedtest-cli-ext.rb` heißt und kopiert dort den gesamten Inhalt rein. Im Weiteren gehe ich davon aus, dass ihr die Datei unter `/home/pi/speedtest-cli-ext.rb` abgelegt habt.

Ihr müsst noch die Zugriffsrechte der Datei so anpassen, dass sie auch ausführbar ist.

```
$ chmod u+x /home/pi/speedtest-cli-ext.rb
```

Bevor es daran geht das Script laufen zu lassen müsst ihr noch euren persönlichen IFTTT – Maker – Secret key in die Datei eintragen. Dazu öffnet die Datei in einem euer Editoren, und ersetzt die „XXXXXXXXXXX“ in Zeile 7 mit euren Key, den ich weiter oben beschrieben habe.

## Speedtest Daten visuell darstellen und automatisch erstellen

Bis jetzt haben wir nur eingerichtet, dass der Raspberry Pi, einen Speedtest auf der Konsole durchführen kann, dass diese Daten umgewandelt werden, damit sie verarbeitet werden können, und dass sie (hoffentlich) an IFTTT gesendet werden. Was noch fehlt ist, dass die Daten bei IFTT auch verarbeitet werden können und das der Test auch automatisch verarbeitet werden können.

Fangen wir damit an, dass IFTTT auch etwas vernünftiges damit anfangen kann.
Dazu legen wir uns ein Rezept an das den Input des Maker Channels erfasst und an Google Tabellen sendet. [Das Rezept habe ich bei IFTTT bereitgestellt](https://ifttt.com/recipes/389916-raspberry-pi-speedtest-to-google-table).\
*Anmerkung zu den Werten 50 und 10 – das sind meine Soll-Geschwindigkeit, die ich für die Darstellung in dem Diagramm nutze. Kann gerne gelöscht oder angepasst werden.*

{{< figure src="ifttt-speedtest-ausgabe-google-tabellen.png" caption="Nach etwas Formatierung kann es dann in Google Tabellen so aussehen" caption-position="bottom" >}}

Jetzt noch etwas Magie mit Diagrammen, und dann kann es auch optisch etwas ansprechender aussehen.

{{< figure src="DSL-speedtest_header.png" caption="Wenn man etwas mit Diagrammen rum spielt kann es dann so aussehen." caption-position="bottom" >}}

Nachdem wir IFTTT dazu gebracht haben, unsere erfassten Daten zu verarbeiten, sollten wir nun auch dafür sorgen, dass die Daten erfasst werden.

Zunächst testen wir die Verarbeitung in der Konsole per Hand. Dazu ruft ihr die Datei `/home/pi/speedtest-cli-ext.rb` in eurer Konsole per Hand auf.

```
$ /home/pi/speedtest-cli-ext.rb
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   101  100    49  100    52     77     81 --:--:-- --:--:-- --:--:--    81
```

Keine Angst wenn da erst ein mal nichts zu sehen ist, das Tool Arbeitet im Hintergrund. Die Ausgabe die darunter kommt, ist erst ein mal uninteressant. Wichtiger ist, was eure Google Tabelle sagt. Da sollte jetzt ein Eintrag zu finden sein, wie in dem Beispiel weiter oben zu sehen ist.

Dazu benutzen wir einen cron job. Die Bearbeitung öffnet man mit

```
$ crontab -e
```

In dem Fenster müsst ihr folgende Zeile (ganz unten) eintragen:

```
0 * * * * /home/pi/speedtest-cli-ext.rb
```

Nachdem ihr die Datei beendet habt, sollte der Raspberry Pi den Speedtest zu jeder vollen Stunde durchführen und das Ergebnis an IFTTT senden, das es wiederum in eure Google Tabelle schreibt.

### Ausblick – mehr Spaß mit dem Speedtest

Das hier ist eigentlich nur eine Basisausstattung für den Speedtest. Mit den Grundlagen, die hier beschrieben sind, könnt ihr dann noch viel mehr machen. Beispielsweise, lass ich die Grafik auf einer Webseite ausgeben, auf der in Zukunft auch noch weitere Projekte kommen sollen.

Oder man kann auch seinen Provider automatisiert darüber informieren, wenn das Internet mal wieder schlecht ist. Wie es beispielsweise dieser Twitter Account macht. Oder ihr schreibt eurem Provider direkt eine Mail – da freuen die sich bestimmt.

Alles was ihr machen müsst ist, entweder die Datei `speedtest-cli-ext.rb` anpassen (sie ist in ruby geschrieben), oder ihr könnt IFTTT dazu bringen andere coole Dinge zu machen. Grenzen sind dabei keine gesetzt.

Wenn ihr was cooles damit macht, dann würde ich mich über einen Hinweis dazu freuen. Ich bin immer auf der Suche nach neuen, tollen Dingen.