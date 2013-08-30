---
layout: post
title:  "GSoC - Developer Meetup in Berlin and Alpha Release"
date:   2013-08-30 11:25:00 CET
categories: [gsoc, owncloud]
---

### Developer Meetup in Berlin

Between 13th and 19th of August the ownCloud Developer Meetup has taken place
in Berlin. I had a great time there and met most of the ownCloud contributors.
It was an amazing time there and working never was so funny and awesome.

![GSoC picture][Dev-Meetup]

This picture is property of Raghu Nayyar and he gives me the permission to
publish it here. Have a look at his other pictures from the [Developer meetup][flickr].

### Design

During the last three weeks many new features are implemented. One of the
biggest changes is the design. It come up in collaboration with Jan - the
design guy at ownCloud. It represents all artists with their albums and lists
the first 5 tracks of each album. So you are able to quickly browse your whole
music collection and have a nice visual feedback where you are in the collection.

Besides this a generic placeholder for non-existent album covers evolves,
which generates a color out of the album name. *Psst: This is also used by the
upcoming avatar feature.*

![Screenshot][Music-screenshot]

### Features

You are now able to playback your music (e.g. play an album by click on the
album) in every browser out there. The current playtime and the overall time
are shown as well as automatically when a song ends the next song is played
(til the playlist is over).

### Alpha release

Today I packaged the first alpha [release][release] and published it on
[apps.owncloud.com][app]. Keep in mind that this is just an alpha release and
not stable or suitable for production use.

#### Features

 * useable with ownCloud 5+
 * shiva API
 * metadata extraction for artist, album and track
 * single page frontend
 * multimedia playback in all browsers trough HTML5 and flash fallback
 * testing of the backend code

#### Known bugs

 * shuffle, repeat and previous button are out of functionality
 * non-high-resolution icons in IE8
 * no Ampache support
 * slow for large music collections
 * tracks without artist or album are not listed in the frontend (but already in the database)

### **Happy testing!**

[Dev-Meetup]: /images/2013-08-30-Dev-Meetup.jpg
[flickr]: http://www.flickr.com/photos/100400787@N06/
[Music-screenshot]: /images/2013-08-30-Music-screenshot.png
[release]: https://github.com/owncloud/music/releases
[app]: http://apps.owncloud.com/content/show.php/Music?content=160485&PHPSESSID=300bc84464a58ebf5e41ca83130ffbcf