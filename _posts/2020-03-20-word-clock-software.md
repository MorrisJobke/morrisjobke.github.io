---
layout: post
title:  "Word Clock - Software"
date:   2020-03-20 19:19:00 CET
categories: [howto]
---

Dies ist die Fortsetzung der Anleitung für die Word Clock. Den Hardwareteil habe ich [hier](/2020/03/19/word-clock-hardware/) beschrieben.

In diesem Beitrag möchte ich auf die Software-Komponente eingehen. Der komplette Quellcode lässt sich auf [Github](https://github.com/morrisJobke/clock) finden und ist unter MIT lizensiert.

## Features

Ich habe einige Features im Kopf, die ich gerne in der Uhr umgesetzt haben möchte und deshalb hier beschreibe.

* Anzeige der Uhrzeit (Captain Obvious lässt grüßen)
* automatisches Stellen der Uhrzeit via [NTP](https://de.wikipedia.org/wiki/Network_Time_Protocol) (ein Protokoll für die Synchronisation der Uhrzeit) über WLAN
* Zugriff auf das Erscheinungsbild der Uhr über eine API (z.B. Umstellen des Farbmusters, Helligkeit, ...) - damit die Uhr auf äußere Einflüsse reagieren kann und sich in die Wohnung integriert
* den aktuellen CO2 Wert meiner Netatmo Wetterstation als Farbe auf der Uhr darstellen (grün -> gelb -> rot)
* die Helligkeit automatisch einstellen - hier benutze ich einen Helligkeitssensor, der in einem anderen Gerät verbaut ist. Das Gerät ist eine [AWTRIX](https://awtrixdocs.blueforcer.de/#/de-de/hardware?id=lichtsensor-zur-helligkeitsregelung-optional), die diesen Wert über eine MQTT-API (siehe "Bibliotheken", für die Erklärung, was das ist) übermittelt. 

## Open Source sei Dank

Da es eine große Open Source Community rund um den Mikrocontroller gibt, wollte ich so wenig wie möglich selbst bauen und auf bestehende Komponenten zurückgreifen. Folgende Bibliotheken und Werkzeuge habe ich genutzt:

### IDE

[Arduino IDE](https://www.arduino.cc/en/Main/Software) - dies ist die Entwicklungsumgebung, in der programmiert wird und anschließend das Programm auf den Mikrocontroller übertragen wird. Einsteigerfreundlich und recht einfach gehalten. Wer mehr möchte, sei auf [PlatformIO](https://platformio.org) verwiesen. Sämtliche unten genannten Bibliotheken habe ich über die Arduino IDE installiert und aktualisiert. Dazu auf "Werkzeuge" > "Bibliotheken verwalten" gehen, nach der Bibliothek suchen und auf "Installieren" klicken. Schon kann diese genutzt werden.

### Unterstützung für den ESP8266 nachrüsten

Von Haus aus kann die Arduino IDE nicht mit dem ESP8266 umgehen. Dazu müssen Informationen zum Mikrocontroller nachgeladen werden. Dies erledigt man, indem man unter "Einstellungen" > "Zusätzliche Boardverwalter-URL" `http://arduino.esp8266.com/stable/package_esp8266com_index.json` einträgt. Nun kann man unter "Werkzeuge" > "Board" > "Boardverwalter" nach "NodeMCU" suchen. Hier muss man nun das gefundene Paket "esp8266" installieren. Ist dies geschehen, kann man unter "Werkzeuge" > "Board" > "NodeMCU 1.0" auswählen und die Arduino IDE ist bereit das Programm an den Mikrocontroller zu übertragen. Der Bequemlichkeit halber würde ich noch empfehlen den "Upload Speed" unter "Werkzeuge" höher zu setzen, um bei der Übertragung nicht zu lange warten zu müssen. `921.600` hat bei mir super funktioniert.

### Bibliotheken

Für die einzelnen Funktionen habe ich auf einige Bibliotheken zurück gegriffen, die ich einmal kurz vorstellen möchte.

* [FastLED](http://fastled.io) - dies ist die wichtigste. Sie übernimmt die komfortable Steuerung der LEDs. Man kann damit angeben, was genau eingesetzt wird - unter anderen zum Beispiel der LED-Controller, die Anzahl an LEDs, die Anordnung dieser (Streifen, Matrix, ...) oder die allgemeine Helligkeit. Anschließend hat man ein definiertes Interface, um die LEDs anzusteuern und gewisse Farben, Muster, Farbverläufe in einer gewissen Taktung zu erzeugen. Unter "Datei" > "Beispiele" > "FastLED" findet man auch zahlreiche fertige Code-Stückchen, die man direkt ausprobieren kann. ColorPalette ist ein schönes Beispiel und nutze ich gerne. Hier muss man nur `LED_PIN` auf "2" ändern, wenn man die Schaltung aus meinem Hardware-Beitrag genommen hat und gibt die Anzahl angeschlossener LEDs unter `NUM_LEDS` an und schon leuchten die LEDs nach der erfolgreichen Übertragung auf den Mikrocontroller (das wird über den Pfeil nach rechts in der Titelleiste angestoßen - vorher natürlich den Mikrocontroller per USB anstecken - alles weitere findet die Arduino IDE eigentlich selbstständig).

* [EspMQTTClient](https://github.com/plapointe6/EspMQTTClient) - diese Bibliothek verbindet zwei meiner Ansprüche - das Verbinden zu einem WLAN und eine API. Die API sollte etwas leichtgewichtiges sein und deshalb setze ich hier auf [MQTT](https://de.wikipedia.org/wiki/MQTT). Dies ist ein Protokoll für das Publish-Subscribe-Pattern. Man kann Nachrichten an einen gewissen Kanal schicken und man kann Nachrichten auf einem gewissen Kanal empfangen. Dazu verbinden sich Sender und Empfänger gegen einen gemeinsamen MQTT-Server und dann können diese Nachrichten untereinander austauschen. Die Bibliothek bietet mir an WLAN Zugangsdaten und einen MQTT-Server zu hinterlegen und dann kümmert sie sich darum, dass die Verbindung bestehen bleibt. Anschließend kann ich auf Kanälen (Topics werden diese in MQTT genannt) hören, ob Nachrichten eintreffen und bei jeder eingehenden Nachricht eine Funktion ausführen, die darauf reagiert. Zum Beispiel auf einem Kanal, denn ich "clock/brightness" genannt habe, kann man einen Zahlenwert von 0 bis 255 schicken und die Uhr reagiert darauf, indem es via FastLED `FastLED.setBrightness(...);` diesen einstellt. Konkret sieht das [so aus](https://github.com/MorrisJobke/clock/blob/c442c6784fcef633e06d05363c3c581aa10995c9/clock.ino#L106-L113). Zusätzlich nutzt diese Bibliothek noch [PubSubClient](https://pubsubclient.knolleary.net), der auch über "Bibliotheken installieren" installiert werden muss.

* [NTPClient](https://github.com/arduino-libraries/NTPClient) - wie der Name schon verrät ist dies die Bibliothek, die darauf achtet, dass die im Mikrocontroller eingebaute Uhr mit der eigentlich Uhrzeit synchronisiert ist. Dazu benötigt es ausschließlich Internetzugang (das macht ja EspMQTTClient für mich) und dann war es das auch schon.

* [ArduinoJSON](https://arduinojson.org) - diese Bibliothek brauche ich, um [JSON](https://de.wikipedia.org/wiki/JavaScript_Object_Notation) - ein Datenformat - zu lesen. Das Datenformat wird von AWTRIX benutzt, um Daten in MQTT zu übermitteln. Diese Daten enthalten den Helligkeitswert, den der Sensor in der AWTRIX misst. Damit konnte ich mir sparen einen Helligkeitssensor in die Uhr einzubauen. Natürlich kann man den auch direkt einbauen und dann auslesen und spart sich den Umweg über MQTT und hat das ganze autark. Falls dies gewünscht ist, kann man den Sensor genau wie in der [AWTRIX-Anleitung](https://awtrixdocs.blueforcer.de/#/de-de/hardware?id=lichtsensor-zur-helligkeitsregelung-optional) einbauen, da ich die exakt gleiche Schaltung verwende.

### Fallstricke

Es gibt noch zwei Fallstricke, die es zu beachten gibt. Einerseits bin ich anfangs über folgenden Fehler in FastLED gestolpert:

```
error: 'boolean' has a previous declaration as 'typedef bool boolean'
 typedef bool boolean;
              ^
```

Diesen habe ich behoben, indem in die FastLED Bibliothek entsprechend angepasst habe. Die heruntergeladenen Bibliotheken finden sich normalerweise im "Dokumente/Arduino/libraries" Ordner des aktuellen Nutzers. Dort musste ich die Zeile 15 in `FastLED/platforms/esp/8266/led_sysdefs_esp8266.h` auskommentieren. Diese Zeile enthält `typedef uint8_t boolean;`. Danach war der Fehler gelöst. Das ganze habe ich [hier](https://github.com/FastLED/FastLED/issues/733#issuecomment-491634606) gefunden.

Ein zweiter Fallstrick ist, dass "EspMQTTClient" standardmäßig nur Nachrichten bis zu einer Länge von 128 Bytes empfängt. Die Nachrichten von der AWTRIX sind jedoch meistens um die 200 Bytes lang. Um das zu lösen muss der Wert von 128 auf 250 gesetzt werden. Man findet das unter `PubSubClient/src/PubSubClient.h` als `MQTT_MAX_PACKET_SIZE`. Sobald der Wert geändert ist, kommen Nachrichten bis zu einer Länge von 250 Bytes an.

## Aktuelle Limitierungen

Aktuell gibt es noch 2 Limitierungen des Programms. Die Sommerzeitumstellung ist noch nicht implementiert. Das heißt, man muss die Uhr via API umstellen. Die zweite ist, dass der AWTRIX Teil noch fest eingebaut ist. Man kann diesen aus dem Quellcode löschen, indem man nach `// TODO only include if AWTRIX is enabled` sucht und die folgenden Zeilen löscht.

## Konfiguration

Um Einstellungen vorzunehmen, gibt es eine Datei, die persönliche Einstellungen enthält und damit die Uhr an die eigenen Bedürfnisse anpasst. Hierzu einfach die Datei `config.h.dist` nach `config.h` kopieren und die entsprechenden Werte ändern. Dies sind unter anderen WLAN-Name und Passwort oder die IP des MQTT-Servers. Ebenso können die MQTT-Topic-Namen geändert werden.

## API

Die Uhr hat eine auf MQTT basierende API. Das heißt, sie verbindet sich zu einem konfigurierten MQTT-Server und lauscht dort auf gewissen Kanälen (Topics). Vom eigenen Rechner aus, kann man sich ebenfalls mit diesem Server verbinden und dann mit der Uhr interagieren. Dazu nach MQTT Client suchen - ich nutze unter macOS [MQTT Box](http://workswithweb.com/mqttbox.html), der auf allen Platformen läuft. Aber es gibt auch andere Clients. Der MQTT-Server selbst läuft bei mir hier auf einem Server zuhause - das ist die Software mosqitto, aber es gibt viele verschiedene [MQTT-Server](https://github.com/mqtt/mqtt.github.io/wiki/brokers), die alle das gleiche Protokoll sprechen.

Die genauen Namen der MQTT-Topics können über die Variablen in `config.h` angegeben werden.

### Farbschema

* `MQTT_SUBSCRIBE_COLOR_PPM_TOPIC`: hier kann eine Ganzzahl hingeschickt werden und die Uhr wird ihr Farbschema je nach Wert anpassen. Diese entsprechen den Grenzwerten für gute, mittlere, mäßige und schlechte Luftqualität und sind bei 800, 1000 und 1400. Dafür nutzt es dann entweder lila, blau, grün-gelb oder rote Farben für die Uhr. Falls irgendwann etwas hier hin geschickt wird, bleibt die Uhr auf diesem Farbschema, bis ein neuer Wert geschickt wird, die Uhr neu gestartet wird oder etwas zu `MQTT_SUBSCRIBE_COLOR_RAINBOW_TOPIC` geschickt wird.
* `MQTT_SUBSCRIBE_COLOR_RAINBOW_TOPIC`: hier kann etwas beliebiges hingeschickt werden und die Uhr wird sich auf das Regenbogen-Farbschema zurück stellen.

### Uhrmodus und Alle LEDs aktivieren

* `MQTT_SUBSCRIBE_VIEW_FULL_TOPIC`: wenn etwas hier hin geschickt wird, werden alle LEDs der Uhr angesteuert und nicht nur die, der aktuellen Uhrzeit
* `MQTT_SUBSCRIBE_VIEW_TIME_TOPIC`: wenn etwas hier hin geschickt wird, werden wieder nur die LEDs angesteuert, die die aktuelle Uhrzeit repräsentieren

### Helligkeit

* `MQTT_SUBSCRIBE_BRIGHTNESS_TOPIC`: hier kann ein Wert zwischen 0 (LEDs aus) und 255 (volle Helligkeit) geschickt werden und die Uhr setzt das um

### Zeitverschiebung

* `MQTT_SUBSCRIBE_OFFSET_TOPIC`: hier kann eine Zeitverschiebung in Sekunden geschickt werden, die die Verschiebung zur UTC-Zeit (das ist GMT in der Normalzeit) angibt. Für Deutschland müsste man im Winter 3600 und im Sommer 7200 setzen. (siehe Limitierungen oben - die Uhr kann noch nicht automatisch zwischen Sommer- und Winterzeit wechseln)

Das wäre es dann. Viel Spaß beim Ausprobieren und lasst mich wissen, falls ihr Fragen habt. :)