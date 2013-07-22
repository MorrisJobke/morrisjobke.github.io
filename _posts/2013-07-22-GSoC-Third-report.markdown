---
layout: post
title:  "GSoC - Weeks 3, 4 and 5"
date:   2013-07-22 11:59:00 CET
categories: [gsoc, owncloud]
---

### Goals reordered

I have decided to reorder my goals. In my proposal I wrote, that I implement
the API first, then write the AngularJS frontend, after that the metadata
scanner and finally external APIs like Ampache. I changed the position of the
frontend and metadata scanner implementation as this is more reasonable.

### Travis is awesome

I introduced continous integration through [Travis CI][] and it's so helpful,
because it checks every commit and runs the whole test suite on it's own.
After it has finished you get an email with the result. And there is nothing
better than an email with "The build passed." in brilliant green at the end of
your working day . :)

### Metadata extraction

At the last weekend I implemented the metadata scanner. Currently it uses the
[getID3][] library to extract the metadate from files. After this it caches
them inside of the database. After a file deletion the corresponding metadata
will be deleted. Thus a consistent state of the metaddata inside of the
database is guranteed.

The hooks I used for the detection of changes works properly for file uploads
or deletion done through the web interface but if I use WebDAV they weren't
emitted.

### Next steps

In the next days I will add further tests, check against audio files with
incomplete metadata and introduce debug logging. The latter should make it
easy for others to send me bug reports and for me to figure out where
something is broken.

### Want to see it in action?

The [README][] contains a instruction how to set up the original shiva client
(because the ownCloud one isn't implemented yet). You need to checkout the
`music-scanner` branch of the [music app repository][] (this will be
merged soon in the `master` branch).

The changes of the last days can be found in this overall [GitHub diff][] or
splitted up into two separate diffs [1][] and [2][] to exclude the import of
the getID3 library.

Today I migrated the old `future` branch to `master` because the old media app
is maintained inside of the [apps repository][] in the `stable5` branch.


[Travis CI]: https://travis-ci.org
[getID3]: https://github.com/JamesHeinrich/getID3
[README]: https://github.com/owncloud/music/blob/master/README.md
[music app repository]: https://github.com/owncloud/music
[GitHub diff]: https://github.com/owncloud/music/compare/7d25859e500dec7c442bf464c1a856467fd31ab1...fab5b57f331aaa6ca0dd7775da52f2a7c77006c3
[1]: https://github.com/owncloud/music/compare/7d25859e500dec7c442bf464c1a856467fd31ab1...daca8eda4183867dd2dc3b75b043fc68793f3ee2
[2]: https://github.com/owncloud/music/compare/0abfbf00ba69330b2fdd85f0844ec8e551fe4869...fab5b57f331aaa6ca0dd7775da52f2a7c77006c3
[apps repository]: https://github.com/owncloud/apps
