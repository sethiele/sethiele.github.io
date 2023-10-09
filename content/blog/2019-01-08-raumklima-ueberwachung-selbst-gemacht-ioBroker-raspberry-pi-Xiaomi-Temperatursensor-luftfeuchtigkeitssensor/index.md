+++
title = "Raumklima-Überwachung selbst gemacht mit ioBroker, Raspberry Pi und Xiaomi Temperatur- und Luftfeuchtigkeitssensor"
description = "Ich wollte schon eine ganze weile wissen wie das Raumklima bei uns zu Hause ist. In meinem 'Zwischen den Jahren'-Projekt habe ich das umgesetzt."
aliases = [
  "/blog/2019/01/raumklima-ueberwachung-selbst-gemacht-iobroker-raspberry-pi-xiaomi-temperatursensor-luftfeuchtigkeitssensor/",
  "/blog/2019-01-08-raumklima-ueberwachung-selbst-gemacht-iobroker-raspberry-pi-xiaomi-temperatursensor-luftfeuchtigkeitssensor/"
  ]
slug = "raumklime-iobroker-raspberry-pi-xiaomi"
date = 2019-01-08T22:07:27+01:00
author = "Sebastian Thiele"
categories = ["Maker"]
tags = ["raspberry pi", "xiaomi", "diy", "technik", "anleitung", "grafana", "temperatur", "klima", "iot", "InfluxDB", "heimautomatisierung", "iobroker"]
image = "assets/2019/01/iot-klimaueberwachung.jpg"
draft = false
+++

Schon einer ganzen Weile wollte ich wissen wie es um das Raumklima bei uns zu Hause wirklich aussieht. Dem einen Teil der Familie ist es fast immer zu Kalt, dem anderen Teil zu warm. Ich erhoffte mir, mit belastbaren Werten diem ständigen hin und her vorbeugen zu können. Bereits vor einigen Monaten habe ich mir ein paar (für jedes Zimmer ein) "[Xiaomi temperature and humidity sensor](https://www.google.de/search?q=xiaomi+temperature+and+humidity+sensor)" zugelegt. Mit denen, dem Entsprechenden Gateway (in meinem Fall die "[Xiaomi Mijia Bedside Lamp (2nd Generation)](https://www.google.de/search?q=xiaomi+ble+gateway)") kann man schon sehr bequem über die Xiaomi App (iOS und Android) den Verlauf der Temperatur und der Luftfeuchtigkeit verfolgen. Allerdings reichte mir die Darstellung in der App und die minimalen Auswertungsmöglichkeiten nicht aus. Es musste etwas individuelleres her.

Dieser Post ist keine Schritt für Schritt Anleitung. Er ist eher als eine Inspirationsquelle zu verstehen. Anleitungen zum installieren der diversen Komponenten verlinke ich soweit es geht.

{{< figure src="iot-klimaueberwachung_header.jpg" caption="Klima-Überwachung Dashboard" >}}

## Daten sammeln mit dem Raspberry Pi, einem Bluetooth Dongel und ioBroker

Nachdem die Sensoren scho gesetzt waren, habe ich mich umgeschaut, womit ich die Daten aus dem Xiaomi Universum heraus bekomme und in die freie Welt und meine eigene Gewalt entlassen kann. Ein Nachbar von mir, der schon eine weile Länger in der Welt der Chinesischen IoT-Welt unterwegs war, empfahl mir den Einsatz der Software "[ioBroker](http://iobroker.net/)" um die diversen IoT-Geräte steuern zu können.

Auf die Installation von ioBroker auf einen Raspberry Pi möchte ich gar nicht weiter eingehen, dafür gibt es auf der Webseite eine sehr gute und ausführliche Anleitung. Natürlich muss es kein Raspberry Pi sein, auf dem die Software läuft, da ich aber bereits einen im Dauerbetrieb habe um meine DSL-Geschwindigkeit dauerhaft zu messen, war es naheliegend die Software darauf laufen zu lassen. Den ioBroker kann man dank diverser Module - bei ioBroker Adapter genannt - vielseitig erweitern.

