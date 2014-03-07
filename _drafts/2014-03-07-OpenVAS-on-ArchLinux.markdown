---
layout: post
title:  "OpenVAS on ArchLinux"
date:   2014-03-07 19:00:00 CET
categories: [howto]
---

You need to install following packages:

	$ pacaur -S openvas-manager openvas-administrator gsa openvas-scanner

Keep in mind that you need to adopt the PKGBUILD file of gsa because it is outdated. You can find a working version [here][].


Following command downloads the current version of the OpenVAS NVT scripts.

	$ sudo openvas-nvt-sync

Then you need to create a SSL certificate for OpenVAS

	$ sudo openvas-mkcert

Following command creates a private-use certificate for the user `mjob` and registers him to OpenVAS.

	$ sudo openvas-mkcert-client -n mjob -i

Following command starts the OpenVAS scanner service and can take a while.

	$ sudo openvassd


[here]: https://aur.archlinux.org/packages/gsa/
