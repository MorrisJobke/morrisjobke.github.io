---
categories:
- gsoc
- owncloud
date: "2013-07-05T20:18:00Z"
title: GSoC - Weeks 1 and 2
---

In the last two weeks - since my previous report - new features has find it's
way into [feature branches](http://nvie.com/posts/a-successful-git-branching-model/). For a detailed list of all changes have a
look at the [GitHub comparision](https://github.com/owncloud/music/compare/2e1e0fc41d56a392dc13bb1463f173b8a4bc66e9...533dc23397c83b4370f1749a755236c132d6b850) for this period.

### Coverage report

I added the coverage report to the php-unit call for a visualization of the
test coverage. I'm pretty proud to say, that the logic currently is 100 %
covered by the tests. And that is what it looks like:

![code coverage 2013-07-05](/images/2013-07-05-Code-coverage.png)

But I estimated the effort for writing test completely wrong. It tooks a lot
more time to finish this.

### Fulltree modifier

The biggest change is the `fulltree` modifier for the artists and albums
resources. This enables URL like `/artist/2?fulltree=true` which result in
following JSON response with nested artists and tracks. This enables the
original shiva client to work with my ownCloud implementation of the shiva
API - Hooray! First milestone finished!

	{
	  "name": "Test album 2",
	  "year": 2014,
	  "cover": "http://lorempixel.com/200/200/nightlife/5",
	  "uri": "/index.php/apps/music/api/album/2",
	  "slug": "2-test-album-2",
	  "id": 2,
	  "artists": [
	    {
	      "id": 3,
	      "name": "Test artist 3",
	      "image": "http://lorempixel.com/200/200/nightlife/3",
	      "slug": "3-test-artist-3",
	      "uri": "/index.php/apps/music/api/artist/3"
	    }
	  ],
	  "tracks": [
	    {
	      "title": "Test track 2-2",
	      "number": 2,
	      "artist": {
	        "id": 3,
	        "uri": "/index.php/apps/music/api/artist/3"
	      },
	      "album": {
	        "id": 2,
	        "uri": "/index.php/apps/music/api/album/2"
	      },
	      "length": 184,
	      "files": {
	        "audio/mp3": "ab/gh/kl2.mp3"
	      },
	      "bitrate": 128,
	      "id": 7,
	      "slug": "7-test-track-2-2",
	      "uri": "/index.php/apps/music/api/track/7"
	    },
	    {
	      "title": "Test track 2-1",
	      "number": 1,
	      "artist": {
	        "id": 3,
	        "uri": "/index.php/apps/music/api/artist/3"
	      },
	      "album": {
	        "id": 2,
	        "uri": "/index.php/apps/music/api/album/2"
	      },
	      "length": 124,
	      "files": {
	        "audio/mp3": "ab/gh/kl1.mp3"
	      },
	      "bitrate": 128,
	      "id": 6,
	      "slug": "6-test-track-2-1",
	      "uri": "/index.php/apps/music/api/track/6"
	    }
	  ]
	}

In comparision to the same call without the `fulltree` parameter:

	{
	  "name": "Test album 2",
	  "year": 2014,
	  "cover": "http://lorempixel.com/200/200/nightlife/5",
	  "uri": "/index.php/apps/music/api/album/2",
	  "slug": "2-test-album-2",
	  "id": 2,
	  "artists": [
	    {
	      "id": 3,
	      "uri": "/index.php/apps/music/api/artist/3"
	    }
	  ]
	}

For the next weeks I plan to work on the frontend player based on AngularJS.

### Welcome package

At the 24th of June I received my welcome package from Google. Besides the
really damaged package everything was fine. I got a lot of paperwork, my prepaid credit card, a
note-book and a pencil. The two latter are branded with the Google Summer of
Code logo.

![Welcome package](/images/2013-06-25-GSoCWelcomePackage.jpg)
