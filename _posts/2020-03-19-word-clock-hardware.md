---
layout: post
title:  "Word Clock - Hardware"
date:   2020-03-19 21:33:00 CET
categories: [howto]
---

Ein kleiner Spoiler am Anfang - so wird es einmal aussehen:

![Fertige Word Clock](/images/2020-03-19-word-clock.jpg)

Ich wollte schon immer eine Word Clock mein eigen nennen. Dazu dachte ich mir, dass man sich das einfach kauft und gut ist. Leider haben mich damals™ (es war Mitte 2015) die Preise im oberen dreistelligen Preisbereich abgeschreckt. Ich habe also nach Selbstbaulösungen gesucht und einige auf Instructables.com gefunden. Hauptsächlich inspiriert hat mich die Anleitung [Sleek Word Clock](https://www.instructables.com/id/Sleek-word-clock/) und ich kaufte also einige handvoll LEDs, Widerstände und ICs (z.B. Schieberegister-ICs).


## Der Rahmen und die Buchstabenblende

Als Grundgerüst diente ein schwarzer [IKEA Rahmen Ribba](https://www.ikea.com/de/de/p/ribba-rahmen-schwarz-40378401/). Dieser ist quadratisch, recht tief und bietet damit auch Platz für die Elektronik.

Als nächstes musste die Blende für die Buchstaben entstehen. Ich habe dazu viel gelesen und hatte bereits Bedenken, dass ich Stunden damit verbringen werde diese irgendwie mit einem Messer aus Pappe auszuschneiden. Dann suchte ich jedoch noch einmal nach Lasercuttern in meiner Umgebung und wurde tatsächlich fündig. Es gibt ein [Fablab in Chemnitz](https://fablabchemnitz.de), welches auch einen Lasercutter hat. Also setzte ich mich daran die Buchstaben für die Uhr zusammen zustellen. Dies habe ich einmal als [SVG](/f/Word Clock English.svg) (für den Lasercutter) und als [PDF](/f/Word Clock English A4.pdf) (zum Ausdrucken bei 100% um eine Orientierungshilfe zu haben). Im Bastelladen des Vertrauens findet man recht leicht schwarzes, dickeres Papier in A3. A3 wird hier benötigt, da der Bilderrahmen 23x23 cm groß ist und somit 2 cm zu breit für die schmale Seite von A4 ist.

![Papierblende](/images/2020-03-19-words.jpg)

Die Buchstabenblende habe ich dann von hinten mit einem weißen Blatt Papier beklebt und damit ist dieser Teil fertig.


## Die LEDs und deren Steuerung

Dann ging es an die LEDs. Ich habe also ganz viele LEDs mit Widerständen versehen und lange gelötet. Anschließend alles getestet dieser Teil funktionierte soweit. Nun ging es daran aus den ICs den Digital-zu-Analog-Wandler für die einzelnen LED-Segmente zu bauen und es war zum Haare raufen. Nichts funktionierte, wie es sollte und meine Frustgrenze war erreicht. Das Projekt landete in der Ecke und blieb dort erst einmal.

![Einzelne LEDs](/images/2020-03-19-old-way.jpg)

## Die Rettung: LED-Stripes mit WS2812B

Nachdem das Projekt Jahre in der Ecke lag, stolperte ich über ein Projekt namens [AWTRIX](https://awtrixdocs.blueforcer.de/), welches eine LED-Matrix über einen ESP8266 ansteuert. Das mag jetzt erstmal unverständlich klingen, aber lasst es mich erklären.

Mittlerweile gibt es auf diversen Online-Kaufportalen eine Vielzahl an LED-Beleuchtungsmitteln. Wenn man jetzt nach WS2812B und LED sucht, dann findet man hauptsächlich Streifen mit RGB-LEDs. Es gibt jedoch auch andere Formen wie eine Matrix (in der Tat nur ein Streifen im Zig-Zag-Muster gelegt) oder Ringe. Diese zeichnen sich alle dadurch aus, dass man sie mittels 5V betreiben und mit einem einzigen Daten-Kanal steuern kann. Das ganze kann man dann über einen Mikrocontroller steuern, da eben nur ein Daten-Kanal gebraucht wird.

Als Mikrocontroller bietet sich hier ein [ESP8266](https://de.wikipedia.org/wiki/ESP8266) an. Dieser ist recht klein und hat ein eingebautes WLAN-Modul, womit man das ganze Projekt auch etwas dynamischer einbinden kann.

Oben erwähntes AWTRIX-Projekt nutzte genau [diese Komponenten](https://awtrixdocs.blueforcer.de/#/de-de/hardware?id=basis-variante) zusammen mit einem Mikro-USB-Breakout-Board und einem Kondensator. Das reduziert die Lötarbeit auf ein Minimum und sieht dann als Schaltung so aus:

![Schaltung](/images/2020-03-19-layout.png)

## Layout der LEDs

Da die LED-Streifen leider nicht exakt so ausgelegt sind, dass sie den bisherigen Buchstabenabstand genau treffen, habe ich mich für die Variante entschieden, die 60 LEDs pro Meter besitzt. Dies war für mich ein recht passender Kompromiss. Ich habe dann versucht die LEDs so anzuordnen, dass die Trennung der Wörter nicht auf eine LED fällt und bin am Ende auf 9 bis 10 LEDs pro Reihe gekommen. Jede Reihe besitzt 13 Buchstaben, womit ich nicht jeden Buchstaben einzeln ansteuern kann, aber ich brauche ja nur Wörter einzeln ansteuern zu können.

![LED Reihen](/images/2020-03-19-led-stripes.jpg)

In dem Bild ist die dritte Reihe von unten noch unvollständig. Ich habe mich da irgendwie vertan und schlussendlich noch links eine weitere LED eingefügt.

Die LED-Streifen lassen sich leicht handhaben, da man sie zwischen jeder LED trennen kann und dann lediglich die drei Kontakte wieder verbinden muss.

![LED Reihen verbinden](/images/2020-03-19-connecting-stripes.jpg)

Es bietet sich hierbei an einige verschiedenfarbige Kabel zu haben, damit man ein einheitliches Farbschema hat, um sich Fehler bei der Verkabelung zu ersparen. Ebenso kann ich eine Abisolierzange nur empfehlen.

## Worttrennungen

Im Anschluss habe ich noch Trennwände gebastelt. Dazu habe ich Pappe in 25 mm breite Streifen geschnitten und von unten nach oben zwischen die LED-Reihen geklebt. Dabei habe ich die Worttrennungen noch auf jeder Reihe hinzugefügt. Einzelne Füllbuchstaben, die zu keinem Wort gehören, habe ich auch abgetrennt und kann diese ebenfalls einzeln ansteuern.

![Trennwände](/images/2020-03-19-separate-words.jpg)

Leider funktionierte das aufgrund der LED-Dichte für die oberste Reihe nicht. Dort musste ich das R und das C von hinten zukleben.

![Zugeklebte Buchstaben](/images/2020-03-19-hide-them.jpg)

## Ab in den Rahmen

Zum Schluss habe ich noch ein Loch in die Rückwand des Bilderrahmens geschnitten, damit ich das Anschlusskabel heraus führen konnte.

![Anschlusskabel](/images/2020-03-19-way-out.jpg)

Zwischen LED-Streifen und der Schaltung habe ich einen kleinen Steckverbinder eingebaut, damit ich beides einfach trennen kann. Dies erleichtert vor allem während des Testens vieles.

![Steckverbinder](/images/2020-03-19-connector.jpg)

Den Mikrocontroller sowie das USB-Breakout-Board habe ich auf die Rückseite des Rahmens geschraubt, da dieser nicht an der Wand hängt, sondern frei steht. Bei Bedarf kann man diese Teile jedoch auch ins Innere verlegen und nur das USB-Kabel heraus führen.

![Mikrocontroller und USB-Breakout-Board](/images/2020-03-19-electronics.jpg)

Für den Stromanschluss kann man nun ein Micro-USB-Kabel sowie ein herkömmliches USB-Netzteil verwenden. Achtet hier bitte darauf, dass das Netzteil ausreichend dimensioniert ist. Wenn alle LEDs auf voller Helligkeit leuchten könnten sie sonst das Netzteil überlasten. Die genauen Werte könnt ihr dem Datenblatt des LED-Streifens entnehmen.

Ein Blog-Post zur Software wird noch folgen. **Edit:** [hier ist er](/2020/03/20/word-clock-software/)

Viel Spaß beim Basteln :)
