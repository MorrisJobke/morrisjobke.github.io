---
layout: post
title:  "Blackfire PHP Profiler on ArchLinux"
date:   2015-01-15 09:53:00 CET
categories: [howto]
---

[Blackfire](https://blackfire.io) is a PHP profiler. To get it run you need the agent and the probe. The probe is a PHP extensions that gathers all the data of the request. The agent is a daemon that is running on your system and processes the data of the probe and sends it to blackfire.io, where the data is then visualized.

![Profile run][blackfire]

I created two AUR packages: one for the [agent](https://aur.archlinux.org/packages/blackfire-agent/) and one for the [PHP extension](https://aur.archlinux.org/packages/php-blackfire/). You can find both PKGBUILDs in my [GitHub repo](https://github.com/MorrisJobke/aur-packages).

# How to set it up

## Agent

For the agent you need to simply install the `blackfire-agent` package from AUR. It will prompt you the remaining steps you need to do:

	You need to configure your Blackfire credentials via: sudo -u blackfire-agent blackfire-agent -register

This will prompt you for the missing information (server-id and server-token) which you can look up once you have logged into your blackfire.io account.

The agent comes with a systemd service file. So you can simply start the agent with following command:

	$ sudo systemctl start blackfire-agent

If you wish you can also enable it to start the agent on each boot:

	$ sudo systemctl enable blackfire-agent

That's all for the agent!

## Probe

The PHP extension can be installed with the AUR package `php-blackfire`. It comes with a default config in `/etc/php/conf.d/blackfire.ini`. There you also need to specify the server id and the server token and uncomment the line

	extension="/usr/lib/php/modules/blackfire.so"

Keep in mind that this extension can interfere with the XDebug and XHProf extension. Therefore those should be disabled.

Then simply restart the webserver or the PHP-FPM service.

## Browser extension

The easiest way to invoke the profile run is by installing the Chrome/Chromium [browser extension](https://chrome.google.com/webstore/detail/blackfire-companion/miefikpgahefdbcgoiicnmpbeeomffld). Click on the icon in the browser toolbar, select the slot and click "Profile". That's it!

Now run it on all your PHP pages and improve their performance! ;)




[blackfire]: /images/2015-01-15-blackfire-profiler.png
