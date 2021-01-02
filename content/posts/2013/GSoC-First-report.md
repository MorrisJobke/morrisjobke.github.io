---
categories:
- gsoc
- owncloud
date: "2013-06-20T01:52:16Z"
title: GSoC - Weeks -2 to 0
---

It's time for the first report about my progress. More than 3 weeks
has passed since I get the great news to be accepted for this years
Google Summer of Code. Let's have look what is done.

> ### Weeks -2 to 0 - The community bonding period
>
> I'm already bond to the great ownCloud community. So let's start with
> coding the spare time away. ;)
>
> After creating a new appframework based app, I'll add the
> implementation of the RESTful API. In this state it provides just
> dummy data and should be fully tested (in the context of unittesting).
> It'll also be possible to check the implementation against the
> [original client](https://github.com/tooxie/shiva-client).

### AppFramework improvements - refactoring to upstream

I'm a bit behind the initial schedule but also a step ahead the feature
set. In this 3 weeks I've written a [HTTPMiddleware](https://github.com/owncloud/appframework/pull/31) for
AppFramework which provides the authentication based on url passed
credentials like in http://user:pwd@example.org. The JSONResponse get
some more generic methods to set the payload. With [another pull
request](https://github.com/owncloud/appframework/pull/43) for the AppFramework a method to slugify entity attributes
find it's way in. Generic methods for the mapper class to convert
database results to entity instances are prepared in a [third pull request](https://github.com/owncloud/appframework/pull/45).

### Bad news

The current state of the master branch isn't fully tested and the
"fulltree" switch is not (yet) available.

### Good news

It's not just dummy data - in terms of hard coded php objects. The
full stack "controller <-> business layer <-> mapper <-> database" is
implemented. In the following days the tests for all those classes
will be written.

### Good bye "Media app" --- Hello "Music app"

As a result of a [bug report](https://github.com/owncloud/music/issues/27) and express the supposed purpose the
"Media app" changed its name to "Music app".


#### Related posts

 * Bernhard Posselt: [API changes for the next News app release](https://owncloud.bernhard-posselt.com/entry/1/)
 * Bernhard Posselt: [Rest API support for apps](https://owncloud.bernhard-posselt.com/entry/2/)
