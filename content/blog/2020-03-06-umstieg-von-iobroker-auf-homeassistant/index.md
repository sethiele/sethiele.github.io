+++
title = "Umstieg von ioBroker auf Home Assistant für die Hausautomatisierung"
description = "Nach einer Empfehlung eines Kollegen habe ich mir den Hoe Assistant angeschaut und es ist eine gute Alternative zu ioBroker"
date = 2020-03-06T00:00:01+01:00
author = "Sebastian Thiele"
slug = "home-assistant-iobroker-hausautomatisierung"
categories = ["Maker"]
tags = ["raspberry pi", "diy", "technik", "temperatur", "klima", "iot", "heimautomatisierung", "home assistant", "iobroker"]
draft = false
+++

Irgend wann begann ich mal damit ein paar "smarte" Geräte ins Haus zu bringen. Ich glaube das erste waren ein paar smarte Assistenten von Amazon. Danach kamen ein paar WiFi Steckdosen, dann ein paar Funktermometer und so wurde es immer mehr.

So lange man immer schön bei einem Anbieter bleibt klappt die Automatisierung beist auch recht gut über die mitgelieferten Apps. Mach Licht an, wenn der Bewegungsmelder etwas meldet, und nach zwei Minuaten wieder aus. Sobald man aber seine Geräte etwas mischt, oder auch mal eigene Sensoren und Aktore baut wird es kniffelig.

{{< figure src="home-assistant-lovelace_header.png" caption="Beispiel für die Lichtsteuerung" caption-position="bottom" >}}

## Unterschiedliche Komponenten zentral steuern

Ein Nachbar empfahl mir dann mal den [ioBroker](https://www.iobroker.net/) als Steuerzentrale auszuprobieren. Und es funktioniert super, wie mein [Beispiel der Temperatur Messung]({{< ref "2019-01-08-raumklima-ueberwachung-selbst-gemacht-ioBroker-raspberry-pi-Xiaomi-Temperatursensor-luftfeuchtigkeitssensor" >}}) zeigt. Und es machte genau das was ich wollte. Es verbindet verschiedene Systeme und bietet eine zentrale "Komandozentale".

Einige Zeit lief das System auf meinem Raspberry Pi (zweite Generation) recht gut. Bis dann Updates kamen, die es für mich nicht mehr brauchbar machte.

Vor ein paar Wochen wurde ich dann auf ein anderes System aufmerksam gemacht. Mit dem [Home Assistant](https://www.home-assistant.io/) habe ich mich dann eine weile beschäftigt.

Was mich sofot begeistert hat, war die Einfachheit. Ganz besonders der einfache Aufbau der "grafischen Oberfläche" mit Namen `lovelace`. Dort kann man einfach Steuerelemente schnell mal zusammen klicken.

## Konfiguration in Files

Als ich etwas tiefer eingestiegen bin, kam aber das für mich absolute totschlag Argument dazu. Sämtliche Konfiguration können im [yaml-Format](https://de.wikipedia.org/wiki/YAML) geschrieben und damit auch versioniert werden. Damit lässt sich sämliche Konfiguration testen und wie jeder Code weiterentwickeln. Auch lassen sich automatisierte Konfigurations-Deployments entwickeln.

Von den Unterstützen Geräten/Herstellern habe ich erst einmal keinen Unterschied gefunden. Allerdings sind einige Integrationen, wie eine direkte Unterstützung von [influxDB](/tags/grafana/) - und damit inidrekt [grafana](/tags/influxdb/), viel einfacher zu benutzen, dank einfacher Konfiguration in Files.

## iOS und Android App mit Geo-Daten lieferung

Besonder gut finde ich, dass es für Home Assistant offizielle Apps für iOS und Android gibt. Wenn man die entsprechende Einstellung/Aktivierung vorausgesetzt, kann die jeweilige App als Geo-Tracker genutzt werden. Dadurch lassen sich Geo-basierende Aktionen ausführen.

## Voraussetzung für einen stabilen Betrieb von Home Assistant

Momentan läuft der Home Assistant auf meinem Raspberry Pi 2. Allerdings ist der etwas schwach für den Umfang der Bedient wird. Empfohlen wird ein mind **Raspberry Pi 4 Model B (2GB)**, mind. 32 GB SSD (Class 2). Entsprechende Images können auf der [Webseite heruntergeladen](https://www.home-assistant.io/hassio/installation/) werden.

Von [Felix](https://wirres.net/) wurde mir aber ein Intel-Nuc empfohlen. Auf den werde ich nun vielleich mal hin sparen.

Nechdem ich nun so begeistert davon auf [Twitter berichtete](https://twitter.com/Sebat/status/1229864437718192129), [stellts sich heraus](https://twitter.com/ekeih/status/1230239687224238080), dass die [Berliner User Group von Home Assistant](https://www.meetup.com/de-DE/Berlin-Home-Assistant/) von einem Kollegen organisiert wird und sogar im Büro meiner Firma stattfindet.