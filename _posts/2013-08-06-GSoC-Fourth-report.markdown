---
layout: post
title:  "GSoC - Weeks 6 and 7"
date:   2013-08-06 15:15:00 CET
categories: [gsoc, owncloud]
---

### Browse your music collection

I implemented the first part of the AngularJS music browser, which uses the
Shiva API to retrieve the data. I just extend the API a bit to better fulfill
this task.

##### Artists view
![artists view][artists]

##### Albums view
![albums view][albums]

##### Tracks view
![tracks view][tracks]

You're now able to browse artists, albums and tracks. As this is in an early
stage you couldn't click to get a filtered view of albums of this artists, but
this will arrive soon.

All changes to the AngularJS app will take effect in the `angularjs` branch on
GitHub.

### Further steps

There are a lot of features which want to be implemented:

 - playlists support (add, delete, edit and add or remove tracks)
 - search/filter support (maybe based on autocomplete multiselect - something fancy - see [chosen][chosen])
 - real music player
 	- play settings (shuffle, repeat)
 - cover art
 - file based music selection (OC.dialog.filepicker)

### Developer meetup in Berlin

Next week many ownCloud developers will meet in Berlin - including me. :) I'm
looking forward to the great discussions, get to know the real persons and not
just their IRC nick names and having a brilliant time with awesome results.


[artists]: /images/2013-08-06-artists.png
[albums]: /images/2013-08-06-albums.png
[tracks]: /images/2013-08-06-tracks.png
[chosen]: http://harvesthq.github.io/chosen/
