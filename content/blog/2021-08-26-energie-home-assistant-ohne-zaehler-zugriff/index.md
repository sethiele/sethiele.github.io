+++
title = "Home Assitant: Energie Messen ohne Zugriff auf den Stromzähler"
description = "Seit Version 2021.08 gibt es im Home Assistant das Engergy Dashboard. Wie man an Verbrauchswerte ohne, Zugriff auf den Zähler bekommt, beschreibe ich hier."
date = 2021-08-26T07:00:00+02:00
author = "Sebastian Thiele"
slug = "home-assistant-energy-messung-ohne-zahler"
categories = ["Maker"]
tags = ["diy", "technik", "energy", "iot", "heimautomatisierung", "home assistant", "zigbee", "zigbee2mqtt"]
toc = true
draft = false
+++

Mit dem Update vom Home Assistant auf die [Version 2018.08](https://www.home-assistant.io/blog/2021/08/04/release-20218/) wurde das [Energy Dashboard](https://my.home-assistant.io/redirect/energy/) hinzugefügt. Ein zentraler Ort, der eine Übersicht über den eigenen Energieverbrauch liefern soll. Und der Energieverbrauch ist eine sehr spannende Sache im Umfeld der Hausautomatisierung. Nicht zu letzt weil man doch auch mal schauen sollte, wieviel Strom die eigene Automatisierung eigentlich verbraucht. Gleichzeitig fallen aber auch sehr schnell Verbraucher auf, die unnötig laufen.

Es gibt einige Wege, wie man den eigenen Energieverbrauch sehr zuverlässig direkt am Stromzähler ablesen kann. Der einfachste Weg ist über einen "smart Meter" die seit 2017 bei Neubauten verbaut werden. Dort kann man über die SP1 Schnittstelle (eine kleine Infrarot LED) den verbrauch "mitzählen". Damit hat man einen sehr genauen Weg den Verbrauch zu messen. Wenn man aber wie ich in einem Mehrfamilienhaus wohnt, in dem der Zähler nicht in der eigenen Wohung ist, wird es schwer ausreichend Werte zu sammeln. Selbst ein [LORAWAN](https://de.wikipedia.org/wiki/Long_Range_Wide_Area_Network) wäre bei mir schwer bis gar nicht möglich. Also musste ich mir anderes helfen.

## Die Hardware für den Home Assistant

Wie ich bereits geschrieben habe, habe ich nicht die Möglichkeit direkt einen Sensor an meinen Stromzähler zu setzen und dadurch den gesamten Verbrauch zu erfassen. Ich behelfe mich damit, dass ich mit Steckdosen besorgt habe, die neben dem AN/AUS auch den Verbrauch an den Home Assistant sendet.

### TP-Link HS110

Schon vor längerer Zeit habe ich einige der {{< werbelink link="<https://amzn.to/3kldhlV>" text="TP-Link HS100" >}} und {{< werbelink link="<https://amzn.to/2XUhs0A>" text="TP-Link HS110" >}} gekauft. Dies sind WIFI-Steckdosen die zunächst über die Kasa-App des Anbieters mit deren Cloud verbunden werden müssen. Die HS110 sind die Modelle, die auch den Stromverbrauch messen und speichern können. Dabei gibt es einen Täglichen- und einen Gesamtverbrauch in KWh Sensor pro Gerät. Aber auch der aktuelle Verbrauch in W werden angegeben. In Zukunft werde ich mich aber von diesen Geräten trennen.

#### Vorteile der TP-Link HS110

* Die Einbindung (nach der Verbindung mit der Anbieter Cloud) ist sehr einfach.
* Kompatibel mit Alexa

#### Nachteile der TP-Link HS110

* TP-Link hat mit einem Firmware Update die Unterstützung lokaler Verbindungen deaktiviert. [Siehe hier](https://community.tp-link.com/en/smart-home/forum/topic/239364)
* Die Geräte sind im Vergleich mit ca. 30 Euro recht teuer.
* Sie sind zwischenzeitlich sehr schwer zu bekommen, weil sie schnell vergriffen sind.

### Xiaomi ZNCZ04LM (Zigbee)

Ich bin großer Fan von Xiaomi also wollte ich auch diese Steckdosen mal ausprobieren. Die {{< werbelink text="Xiaomi ZNCZ04LM" link="<https://amzn.to/2UOKbTq>" >}} Steckdosen sind [zigbee2mqtt kompatibel](https://www.zigbee2mqtt.io/devices/ZNCZ04LM.html) und daher sehr einfach einzubinden. Sie bieten Werte für den aktuellen Verbrauch in W als auch den gesamten Verbrauch in KWh.

#### Vorteile der Xiaomi ZNCZ04LM

* Sie sind einfach einzubinden.
* Sie scheinen sehr stabil Daten zu liefern.
* Sie sind ohne zwangsweise verbindung mit einer Cloud nutzbar.
* Sie sind Zigbee Repeater.

#### Nachteile der Xiaomi ZNCZ04LM

* Wenn man sie nicht bei Aliexpress und Co bestellt sind sie mit 30 Euro nicht gerade günstig.
* Auch wenn sie einfach einzubinden sind, ist Zigbee doch eine weitere Integration die man einrichten muss.

### Shelly Plug und Shelly Plug S

Beide Versionen der Shelly Stecker sind WIFI-Steckdosen. Sie liefern wie die anderen den Verbrauch in KWh als auch die aktuellen W. Der Unterschied ist, dass die {{< werbelink text="Shelly Plug S" link="<https://amzn.to/48PueNJ>" >}} "nur" 2500W (10A) vertragen und die {{< werbelink text="Shelly Plug" link="<https://amzn.to/3y8gudu>" >}} 3500W (16A) vertragen.

#### Vorteile der Shelly Plugs

* Preislich (immer mal schauen) sind die Steckdosen aktuell relativ günstig.
* Sie sind ohne zwangsweise verbindung mit einer Cloud nutzbar.
* Sie bieten weitere Sensoren die vor Überhitzung und Überlastung warnen

#### Nachteile der Shelly Plugs

* Die Stecker sind recht groß. Also nicht unbedingt geeignet um sie hinter Schränken verschwinden zu lassen.

## Die Energie richtig summieren

Hier begannen für mich eigentlich die größeren Herausforderungen. Am Anfang hatte ich jeden eigenen Sensorwert für die kWh einzeln als Messgerät unter `Grid consumption` eingerichtet. Für den ersten Moment sah das nach einer schnellen Lösung aus die gut funktionierte. Aber schnell stellte sich heraus, dass es da ein Paar Probleme gab.

{{< figure src="negative-energie.png" caption="Eigenartige Anzeigen durch falschen Verbraucher" >}}

Bei der Art der Datenerhebung passiert, es, dass Sensoren, die ihre Werte zurücksetzen (bei den HS110 Steckdosen am ende des Tages, bzw. wenn sie mal keinen Strom mehr haben) einen negativen Balken beim Verbrauch anzeigen. Oder es gab einen mir nicht ganz erklärlichen Schluckauf und auf einmal wurden riesige Summen an Verbrauch addiert, die so aber nicht einzeln nachvollziehbar waren.

Auf die Lösung brachte mich (wieder) [Felix](https://wirres.net/). Um einen (nahezu) genauen Wert liefern zu können müssen die einzelnen, aktuellen Verbrauchswerte in Watt addiert. Anschließend kann man die [Integration - Riemann sum integral](https://www.home-assistant.io/integrations/integration/) integration nutzen um das in kWh umrechnen zu lassen. Bei mir sieht das so aus:

```yaml
sensor:
  - platform: template
    sensors:
      power_total_consumption:
        friendly_name: Total Power Usage
        unit_of_measurement: W
        device_class: power
        value_template: >-
          {{
            states('sensor.1_power') | float +
            states('sensor.2_power') | float +
            states('sensor.3_power') | float +
            states('sensor.4_power') | float +
            states('sensor.5_power') | float + 
            states('sensor.6_power') | float +
            states('sensor.7_power') | float
          }}

  - platform: integration
    source: sensor.power_total_consumption
    name: Total Energy Spend
    unit_prefix: k
    round: 2
```

Im Anschluss daran sollte man sich (mind.) einen [Utility Meter](https://www.home-assistant.io/integrations/utility_meter/) anlegen, der dann die Verbrauchswerte je nach Periode summiert und dem Energy Dashboard zur Verfügung stellt.

Ich habe mir (erst einmal) zwei angelegt. Einen täglichen, um den täglichen Verbrauch zu vergleichen) und einen monatlichen um auch über längere Zeit hinweg einen Vergleich zu haben. Zum Jahreswechsel, wenn ich dran denke, werde ich mir wohl auch noch einen jährlichen anlegen.

```yaml
energy_total_daily_energy:
  source: sensor.total_energy_spend
  cycle: daily
  name: Total Daily Energy
energy_total_monthly_energy:
  source: sensor.total_energy_spend
  cycle: monthly
  name: Total Monthly Energy 
energy_total_yearly_energy:
  source: sensor.total_energy_spend
  cycle: yearly
  name: Total Yearly Energy 
```

## Das Ergebnis

Das Ergebnis lässt sich nach der Umstellung das Ergebnis sehr stabil nachvollziehen.

{{< figure src="enegy-dashboard.png" caption="Übersicht über den Stundengenauen Verbauch." >}}

Natürlich würde ich mir wünschen, direkt den Wert an meinem Stromzähler ablesen zu können, aber leider ist das Baulich nicht möglich. Aber ich gewinne den Steckdosen tatsächlich etwas gutes ab. Beispielsweise habe ich festgestellt, dass mein Schreibtisch, mit allen Anbauten erstaunlich "viel" Strom verbraucht. Grund sind einige Geräte im Stand By-Modus. Nun kann ich per automatisierung (wenn ich Feierabend habe, oder nicht am Schreibtisch sitze) meinen Schreibtisch komplett Stromlos machen.

Generell ist das Thema Verbrauch sehr spannend. In einem der nächsten Beiträge werde ich mal meine Automatisierung vorstellen die aus meiner "dummen" Waschmaschine eine smarte Waschmaschine macht und mich informiert wenn sie fertig ist.
