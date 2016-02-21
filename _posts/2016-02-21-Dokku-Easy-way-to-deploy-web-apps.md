---
layout: post
title:  "Dokku - An easy way to deploy web apps"
date:   2016-02-21 17:55:00 CET
categories: [howto]
---

> TL;DR Use easy-to-setup [Dokku](http://dokku.viewdocs.io/dokku/) and push your code via git to have it deployed in a docker container without hassle. Bonus points for subdomain and Let's Encrypt support.

I was looking for a nice way to deploy some small apps quickly without juggling all the time webserver configs and think about the ports or domains under which I should host them.

Additionally I want to play with [docker](https://docker.io) to deploy apps. I searched a bit and have found [Kubernetes](http://kubernetes.io) - a scalable solution to run any docker container in a clustered high availablilty environment. This sounded a bit overkill for my scenario - but I gave it a try. I have choosen the [docker based installation method](http://kubernetes.io/v1.1/docs/getting-started-guides/docker.html). It was easy, quite fast and started a ton of services. In the end it revealed that it was a total overkill and needed more configuration than the old way based on web server config juggling. I abandoned this tool quickly because I realized that it was build for really large setups.

# Dokku

Then I found [Dokku](http://dokku.viewdocs.io/dokku/) which advertises it self as "The smallest PaaS implementation you've ever seen - Docker powered mini-Heroku in around 200 lines of Bash".

What does Dokku do? Basically it sets up a Nginx webserver, a user called dokku and brings a command line tool. Now you are able to use a git repository that is based on either a [Heroku image](http://dokku.viewdocs.io/dokku/deployment/buildpacks/) or contains a [Dockerfile](http://dokku.viewdocs.io/dokku/deployment/dockerfiles/) as base for the deployment. Just add the dokku host as a git remote (with the git repo name as the name it should be managed on the dokku server) and push to that repository:

	$ git remote add dokku dokku@example.org:php
	$ git push dokku master
	Counting objects: 145, done.
	Delta compression using up to 8 threads.
	Compressing objects: 100% (80/80), done.
	Writing objects: 100% (145/145), 28.77 KiB | 0 bytes/s, done.
	Total 145 (delta 54), reused 139 (delta 51)
	-----> Cleaning up...
	-----> Building php from herokuish...
	...
	... output truncated for brevity
	...
	=====> Application deployed:
	       http://example.org:32770 (container)
	       http://example.org:80 (nginx)

	To dokku@example.org:php
	 * [new branch]      master -> master

That's all - now the app could be accessed by browsing `http://example.org:32770`. Dokku also supports subdomain based apps and SSL, but I will talk about this later.

For the above example I used Herokus [Getting started with PHP](https://github.com/heroku/php-getting-started).

## Dokku on ArchLinux

The only drawback of this tool was that only Ubuntu 14.04 was supported, but I thought it couldn't be too hard to somehow make those 200 lines of Bash compatible with ArchLinux (which I currently use on my server). It turned out that 200 lines of Bash is the optimistic view on the code base, but how cares - I fixed the issues I had, [opened a pull request](https://github.com/dokku/dokku/pull/1918), [updated the AUR package](https://aur.archlinux.org/packages/dokku/) and had some nice chats with the maintainers. Now the setup on ArchLinux is a breeze:

	$ pacaur -S dokku
	$ cat ~/.ssh/id_rsa.pub | sudo sshcommand acl-add dokku 'local key'

The above three commands install the `dokku` package and add the local SSH key as allowed authentication token. With this command other SSH keys could also be added to gain management access to the dokku server.

## Subdomains a.k.a. vhosts

By default dokku deploys all the different apps on different ports, but it also supports domains and subdomains. To use this feature just add a wildcard DNS to point to the dokku server and specify that domain in dokku as vhost base domain.

	$ cat /var/lib/dokku/VHOST
	example.org

Then dokku will catch up this setting and deploy all apps as `APPNAME.example.org` instead of `example.org:32770`.

The above deploy step then looks like this:

	$ git remote add dokku dokku@example.org:php
	$ git push dokku master
	-----> Cleaning up...
	-----> Building php from herokuish...
	...
	... output truncated for brevity
	...
	=====> Application deployed:
	       http://php.example.org

The app now could be accessed by browsing `http://php.example.org`.

## HTTPS via Let's Encrypt

End of last year Let's Encrypt entered [public beta](https://letsencrypt.org/2015/12/03/entering-public-beta.html). As it was build to allow automatic SSL certificate creation it is the ideal way to bring HTTPS to those dokku apps.

Dokku allows to be extended by so called plugins and as you already imagine there is a plugin to bring Let's Encrypt and Dokku together. It was build by [Stefan Seemayer](https://blog.semicolonsoftware.de/securing-dokku-with-lets-encrypt-tls-certificates/) and could be installed with following command:

	$ sudo dokku plugin:install https://github.com/dokku/dokku-letsencrypt.git

 Then only two commands are left to have a fully working HTTPS setup for the `php` app:

	$ ssh dokku@example.org letsencrypt:email php email@example.org
	=====> Setting Let's Encrypt e-mail address for php to 'email@example.org'
	$ ssh dokku@example.org letsencrypt php
	=====> Let's Encrypt php...
	-----> Updating letsencrypt docker image...
	latest: Pulling from m3adow/letsencrypt-simp_le
	...
	... output truncated for brevity
	...
	-----> Certificate retrieved successfully.
	-----> Symlinking let's encrypt certificates
	-----> Configuring SSL for php.example.org...
	-----> Creating https nginx.conf
	-----> Running nginx-pre-reload
	       Reloading nginx
	-----> Disabling ACME proxy for php...
	       done

Now your app is secured by an Let's Encrypt certificate. Also the redirects from unencrypted to the encrypted version of your app are set up.

Tip: Checkout the help section of the command:

	$ ssh dokku@cloud.morrisjobke.de help            
	Usage: dokku [--quiet|--trace|--rm-container|--rm|--force] COMMAND <app> [command-specific-options]

	Options:
	apps                                                                                         List your apps
	apps:create <app>                                                                            Create a new app
	apps:destroy <app>                                                                           Permanently destroy an app
	apps:rename <old-app> <new-app>                                                              Rename an app
	backup:export [file]                                                                         Export dokku configuration files
	backup:import [file]                                                                         Import dokku configuration files
	...

## Show me the commands!

* add git remote

		$ git remote add dokku dokku@example.org:APPNAME

* deploy (update of) an app

		$ git push dokku master

* configure Let's Encrypt plugin

		$ ssh dokku@example.org letsencrypt:email APPNAME email@example.org

* switch to Let's Encrypt based HTTPS setup for an app

		$ ssh dokku@example.org letsencrypt APPNAME

## Thanks

A huge thanks to all Dokku contributors for this really nice tool! A special thanks to [Michael Hobbs](https://github.com/michaelshobbs) for answering me my first questions about it on IRC, [Jose Diaz-Gonzalez](http://josediazgonzalez.com) for reviewing my pull request and [Stefan Seemayer](https://blog.semicolonsoftware.de/securing-dokku-with-lets-encrypt-tls-certificates/) for the Let's Encrypt plugin. It was a very warm welcome in that community :) The open source community rocks all the time and I'm hugely impressed again and again :)
