+++
title = "Die ersten Überlegungen wenn man mit Hausautomatisierung anfängt"
description = "Bevor man einfach los legt gibt es ein paar Dinge die man sich überlegen sollte bevor man wild mit der Hausautomatisierung anfängt"
date = 2023-07-07T08:49:43+02:00
author = "Sebastian Thiele"
slug = "beginnen-mit-hausautomatisierung"
categories = ["Maker"]
tags = ["diy", "Technik", "iot", "heimautomatisierung", "home assistant", "Bluetooth"]
toc = true
draft = true
+++

Das Thema Hausautomatisierung beginnt meistens sehr klein, eine Empfehlung eines Freundes, ein cooles YouTube Video, ein Besuch bei jemanden der/die es bereits eine weile betreibt und schon ist die Idee geboren "Ich will das auch". Und dank Systeme wie [Home Assistant](https://home-assistant.io) (mein bevorzugtes System) ist der Einstieg auch unglaublich einfach. Ein {{< werbelink link="https://amzn.to/46GY06f" text="Raspberry Pi" >}} mit dem bereitgestellten Image bespielen, Strom rein und (fast) fertig.

Doch aus Erfahrung weiß ich, wenn jetzt direkt losgelegt wird (was am Anfang auch einfach der beste Weg ist um schnell Erfolge zu sehen) baut man sich Strukturen und Automatisierungen auf, die man später nie wieder glatt ziehen wird. In diesem Artikel möchte ich ein paar Denkanstöße geben, die man vielleicht direkt am Anfang machen sollte um später den Überblick zu behalten und eine saubere Struktur zu haben. Aber auch um sich das Leben an einigen Stellen leichter zu machen. Und ich wünschte mir ich hätte es auch gemacht.

## Benennung von Sensoren, Aktoren, Automatisierungen und einfach allem

Eine der wichtigsten Dinge um auch nach längerer Zeit wieder zu verstehen was man da mal gemacht hat ist eine gute Namensvergabe/Namenskonvention.
Es hilft unheimlich der späteren Orientierung, wenn man sich gleich von Anfang an angewöhnt eine einheitliche Benennung zu wählen. Wie sie am Ende aussieht ist jedem selbst überlassen. Auch ob ihr lieber deutsche oder englische Namen gebt, ist ganz euch überlassen, aber tut euch den gefallen un seid einheitlich. (Klar Englisch hätte den Vorteil dass ihr keinen Stress mit Sonderzeichen habt, die im Englischen nicht vorkommen und darum evtl. von eurer Software nicht verwenden könnt.) Das ganze ist echt nah an üblichen Konventionen bei Programmiersprachen angesiedelt.

### Hier ein paar Vorschläge wie ihr eure Sensoren benennen könntet

| Typ | Beschreibung | Platzhalter |
| --- | ---- | --- |
| Ort ein wo der Sensor zu finden ist | Badezimmer/bath, Küche/kitchen, Garten/garden, ... | `$ORT` |
| Art des Sensors | Bewegung/motion, Temperatur/temperature, Luftfeuchtigkeit/humidity, Erschütterung/vibration, Anwesenheit/presence, ... | `$ART` |
| Deteil zum Sensor um sich abzugrenzen zu anderen | Eingangstür/main entry, Fenster links/left window, ... | `$DETAIL`|

Im Falle von der Software Home Assistant kommen durch das System bereits weitere Werte dazu.
Beispielsweise gibt es [Binäre Sensoren](https://www.home-assistant.io/integrations/binary_sensor/) die mit `binary_sensor.` vorangestellt werden. Oder auch `input_number.`, `sensor.` sind mittel um virtuelle als auch reale Sensoren zu identifizieren.

## Hausstatus - Urlaubsmodus, Abwesenheitsmodus und mehr

.

## Kein Backup, kein Mitleid

.