Um mit den Xiaomi Sensoren komunizieren zu können benötigt man zum einen die Möglichkeit Bluetooth Signale senden und empfangen zu können. Ab dem Raspberry Pi 3 Model B ist diese Möglichkeit von Hause aus gegeben. Mein Model A bietet dies noch nicht, darum brauchte ich noch einen Dongel mit dem ich Bluetooth - genauer gesagt [Bluetooth Low Energy (BLE)](https://de.wikipedia.org/wiki/Bluetooth_Low_Energy) - nutzen kann.
Im ioBroker habe ich noch den [Adapter Bluetooth Low Energy](https://github.com/AlCalzone/ioBroker.ble) installieren und aktivieren müssen. In der entsprechenden Instanz im ioBroker wählt man noch - nach Anleitung - das Bluetoorh Device aus und aktiviert noch das Xiaomi Plugin. Nach einem neustart der Instanz (durch das Speichern) landeten auch schon alle meine Sensoren (die in Empfangsnähe sind) under den Objekten im ioBroker.

{{< figure src="ble-devices.jpg" caption="Liste der Xiaomi Sensoren im ioBroker" >}}

Wenn man die Obejte aufklappt kann man bereits die (wichtigsten) Eigenschaften, wie Temperatur, Feuchtigkeit und Batteriestand ablesen.

## Daten haltung mit InfluxDB

Zum speichern von solchen Zeit basierten Werten empfiehlt sich [InfluxDB](https://www.influxdata.com/) als Datenbank. Nicht nur, weil sie einfach zu installieren und zu verwalten ist, sondern auch weil sie sehr gut integriert in Grafana (für das spätere Anzeigen der Werte) und ioBroker ist.

Im ioBroker verwende ich den Adapter [Logging data with InfluxDB](https://github.com/ioBroker/ioBroker.influxdb) zum speichern von Werten aus den Objekten in die eingerichtete InfluxDB. Die Einstellungen der Instanz fragen nach den Zugangsdaten zur Datenbank und den Schreibaktionen - hier habe ich ersteinmal alles so gelassen wie es war.

Nach der Aktivierung und Konfiguration müssen die einzelnen Werte, die in die Datenbank gespeichert werden sollen, bei den Eigenschaften der Werte hinterlegt werden.

{{< figure src="influxdb-einstellungen.jpg" caption="Einstellungen zum speichern in InfluxDB" >}}

Meine Empfehlung ist einen aussagekräftigen Alias zu verwenden, ansonsten wird im nächsten Schritt das wiederfinden der Werte in der Datenbank aufwändig. Auf der InfluxDB Seite sollte überprüft werden, dass die Werte auch ankommen.

## Ausgabe der Werte in Grafana (Dashboard bauen)

Nachdem die Werte erhoben und gespeichert werden müssen sie nun auch noch visualisiert werden. Ich habe mich für [Grafana](https://grafana.com/) entschieden, da ich es bereits zur visualisierung anderer Werte im Einsatz habe. Grafana muss natürlich auf die InfluxDB Datenbank zugreifen können. Danach kommt nur noch fleißarbeit, die ganzen Daten auf ein Dashboard zu bringen und auf eine übersichtliche Weise darzustellen. Wenn man möchte kann man sich durch Grafana auch alarmieren lassen, wenn die Werte zu hoch oder zu niedrig werden.

Für die idealen Werte (die bei mir nur farblich hervorgehoben werden) habe ich mich im [Internet](https://www.inventer.de/wissen/luftqualitaet-gesundheit/luftfeuchtigkeit-in-wohnraeumen/) bedient.

Raum                     | Optimale Luftfeuchtigkeit | Optimale Temperatur
-------------------------|---------------------------|---------------------
Wohn- und Arbeitszimmer  | 40 - 60%                  | 20°C
Schlafzimmer             | 40 - 60%                  | 16 - 18°C
Kinderzimmer             | 40 - 60%                  | 20 - 22°C
Küche                    | 50 - 60%                  | 18°C
Badezimmer               | 50 - 70%                  | 23°C
Keller                   | 50 - 65%                  | 10 - 15°C

## Ausblick und Probleme

Nachdem nun alle Daten vorliegen und man sich die Veränderung beim lüften und heizen anschauen kann, könnte damit aber uach noch mehr gemacht werden. Denkbar ist über die automatisierung des ioBrokers weitere Steuerungen durchzuführen. Beispielweise könnte man Heizungen steuern oder Luftbefeuchter an oder aus machen lassen.

Der Nachteil ist, dass die Bluetooth Verbindung nicht sehr stark ist. In meinem Umfeld reicht es schon aus, einzelne Türen zu schließen und die Verbindung über Bluetooth Low Energy reicht nicht mehr aus um zuverlässig Daten zu empfangen. Damit entstehen leider Lücken in den Werten. Ich überlege, den Raspberry Pi an einer zentraleren Stelle aufzustellen um eine bessere Abdeckung zu gewähren.

Die Außenwerte sammel ch über einen anderen Wert ein (dazu an anderer Stelle mehr).