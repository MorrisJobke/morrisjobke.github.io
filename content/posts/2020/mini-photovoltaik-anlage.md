---
categories:
- notes
date: "2020-08-25T19:37:00Z"
title: Mini PV-Anlage - Balkonkraftwerk - Steckerfertige PV-Anlage
---

**Update Juli 2022:** Andreas Schmitz hat ein [19-minütiges Video](https://www.youtube.com/watch?v=KmGLv12huHA) wie man an dieses Projekt ran geht, inklusive grober Kosten.

**Update Juni 2021:** Mittlerweile gibt es auch eine Seite der [Verbraucherzentrale zu dem Thema](https://www.verbraucherzentrale.de/wissen/energie/erneuerbare-energien/steckersolar-solarstrom-vom-balkon-direkt-in-die-steckdose-44715).

Eine kleine Notizsammlung hinsichtlich Mini PV-Anlage:

### Idee

* Mietern die Möglichkeit bieten ihre Grundlast über Solarmodule zu reduzieren
* Wikipedia: [https://de.wikipedia.org/wiki/Solarmodul#Stecker-fertige_Solarmodule](https://de.wikipedia.org/wiki/Solarmodul#Stecker-fertige_Solarmodule)

### Randbedingung

* Anlage mit maximal 600 W Leistung kann direkt vom Mieter (und nicht wie normalerweise vom Solarteur) beim Netzbetreiber angemeldet werden.
* [Anmeldung in Chemnitz bei inetz](https://www.inetz.de/startseite/erzeugungsanlagen/anschluss-und-parallelbetrieb/)
    * Punkt „Zur Anmeldung von Photovoltaikanlagen“
* [Formular zur Anmeldung](https://www.inetz.de/fileadmin/dokumente/03_Erzeugeranlagen/01_Anschluss_und_Parallelbetrieb/Anmeldung_steckerfertige_PVA_022022.pdf)
* [inetz Verlinkt auf die BDEW Anwendungshilfe](https://www.inetz.de/fileadmin/dokumente/04_Service/Installateure/Elektro/20181128_S_BDEW_AWH_Plug_in_PV_Anlagen.pdf)

### Steckverbindung

* Vorraussetzung: Dichtigkeit von mindestens IP44
* über die Steckverbindung zwischen PV-Anlage und Hausstromkreis besteht noch etwas Unklarheit, da zwei Optionen vorhanden und am Markt sind:
    * Schutzkontaktsteckdose:
        * die Außensteckdose muss nicht getauscht werden bzw ist meist vorhanden
        * erfüllt typischerweise IP44
        * freiliegende Pins - jedoch muss der normkonforme Mikrowechselrichter der Solaranlage sowieso innerhalb von Millisekunden der Netztrennung abschalten, weshalb dies oft als nicht praxisrelevantes Problem gesehen wird
    * Wieland Einspeisesteckdose (siehe [https://www.wieland-electric.com/sites/default/files/2015-04-104-gesis-rst-classic-steckdose.jpg](https://www.wieland-electric.com/sites/default/files/2015-04-104-gesis-rst-classic-steckdose.jpg))
        * Stecker sind dafür ausgelegt, dass die Pins nicht berührt werden können
        * die Außensteckdose muss durch eine Wielandsteckdose ersetzt werden

* Aus [https://www.pv-magazine.de/2019/11/21/legal-oder-illegal-stecker-solar-geraete-im-labyrinth-der-normung/](https://www.pv-magazine.de/2019/11/21/legal-oder-illegal-stecker-solar-geraete-im-labyrinth-der-normung/):

    > Ob man nicht auch die vorhandene Schukosteckdose („Typ F“) benutzen kann, wie das beispielsweise in Österreich unbestritten zulässig ist, auch darüber wird noch gestritten. Allerdings erklärte dazu kürzlich ein öffentlich bestellter und vereidigter Sachverständiger für Photovoltaik-Anlagen:
    > „Die VDE-AR-N 4105:2018-11 sieht unter Abschnitt 5.5.3 ‚spezielle Energiesteckdosen‘ (zum Beispiel nach VDE V 0628-1) vor. Falls ein Balkonmodul als steckbares Stromerzeugungsgerät mit Typ F Stecker (Schuko) die Anforderungen der EN 60335-1:2012 Abschnitt 22.5 und der DIN EN 60204-1 (VDE 0113-1):2007-06 Abschnitt 18.5 Schutz gegen Restspannung erfüllt, dann ist das hinter VDE-AR-N 4105:2018-11 Abschnitt 5.5.3 stehende Schutzziel als erfüllt zu betrachten.
    >
    > Wenn ein steckbares Stromerzeugungsgerät einen integrierten NA-Schutz nach VDE-AR-N 4105 aufweist, so schaltet es ab, sobald keine Netzspannung (mehr) anliegt. Zieht man den Netzstecker des Balkonmoduls, so liegt an dessen Stecker keine Netzspannung mehr an, der NA-Schutz des Wechselrichters schaltet den Stecker spannungsfrei. Die Berührung der Steckerstifte bleibt ungefährlich.“
    >
    > Kurz gesagt: Wenn der verwendete Wechselrichter die Anforderungen der Wechselrichternorm erfüllt, dann ist der Schukostecker genauso sicher wie der in der Norm empfohlene Spezialstecker der Firma Wieland.

### Zähler

* theoretisch sollte ein Zähler mit Rücklaufsperre verbaut sein, um ein Rückwärtsdrehen zu vermeiden. Diese sind mit einem der zwei Symbole im Bild 12 auf [https://www.heise.de/ct/ausgabe/2013-19-Praxiserfahrungen-mit-einer-Mini-Solaranlage-2315372.html](https://www.heise.de/ct/ausgabe/2013-19-Praxiserfahrungen-mit-einer-Mini-Solaranlage-2315372.html) zu sehen.
* Auch hierzu  [https://www.pv-magazine.de/2019/11/21/legal-oder-illegal-stecker-solar-geraete-im-labyrinth-der-normung/](https://www.pv-magazine.de/2019/11/21/legal-oder-illegal-stecker-solar-geraete-im-labyrinth-der-normung/)  - (Absatz „Aufreger Zählertausch“), die Menge des produzierten und nicht verbrauchten Stroms dürfte praktisch unterhalb der Messtoleranz eines klassischen Zählers liegen.
* Ich werde diesbezüglich auch nochmals bei inetz anfragen, ob nicht sowieso ein Zählertausch ansteht (da bis spätestens 2032 alle Zähler auf digitale umgerüstet sein müssen und in den nächsten Jahren die letzte Phase der Haushaltsumrüstungen beginnt)

### Kosten

* Mini-PV-Anlagen inkl. Mikrowechselrichter und Anschlusskabel kosten ca. 1€ pro Watt Peakleistung (Wp). Typischerweise sind die Module um die 300 Wp ausgelegt. Das heißt eine 1 Modul-Anlage hat ~300 Wp bei einem Preis ab 300€ und eine 2 Modul-Anlage hat bei etwas unter 600 Wp (damit es in die vereinfachte Anmeldung fällt) und hat einen Preis ab 600€.

### Ertrag

* bei senkrechter Anbringung Richtung Süden liegt der Ertrag bei 70% (siehe Tabelle in [https://www.rechnerphotovoltaik.de/photovoltaik/voraussetzungen/dachausrichtung](https://www.rechnerphotovoltaik.de/photovoltaik/voraussetzungen/dachausrichtung))
* für Chemnitz liegt der durchschnittliche Ertrag pro Jahr und pro installierter kW Peak-Leistung bei 954 kWh (siehe [https://photovoltaiksolarstrom.com/photovoltaiklexikon/solarertrag-staedte/](https://photovoltaiksolarstrom.com/photovoltaiklexikon/solarertrag-staedte/))
* bei 300 Wp: `0,3 kWp * 0,7 * 954 kWh/kWp` = 200 kWh, bei 600 Wp: 400 kWh

### Amortisation

* angenommener Strompreis: 0,29 ct/kWh
* gesparte Stromkosten, wenn 90% der erzeugten Energie direkt genutzt wird: 52 € pro Jahr
* Mini-PV-Anlage 300 Wp mit Balkonmontagekit: ~350 € (+~50 € Versand)
* eventuelle Zusatzkosten: Tausch der Steckdose durch Elektriker (80-150€), erhöhte Miete der Zählerstelle (Ferrariszähler liegen bei ~6-13 €, digitale Zähler mit Rücklaufsperre bei ~20€ im Jahr - siehe [https://machdeinenstrom.de/welchen-stromzaehler-braucht-mein-balkonkraftwerk/](https://machdeinenstrom.de/welchen-stromzaehler-braucht-mein-balkonkraftwerk/))
* Amortisation nach rund 8 Jahren (bzw. im Fall von Zusatzkosten bei 13 Jahren)
* Garantie auf die PV-Module und Wechselrichter: 15-25 Jahre

### Beispiele

* dienen nur zu Anschauungszwecken und sind keine Shop-Empfehlungen:
* von fast komplett schwarz über quadrattisch karriert bis hin zum üblichen Erscheinungsbild der größeren Rechtecke mit feinen Linien ist alles auf dem Markt zu finden
* reine Module:
    * [https://www.photovoltaik4all.de/solarmodule-?p=1](https://www.photovoltaik4all.de/solarmodule-?p=1)
* Beispiel mit Wechselrichter und Aufsteller:
    * [https://www.alpha-solar.info/Balkonkraftwerk-Canadian-Solar-325w-Befestigungsarten-versand.html](https://www.alpha-solar.info/Balkonkraftwerk-Canadian-Solar-325w-Befestigungsarten-versand.html)
    * [https://volxpower.de/Stecker-PV-Anlagen](https://volxpower.de/Stecker-PV-Anlagen)


