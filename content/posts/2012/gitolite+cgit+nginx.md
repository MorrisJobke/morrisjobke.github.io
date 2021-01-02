---
categories:
- howto
date: "2012-02-28T19:00:00Z"
title: gitolite + cgit + nginx
aliases:
- /2012/02/28/gitolite+cgit+nginx
---

Ziel dieser Anleitung soll es sein einen einfach zu wartenden Git-Server einzurichten, der ein einfaches Web-Frontend bietet. Dazu wird gitolite zur Git-Repository-Administration und cgit als Webfrontend verwendet. Das ganze soll unter Ubuntu 10.04 laufen.

## Vorbereitung

Der Git-Server soll unter dem Benutzer *git* laufen und die Repositories in dessen Home-Verzeichnis zu finden sein.

## Gitolite

[Gitolite][] ist eine Skriptsammlung, um den Zugriff auf Git-Repositories auf einem Server komfortabel und sehr genau einzurichten. Für Ubuntu 10.04 gibt es noch kein Paket, also benutzen wir ein Paket aus einer neueren Ubuntu-Version. Dies hat den Vorteil, dass bei einem Upgrade auf Ubuntu 12.04, theoretisch keine Schwierigkeiten auftreten sollten. [Gitolite-Paket][] für Ubuntu 12.04

Installiert wird das Paket folgendermaßen:

	$ sudo dpkg -i gitolite_2.2-1_all.deb

Bei fehlenden Abhängigkeiten werden diese mit folgendem Befehl nachinstalliert.

	$ sudo apt-get install -f

Anschließend wird der eigene SSH-Schlüssel auf den Server geladen und die Konfiguration von gitolite erneut gestartet.

	$ sudo dpkg-reconfigure gitolite

Hierbei sind folgende Angaben zu machen. In Klammern stehen die Werte, die ich benutzt habe.

 * Benutzername (git)
 * Repository-Pfad (/home/git)
 * SSH-Schlüssel des Administrators (ebenjener, der vorher auf den Server kopiert wurde)

Für die Konfiguration wird ein spezielles Repository mit dem Namen `gitolite-admin` benutzt.

	$ git clone GIT-USER@GIT-SERVER:gitolite-admin

Dort werden Zugriffsberechtigungen und Public-Keys der Benutzer hinterlegt. Um auf ein beliebiges Repository des Servers zuzugreifen, kann man folgenden Befehl verwenden.

	$ git clone GIT-USER@GIT-SERVER:GIT-REPO

Damit später CGit auf die Repositories zugreifen kann, muss noch die Berechtigung für die Ordner angepasst werden. Dies geschieht in der Datei **~/.gitolite.rc** des Gitolite-Benutzers - in meinem Fall ist dies **/home/git/.gitolite.rc**. Hier muss die Zeile

	$REPO_UMASK = 0077;

in folgende abgewandelt werden:

	$REPO_UMASK = 0027;

Dadurch erhält die Gruppe Leserechte auf den Repository-Ordner. Da das nur Einfluss auf zukünftig erstellte Ordner und somit Repositories hat, müssen noch die aktuellen Berechtigungen angepasst werden.

	$ chmod -R g+rX /home/git

## CGit

Für [CGit][] gibt es keinerlei Ubuntu-Paket, weshalb es kompiliert werden muss. Dafür sind folgende Paket erforderlich:

	$ sudo apt-get install build-essential git-core libssl-dev

Nun wird der Quellcode bezogen, die letzte als stabil markierte Version ausgecheckt und kompiliert:

	$ git clone git://hjemli.net/pub/git/cgit
	$ cd cgit/
	$ git submodule init git submodule update
	$ git checkout stable
	$ make

Nun könnte CGit mit `make install` ins System installiert werden, jedoch sollen die Dateien an anderer Stelle ins Dateisystem eingepflegt werden, weshalb die benötigten Dateien dahin kopiert werden.

	$ sudo mkdir /usr/lib/cgit/
	$ sudo cp cgit /usr/lib/cgit/cgit.cgi
	$ sudo cp -r filters/ /usr/lib/cgit/
	$ sudo mkdir /var/www/cgit/
	$ sudo cp cgit.css cgit.png /var/www/cgit/

Jetzt wird CGit noch konfiguriert. Die Umgebungsvariable $REPOLIST wird vom Webserver gesetzt. Dies bietet die Möglichkeit mit einer CGit-Konfiguration unterschiedliche Listen von Repositories anzuzeigen. Ein Anwendungsfall ist hier zum Beispiel die Trennung von privaten und öffentlichen Repositories in zwei vHosts, die die Variable $REPOLIST unterschiedlich setzen. Anschließend kann man den "privaten" vHost mit einer Benutzer-Authentifikation versehen.

	root-title=My private cgit server
	root-desc=Private git repositories
	css=/cgit.css
	logo=/cgit.png
	snapshots=tar.gz tar.bz2

	source-filter=/usr/lib/cgit/filters/syntax-highlighting.sh

	virtual-root=/
	enable-index-links=1
	enable-log-filecount=1
	enable-log-linecount=1
	enable-commit-graph=1

	include=$REPOLIST

Damit das Syntaxhighlighting in CGit funktioniert muss noch `highlight` (siehe [syntax-highlighting.sh][]) installiert sein.

	$ sudo apt-get install highlight


## CGit, Gitolite und verschiedene Repository-Listen

Um die Zugehörigkeit der Repositories zu einer Liste einzustellen, wird die Gitolite-Konfiguration erweitert, um das Setzen diverser Variablen pro Repo zu ermöglichen. Dazu muss der Wert für `$GL_GITCONFIG_KEYS` in der Datei **/home/git/.gitolite.rc** angepasst werden.

	$GL_GITCONFIG_KEYS = "cgit\..*"

