---
categories:
- howto
date: "2012-02-29T15:03:00Z"
title: Stapelverarbeitung von JPGs - Komprimieren und PDF erstellen
---

Unter Linux kann man recht einfach mehrere JPG-Dateien mit einmal auf eine Qualitätsstufe/Komprimierungsgrad setzen. Anschließend sollen diese noch zu einer PDF zusammen gefasst werden. Dazu wird [ImageMagick](http://www.imagemagick.org/script/index.php) verwendet.

## Vorbereitung

Unter Ubuntu und ArchLinux wird ImageMagick jeweils durch das Paket **imagemagick** bereitgestellt.

## Komprimierung aller JPG-Dateien in einem Ordner

Dies sollte auch mit allen anderen von ImageMagick unterstützten Bildformaten funktionieren.

	$ cd /path/to/folder/with/jpgs
	$ for pic in $(ls *.jpg) ; do convert "$pic" -quality 15% "${pic%%.jpg}-quality15.jpg" ; done;

Dies setzt die Quilität der Kompression aller JPG-Dateien im Ordner auf 15 %, wobei eine niedrigere Zahl in einer geringeren Qualität aber auch in einer geringeren Bildgröße resultiert.

## Zusammenfassung aller Bilder in eine PDF

	$ convert *.jpg merge.pdf

Dieser Befehl erstellt aus allen JPGs im aktuellen Ordner eine PDF mit dem Namen `merge.pdf`. Alternativ kann man auch statt `*.jpg` die zu benutzenden Bilddateien explizit angeben.


## Links

 * ImageMagick: [http://www.imagemagick.org/script/index.php](http://www.imagemagick.org/script/index.php)
 * Ubuntuusers-Wiki zu ImageMagick-Optionen: [http://wiki.ubuntuusers.de/Imagemagick#Bilder-Fotos-nachbearbeiten](http://wiki.ubuntuusers.de/Imagemagick#Bilder-Fotos-nachbearbeiten)
 * Ubuntuusers-Forum-Beitrag zur Stapelverarbeitung: [http://forum.ubuntuusers.de/post/390676/](http://forum.ubuntuusers.de/post/390676/)
 * Umwandlung von JPG in PDF: [http://bitprison.net/jpg_to_pdf](http://bitprison.net/jpg_to_pdf)
