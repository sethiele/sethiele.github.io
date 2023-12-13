+++
title = "Die ersten Überlegungen wenn man mit Hausautomatisierung anfängt"
description = "Bevor man einfach los legt gibt es ein paar Dinge die man sich überlegen sollte bevor man wild mit der Hausautomatisierung anfängt"
date = 2023-12-13T15:00:00+02:00
author = "Sebastian Thiele"
slug = "beginnen-mit-hausautomatisierung"
categories = ["Maker"]
tags = ["diy", "Technik", "iot", "heimautomatisierung", "home assistant", "sensoren"]
toc = true
draft = true
+++

Das Thema Hausautomatisierung beginnt meistens sehr klein, eine Empfehlung eines [Freundes](https://www.tim-schiemann.de/blog/2023/12/08/home-assistant.html), ein cooles YouTube Video, ein Besuch bei jemanden der/die es bereits eine weile betreibt und schon ist die Idee geboren "Ich will das auch". Und dank Systeme wie [Home Assistant](https://home-assistant.io) (mein bevorzugtes System) ist der Einstieg auch unglaublich einfach. Ein {{< werbelink link="https://amzn.to/46GY06f" text="Raspberry Pi" >}} mit dem bereitgestellten Image bespielen, Strom rein und (fast) fertig.

Doch aus Erfahrung weiß ich, wenn jetzt direkt losgelegt wird (was am Anfang auch einfach der beste Weg ist um schnell Erfolge zu sehen) baut man sich Strukturen und Automatisierungen auf, die man später nie wieder glatt ziehen wird. In diesem Artikel möchte ich ein paar Denkanstöße geben, die man vielleicht direkt am Anfang machen sollte um später den Überblick zu behalten und eine saubere Struktur zu haben. Aber auch um sich das Leben an einigen Stellen leichter zu machen. Und ich wünschte mir ich hätte es auch gemacht.

## Benennung von Sensoren, Aktoren, Automatisierungen und einfach allem

Eine der wichtigsten Dinge um auch nach längerer Zeit wieder zu verstehen was man da mal gemacht hat ist eine gute Namensvergabe/Namenskonvention.
Es hilft unheimlich der späteren Orientierung, wenn man sich gleich von Anfang an angewöhnt eine einheitliche Benennung zu wählen. Wie sie am Ende aussieht ist jedem selbst überlassen. Auch ob ihr lieber deutsche oder englische Namen gebt, ist ganz euch überlassen, aber tut euch den gefallen un seid einheitlich. (Klar Englisch hätte den Vorteil dass ihr keinen Stress mit Sonderzeichen habt, die im Englischen nicht vorkommen und darum evtl. von eurer Software nicht verwenden könnt.) Das ganze ist echt nah an üblichen Konventionen bei Programmiersprachen angesiedelt.

### Hier ein paar Vorschläge wie ihr eure Sensoren benennen (Entitäts-ID) könntet

| Typ | Beschreibung | Platzhalter |
| --- | ---- | --- |
| Ort ein wo der Sensor zu finden ist | Badezimmer/bath, Küche/kitchen, Garten/garden, ... | `$ORT` |
| Art des Sensors | Bewegung/motion, Temperatur/temperature, Luftfeuchtigkeit/humidity, Erschütterung/vibration, Anwesenheit/presence, ... | `$ART` |
| Detail zum Sensor um sich abzugrenzen zu anderen | Eingangstür/main entry, Fenster links/left window, ... | `$DETAIL`|

Im Falle von der Software Home Assistant kommen durch das System bereits weitere Werte dazu.
Beispielsweise gibt es [Binäre Sensoren](https://www.home-assistant.io/integrations/binary_sensor/) die mit `binary_sensor.` vorangestellt werden. Oder auch `input_number.` oder `sensor.` sind mittel um virtuelle als auch reale Sensoren zu identifizieren.

Und überlegt euch von anfang an in welcher Sprache ihr die Namen vergebt. _Ein kleiner Hinweis: im Home Assistant gibt es eine Unterscheidung zwischen Namen, der Menschenlesbar sein kann und soll und der Entitäts-ID, die zur technsichen Identifikation benutzt wird. Der Name sollte so gestaltet werden, dass ihr ihn einfach findet und versteht was es für ein Sensor ist. Die Entitäts-ID kann es nur einmal im System geben und sollte diesen Namensvorschlägen folgen._

Eine Mögliche Namensconvention könnte dann etwas wie das hier sein:

`$SENSORTYP.$ORT_$ART_$DETAIL` das könnte dann so aussehen: `binary_sensor.hall_presence_entrance` oder `sensor.childroom_humiditytemperature_temperature` und so weiter. Das gleiche gilt natürlich auch für Akteure, also das was etwas ausführen soll. Lichter, Schalter, Motoren und so weiter. Ein Beispiel für ein Licht wäre dann `light.masterbetroom_light_bed`.

## Globale Status - Urlaubsmodus, Abwesenheitsmodus und mehr

Eine Weitere Sache die ich leider viel zu spät für mich entdeckt habe, die aber so viel Arbeit spart sind globale Status.

Ich habe oft den Anwendungsfall, dass ich unterschiedliches Verhalten zu Hause erwarte, je nachdem welcher Status besteht. Beispielsweise, soll es einen Alarm geben, wenn die Tür geöffnet wird, aber niemand zu Hause ist. Oder wenn ich im Urlaub bin, möchte ich keine Benachrichtigungen über das Blumengießen bekommen. Und ganz wichtig, am Wochenende oder im Urlaub möchte ich nicht, dass der Wecker zu den üblichen Zeiten an geht.

Für alles gibt es kombinierte Sensorenzustände die man abfragen kann, aber das bei jeder Automatisierung zu machen ist aufwändig und fehleranfällig. Und widerspricht etwas dem Prinzip der Wiederverwendung. Was ich gemacht habe ist, ich habe globale Status definiert auf die ich meine Automatisierungen schauen lasse.

Die Status sind in Home Assistant [Input Boolean](https://www.home-assistant.io/integrations/input_boolean/), die ich über Automatisierungen schalten lasse, die dann auch die Kriterien beinhalten und sich zentral an einer Stelle anpassen lassen. Folgende Liste an globalen Status kann als Inspiration benutzt werden. Vielleicht werde ich in einem späteren Post mal beschreiben auf welchen Kriterien ich diese setzen. Bei Interesse [einfach melden](https://links.sethiele.de).

| Status | Beschreibung |
| --- | ---- |
| Urlaubsmodus | Ganz klar, wenn wir im Urlaub sind, sollen die Automatisierungen anders reagieren als wenn wir zu hause sind. |
| Nachtmodus | In der Nacht reagieren Lichter, die an Bewegungsmelder angeschlossen sind anders. Oder auch Heizungen und ähnliches. |
| Abwesenheitsmodus | Wenn niemand zu Hause ist, soll auch ein unterschiedliches Verhalten angewendet werden. |
| Hausputz | Wenn Hausputz angesagt ist, sollen zum Beispiel alle Lichter 100% Helligkeit geben, egal was die Zeit oder ähnliches sagt. |
| Schulmodus | Wenn Schulzeit ist, gibt es auch unterschiedliches Verhalten als zum Beispiel an Feiertagen und Ferien. |
| Werktag heute | Wenn am heutigen Tag ein Werktag ist (das kann losgelöst von Ferien sein) soll zum Beispiel der Wecker anders reagieren oder auch die Schlafenszeiten anders gesteuert werden. |
| Werktag morgen | Ich habe festgestellt, dass auch eine abfrage ob Morgen ein Werktag ist, sinnvoll ist. Zum Beispiel auch um den Wecker zu stellen oder Erinnerungen abzuspielen. |

Gebt mir gerne mal Info welche Status für euch noch von Interesse sind und welche ihr habt, die ich hier noch nicht aufgenommen habe.

## Kein Backup, kein Mitleid

Und natürlich das wichtigste sollte gleich von Anfang an geklärt sein, wie sichert ihr eure Systeme. Ich will und kann hier keine Umfangreiche Anleitung geben wie ihr eure System sichert, aber ich will unbedingt darauf hinweisen, dass ein Backup unbedingt zu empfehlen ist (nicht nur bei der Hausautomatisierung).

Für den Home Assistant gibt es dazu einen [Anleitungsartikel](https://www.home-assistant.io/common-tasks/supervised/#backups), der schon viele Fragen beantwortet.

Ich beispielsweise habe meine Konfigurationen im yaml-Format und versioniert mit Git. Damit kann ich die meisten Konfigurationen einfach wieder herstellen.

Aber eigentlich ist neben dem Backup der Restore das wichtigste. Besonders wenn ihr noch am Anfang seid empfiehlt es sich einfach mal einen restore zu versuchen um zu schauen ob auch das klappt. Es nützt das Beste Backup nichts, wenn das wiederherstellen nicht funktioniert.

Aus meiner Sicht sind das die ersten, wichtigsten Dinge die man beim Starten mit der Hausautomatisierung überlegen/machen sollte. Die Frage nach den passenden Sensoren, Automatisierungen und ähnliches ergeben sich dann später und lösen sich ziemlich einfach. Aber wenn der Grundstock schon wackelt, wird es über die Zeit immer schwerer dort wieder rauszukommen.
