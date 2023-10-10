---
title: (Kopiergeschützte) DVDs unter ubuntu abspielen
author: Sebastian
date: 2008-09-16T08:17:32+00:00
categories:
  - Linux/Unix
tags:
  - Anleitung
  - DVD
  - Linux
import: sebastian.thiele.me
---
Einige male habe ich es bereits versucht doch jedesmal aus Zeit und lustmangel aufgegeben. Klar, wenn man schon am Laptop sitzt und eine DVD schauen will, und das geht nicht, hat man (ich) nicht unbedingt gleich lust dies zu fixen.

Von Hause aus ist es ubuntu (scheinbar) nicht möglich kopiergeschützte DVDs abzuspielen. Da aber fast alle Filme sozusagen Kopiergeschützt sind, wird die Auswahl der zu schauenden Filme sehr klein werden.

Hilfreich dabei ist das Paket (und die Abhängigkeiten dazu) mit den Namen **libdvdcss2**.
  
**libdvdcss2** wird nicht standart mäßig mit installiert, da es ein non-free Paket ist, welches aber jederzeit nachinstalliert werden kann. Da ich auf `www.apt-get.org` keine Quelle für **hardy** gefunden habe musste ich zu Fuß auf die suche gehen. Auf der Webseite [http://medibuntu.org](http://medibuntu.org/) gibt es das gewünschte Paket als .deb Datei die (für die Umsteiger freudig zu hören) man wie von Windows gewohnt per doppelklick installieren kann.

Die vollständige Adresse für das hardy Paket findet man hier: `http://packages.medibuntu.org/hardy/libdvdcss2.html`.

Nach dem Download des euch entsprechenden .deb Paketes kann das Paket (wie beschrieben) durch doppelklick installiert werden. Wie gewohnt werden alle Abhängigkeiten mit installiert. Alternativ dazu der wiki Eintrag von [ubuntuusers.de](http://wiki.ubuntuusers.de/Paketinstallation_DEB)

  _(Ich möchte nochmal darauf hinweisen, dass die verwendung von fremden Links ab und an keine gute Idee ist,
  und ihr hier alles auf eigenes Risiko macht.)_

Als Abspieler würde ich den Freien Player **VLC** empfehlen. Umsteiger kennen den Player evtl. bereits von Windows, denn auch da ist er verfügbar. VLC bietet kein Grafisch-hochwertiges Menü, wie evtl. diverse Player unter Windows, doch leistet dafür sehr gute und robuste Arbeit. Zu installieren ist VLC ganz einfach über das Panel &#8220;Anwendungen&#8221; -> &#8220;Hinzufügen/Entfernen&#8230;&#8221; und dort den VLC Player auswählen.