Nun kann man Git-Config-Variablen nach dem Muster `cgit.VARIABLENNAME` einstellen. Diese werden von einem Skript ausgewertet, was von [Yasin Zähringer][] erstellt wurde. Ich habe es um einige Attribute erweitert - all diese und ihre Zuordnung zur CGit-Option sind in den ersten Zeilen des Skripts zu finden.

Dieses Skript laden wir nun herunter und installieren einen Hook, damit es bei jedem Update des Repositories `gitolite-admin` ausgeführt wird und somit die Listen für CGit aktualisiert werden.

	$ wget http://cgit.mjbk.de/cgit-helper.git/plain/cgit-update.py /home/git/cgit-update.py
	$ chmod +x /home/git/cgit-update.py

Die Datei **/home/git/repositories/gitolite-admin.git/hooks/post-update.secondary** ist (mit den entsprechenden Rechten) zu erstellen und wie folgt zu füllen.

	#!/bin/bash
	python /home/git/cgit-update.py /home/git/repositories /home/git

Ein Konfigurationseintrag für ein Repository sieht nun folgendermaßen aus.

	repo    gitolite-admin
	        RW+                     = kabum
	        config cgit.listname    = "private"
	        config cgit.section     = "Config"
	        config cgit.desc        = "Gitolite Administration"

Er wird in CGit unter der Section `Config` aufgeführt und enthält die Beschreibung `Gitolite Administration`. Er ist lediglich in der Liste `private` zu finden, die vom Skript unter **/home/git/cgit.private.list** abgelegt wird.

## Nginx

Um CGit als CGI-Skript mit Nginx zum Laufen zu bekommen, benötigt man noch das [fcgiwrap-Paket][]. Dieses ist jedoch nicht in Ubuntu 10.04 enthalten, weshalb wir wieder das Paket aus der kommenden Ubuntu-Version 12.04 entnehmen und installieren.

	$ sudo dpkg -i fcgiwrap_1.0.3-3_amd64deb
	$ sudo apt-get -f install

Nun kann nginx konfiguriert werden.

	server {
		listen			80;
		server_name		cgit.example;

		access_log		/var/log/nginx/cgit.log;
		error_log		/var/log/nginx/cgit.error;

		root			/var/www/cgit/;
		proxy_redirect	off;

		location ~* ^.+\.(css|png|ico)$ {
			expires 30d;
		}

		location / {
			include			fastcgi_params;
			fastcgi_pass	unix:/var/run/fcgiwrap.socket;
			fastcgi_param	SCRIPT_FILENAME /usr/lib/cgit/cgit.cgi;
			fastcgi_param	PATH_INFO $uri;
			fastcgi_param	QUERY_STRING $args;
			fastcgi_param	REPOLIST /home/git/cgit.public.list;
		}
	}

Abschließend fügen wir dem Benutzer `www-data`, unter dem der Webserver und somit auch das CGit-Skript läuft, die Zugehörigkeit für die Gruppe `git` hinzu, damit dieser die Repositories lesen kann.

	$ sudo usermod -aG git www-data

## Neustart der Daemons

Abschließend starten wir noch `fcgiwrap` und `nginx` neu, damit sämtliche geänderten Konfigurationen übernommen werden.

	$ sudo service fcgiwrap restart
	$ sudo service nginx restart

## Links

 * gitolite: [https://github.com/sitaramc/gitolite](https://github.com/sitaramc/gitolite)
 * Gitolite-Paket: [http://packages.ubuntu.com/precise/gitolite](http://packages.ubuntu.com/precise/gitolite)
 * CGit: [http://hjemli.net/git/cgit/](http://hjemli.net/git/cgit/)
 * syntax-highlighting.sh: [http://hjemli.net/git/cgit/tree/filters/syntax-highlighting.sh](http://hjemli.net/git/cgit/tree/filters/syntax-highlighting.sh)
 * Yasin Zähringer: [http://www.yhjz.de/www/linux/prog/gitolite-cgit.html](http://www.yhjz.de/www/linux/prog/gitolite-cgit.html)
 * fcgiwrap-Paket: [http://packages.ubuntu.com/precise/fcgiwrap](http://packages.ubuntu.com/precise/fcgiwrap)
 * Gitolite - Artikel auf Dinotools.de: [http://blog.dinotools.de/2012/01/28/gitolite-mehrbenutzer-git-server-uber-ssh](http://blog.dinotools.de/2012/01/28/gitolite-mehrbenutzer-git-server-uber-ssh)
 * CGit und Nginx - Artikel auf Dinotools.de: [http://blog.dinotools.de/2012/01/24/cgit-mit-nginx](http://blog.dinotools.de/2012/01/24/cgit-mit-nginx)


[gitolite]: https://github.com/sitaramc/gitolite
[Gitolite-Paket]: http://packages.ubuntu.com/precise/gitolite
[CGit]: http://hjemli.net/git/cgit/
[syntax-highlighting.sh]: http://hjemli.net/git/cgit/tree/filters/syntax-highlighting.sh
[Yasin Zähringer]: http://www.yhjz.de/www/linux/prog/gitolite-cgit.html
[fcgiwrap-Paket]: http://packages.ubuntu.com/precise/fcgiwrap
[Gitolite - Artikel auf Dinotools.de]: http://blog.dinotools.de/2012/01/28/gitolite-mehrbenutzer-git-server-uber-ssh
[CGit und Nginx - Artikel auf Dinotools.de]: http://blog.dinotools.de/2012/01/24/cgit-mit-nginx
