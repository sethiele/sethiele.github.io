+++
title = "Home Assistant Presence Detection für Kinder - Und andere - mit Room Assistant"
description = "Presence Detection in Home Assistent mit einem smart phone ist einfach, doch was ist mit Kindern, die kein smart phone haben? Die Lösung Room Assistant"
date = 2022-03-23T07:00:00+02:00
author = "Sebastian Thiele"
slug = "home-assistant-presence-detection-kinder-room-assistant"
categories = ["Maker"]
tags = ["diy", "technik", "kind", "iot", "heimautomatisierung", "home assistant", "bluetooth", "room assistant"]
toc = true
draft = false
+++

Eine der wichtigsten Sachen in der Hausautomatisierung ist einer vernünftige Presence Detection (Erkennen ob und wer zu Hause ist oder auch nicht). In den meisten Fällen ist das verhalten der Automatisierungen unterschiedlich ob jemand zu Hause ist oder nicht. Ob nun der Staubsauger los laufen soll, die Heizung an gehen soll, die Lichter an oder aus sein sollen oder oder oder. Möglichkeiten gibt es viele, aber alle sind davon abhängig einen vernünftige Erkennung zu haben ob jemand zu Hause ist oder nicht.

Bei Erwachsenen ist das eigentlich recht einfach möglich. Fast jeder von uns hat mittlerweile ein Smart Phone. Und ob man nun die App ([iOS](https://apps.apple.com/de/app/home-assistant/id1099568401), [Android](https://play.google.com/store/apps/details?id=io.homeassistant.companion.android)) verwendet oder über die Präsenz im Wifi prüft pb jemand zu hause ist oder nicht ist dabei die einfachste Lösung. (Ich vertraue da auf die App für Familienmitglieder und WiFi-Erkennung bei Gästen.)

## Denk doch mal einer an die Kinder - die noch kein Smart Phone haben

Es soll ja Menschen geben - wie mich - die ihren Kindern noch kein smart phone an die Hand geben. Also zumindest keins, dass sie auch außerhalb des eigenen WiFis nutzen können und darum auch nicht mit raus nehmen. Das hatte bei uns immer zur Folge, dass das Kind immer als "zu Hause/`at_home`" angezeigt wurde.

Also überlegte ich mir in erster Linie, was hat das Kind in den meisten Fällen dabei, an dem man die Präsenz erkennen kann oder auch nicht.
Die erste Überlegung war die Mappe. Die wird aber nur in die Schule mitgenommen.
Zweite Überlegung war die Jacke/Schuhe. Das war mir aber zu aufwendig und außerdem wird Kleidung auch mal gewechselt.
Kam ich also zum Schlüssel. Okay mit acht Jahren nimmt er den auch noch nicht überall mit hin, aber besser wird es nicht.

Da viel mir ein, dass ich vor einigen Monaten die {{< werbelink text="Tile Token" link="https://amzn.to/3D3V9pN" >}} durch {{< werbelink text="Apple Air Tags" link="https://amzn.to/3JzWRSj" >}} ersetzt habe. Einfach weil die Funktionalität für den Zweck, verloren gegangene Dinge zu finden, bei den Air Tags viel besser ist als bei den Tile Anhängern.

Allerdings haben die Tile Geräte einen erheblichen Vorteil gegenüber den Apple Air Tags... auch wenn beide regelmäßig ein Bluetooth Signal absenden, ändern die Air Tags ihre Kennung (sehr vereinfacht gesagt) jedes mal um ein unerwünschtes Tracking zu verhindern. Die Tile Geräte aber nicht. Ihre Kennung bleibt gleich.

## Presence Detection per Bluetooth und Room Assistant auf einem Raspberry Pi Zero W

{{< figure src="presence-detection-room-assistant_header.png" caption="Akkurate Presence Detection." >}}

Nachdem die Frage "was ist zu Hause oder nicht?" geklärt war ging es noch darum herauszufinden, wie man erkennt ob etwas zu Hause ist oder nicht. Dank Bluetooth ist das alles kein Problem. Bei meiner Suche bin ich auf das Projekt [Room Assistant](https://www.room-assistant.io/) aufmerksam geworden. Eigentlich ist es dafür da, eine Erkennung der Anwesenheit auf Raum Ebene zu ermöglichen, aber wenn es erkennen kann in welchem Raum etwas ist, weiß es auch ob etwas da ist oder nicht. Zumal in das mit der Erkennung des Raumes auch noch auf meiner Liste habe.

Die Installation (auf einem {{< werbelink text="Raspberry Pi Zero W" link="https://amzn.to/3qv4TnU" >}}) war dank der sehr guten [Anleitung](https://www.room-assistant.io/guide/quickstart-pi-zero-w.html#requirements) wirklich schnell und einfach gemacht. (Anmerkung mittlerweile laufen drei von den Geräten bei mir und ich habe alle drei in meine Automatisierung per [Ansible](https://de.wikipedia.org/wiki/Ansible) aufgenommen, und lasse damit alle drei automatisiert konfigurieren.)

Die Konfiguration des Room Assistant ist ebenfalls sehr einfach. In der einfachsten Anwendung sieht sie bei mir wie folgt aus ([hier geht es zum gist](https://gist.github.com/sethiele/250b1e09005ec8e82fb45f23635be03b)):

```yaml
global:()
  instanceName: RAUMNAME
  integrations:
    - homeAssistant
    - bluetoothLowEnergy

homeAssistant:
  mqttUrl: 'mqtt://mqtt_broker_ip:mqtt_broker_port'
  mqttOptions:
    username: mqtt_broker_user
    password: mqtt_broker_password

bluetoothLowEnergy:
  allowlist:
    - ABCDEFGHIJ
  tagOverrides:
    ABCDEFGHIJ:
      name: EIN NAME
```

Und jetzt zum schwersten Teil. Ich musste ihrend wie die Kennung (Mac-Adresse) des Tile-Gerätes herausfinden. Das gestaltete sich ziemlich schwer. In der Gegend in der ich Wohne schwirren ein haufen Bluetooth-Geräte herum. Und dank [Corona Warn App (CWA)](https://www.coronawarn.app/de/) sind die Bluetooth-Erkennungen sehr flüchtig. Der einzige Weg den ich gefunden habe war der, dass ich am Tile-Gerät die Batterie raus genommen habe, geschaut habe welche Geräte beim start von Room Assistant erkennt wurde, dann die Batterie eingelegt habe, geschaut habe welche dazu kamen, dann die Batterie wieder raus genommen habe, geschaut habe welches Gerät nicht mehr erscheint und das ganze so oft wiederholt bis ich mir sicher war, das richtige Gerät gefunden zu haben. (Wenn jemand einen besseren Weg kennt, [schreibt ihn mir gerne](https://twitter.com/sebat)).

Seit dem habe ich eine recht zuverlässige Erkennung ob (sind wir ganz genau) der Schlüssel da ist oder nicht. (in Home Assistant Sprache ob er `home` oder `not_home` ist)

## Bonus: Room Assistant erkennt auch anderen Bluetooth Geräte, wie Xioami Aqara Thermometer

Die Lösung über den Room Assistant bietet gleich noch mehr Möglichkeiten. Beispielsweise habe ich damit meine alte auf selbst gebastelten MQTT versendeten Werten der Aqara Thermometer ersetzt. Die alte Lösung musste immer in einem gewissen Zeitintervall lauschen ob Daten ankommen... die neue hört einfach mit ob die Thermometer (beim ändern der Temperatur, Luftfeuchtigkeit oder Ladestand) Daten senden und leitet sie dann weiter.