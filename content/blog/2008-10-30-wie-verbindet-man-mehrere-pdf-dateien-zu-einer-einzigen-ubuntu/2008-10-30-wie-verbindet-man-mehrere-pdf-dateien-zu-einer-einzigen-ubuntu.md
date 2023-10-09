+++
title = "Wie verbindet man mehrere PDF-Dateien zu einer einzigen (ubuntu)"
date = 2008-10-30T15:18:08+00:00
categories = ["Linux/Unix"]
tags = ["Linux/Unix", "office", "PDF"]
import = "sebastian.thiele.me"
+++

Bei mir auf Arbeit ist es des öfteren von Nöten aus mehreren PDF-Dateien eine zu machen. Dies gelingt unter Linux sehr einfach mit dem Programm pdftk. Das Programm kann über die Kommandozeile einfach installiert werden:

<pre lang="lang">sudo aptitude install pdftk</pre>

Das Programm pdftk kann folgendes:

  * Zusammenfügen und Teilen von PDF-Dokumenten
  * PDF-Dokumente mit Wasserzeichen versehen
  * Dateien als Anlage an ein PDF-Dokument anfügen
  * Abfragen und Aktualisieren der Meta-Informationen einer PDF-Datei
  * Ausfüllen von PDF-Formularen
  * Ausführliche Infos gibt es auf dieser <a href="http://www.lagotzki.de/pdftk/" target="_blank">Seite</a>

Heute aber erst einmal nur das zusammenfügen von PDF-Dateien.

In der Kommandozeile wechselt man in den Ordner in dem die PDFs liegen und gibt folgendes ein:

<pre lang="bash">pdftk doc1.pdf doc2.pdf cat output ziel.pdf</pre>

doc1.pdf und doc2.pdf sind natürlich die Quelldokumente (mit beliebiger Anzahl erweiterbar) ziel.pdf ist dann die Datei, die am Ende heraus kommt.

Tipp von tec.bdd-music.de:

> Hat man die PDF-Dateien bereits durchgehend und in der koerrekten Reihenfolge benannt (z.B. seite01.pdf bis seite25.pdf), so kann man sie einfach mit dem Platzhalter * eingeben und pdftk wird sie in der entsprechenden Reihenfolge aneinanderfügen. Beispiel:
> 
> <pre lang="bash">pdftk seite* cat output document.pdf</pre>

Quellen:
  
<a href="http://wiki.ubuntuusers.de/PDF?highlight=pdftk#pdftk" target="_blank">ubuntuusers Wikipedia</a>
  
<a href="http://tec.bdd-music.de/?p=68" target="_blank">tec.bdd-music.de</a>

**Nachtrag und auch viel besser!**

Dank Sashas Kommentar, habe ich gesehen das es eine noch viel bessere Methode gibt. Dank Imagemagick (sudo aptitude install imagemagick) kann man die PDF Dateien einfach mit

<pre lang="bash">convert doc1.pdf doc2.pdf ziel.pdf</pre>

Vielen dank an dieser Stelle