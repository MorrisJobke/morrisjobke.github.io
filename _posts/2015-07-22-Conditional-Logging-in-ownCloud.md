---
layout: post
title:  "Conditional Logging in ownCloud"
date:   2015-07-22 21:46:00 CET
categories: [howto, owncloud]
---

If you have an installation of ownCloud and you experience a bug it is quite difficult to debug the problem, because usually only errors and warnings are logged. But to identify a problem it is really helpful to log more stuff to get all the information that are needed. Wouldn't it be cool to raise the log level from "warning" to "debug" just for specific conditions? That's what [I just added some time ago](https://github.com/owncloud/core/pull/15965) and it landed in ownCloud 8.1.

## Conditions

There are three different types of conditions that can be used:

 * use a **shared secret** that is added to a request
 * a list of **users**
 * a list of **apps**

Once the log message meets one of the set up conditions it will be logged no matter what the log level is.

## Example settings

To setup conditional logging the following config parameter needs to be set in `config.php`:

	'log.condition' => [
		'shared_secret' => '57b58edb6637fe3059b3595cf9c41b9',
		'users' => ['sample-user'],
		'apps' => ['files'],
	],


By default `log.condition` is set to an empty array. All array entries are optional. This means that it is also allowed to only specify the list of apps.

The options for users and apps are quite self-explanatory: If the request is made by a user that is in the list all log messages will be written or if the log message is raised by an app that is in the list then all log messages will be written.

The option `shared_secret` is something special to pick a very specific use case. If you notice that for a single request something bad happens then you can add a parameter named `log_secret` with the exact same value as set in the config to log all messages of this single request. Be aware to use a random string that isn't easy to guess and remove the setting once it is not needed anymore.

For example following request will cause to write all log messages during the execution (given that the above config is applied):

	curl -X PROPFIND -u another-user:password \
	  https://own.cloud/remote.php/webdav/\?log_secret\=57b58edb6637fe3059b3595cf9c41b9

The parameter doesn't need to be a GET parameter - a POST parameter will also be detected.
