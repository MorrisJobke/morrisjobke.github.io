---
categories:
- gsoc
- owncloud
date: "2013-05-30T12:26:21Z"
title: Google Summer of Code 2013 - May the fun begin!
---

I'm really proud to be part of this year's Google Summer of Code. My [proposal](https://www.google-melange.com/gsoc/proposal/public/google/gsoc2013/kabum/5727390428823552) is about including a [RESTful](https://en.wikipedia.org/wiki/Representational_state_transfer#RESTful_web_APIs) API for music into ownCloud. It's inspired by the project [shiva](https://github.com/tooxie/shiva-server) and will be based on it's [resource definition](https://github.com/tooxie/shiva-server#resource). Due to the currently unmaintained state of the media app it will include a rewrite from scratch based on the new [appframework](https://github.com/owncloud/appframework)

### Weeks -2 to 0 - The community bonding period

I'm already bond to the great ownCloud community. So let's start with coding the spare time away. ;)

After creating a new appframework based app, I'll add the implementation of the RESTful API. In this state it provides just dummy data and should be fully tested (in the context of unittesting). It'll also be possible to check the implementation against the [original client](https://github.com/tooxie/shiva-client).

### Weeks 1 to 3 - The AngularJS weeks

In these three weeks the frontend - based on AngularJS - will arise. I hope this will be really funny weeks, because AngularJS looks so awesome in contrast to my work with ExtJS.

### Weeks 4 to 9 - The real world data introduction

Six weeks? Why so a huge period? - It's exam time ... and I don't want to mess up my studying.

But there will be a great implementation. At the end of this period the meta data scanner will be fully functional and the dummy data is no more. It's replaced by real database calls. Ready to test this stuff in the wild! Hooray!

### Weeks 10 to 12 - The external APIs period

Ampache, subphonic, ... APIs for music, that determined to be included in delivery, will be implemented in this period. Also it seems to be the time to (have) fix(ed) all upcoming issues, bugs, unlikely behaviours ... or how they are called.

### Week 13

The summer has nearly ended and it's time for vacation.

### Pencils Down

I hope all is finished and the ownCloud community has an all-new and shiny media app with a great UX and functionality (and of course a great maintainablity).

Thanks for reading,

*Morris*
