---
title:  "Collection #2"
date:   "2021-01-10T13:01:00Z"
categories:
- collection
---

# Apple TV screensavers

As the Apple TV screensavers are really beautiful I hecked how to get them to have them also on other devices. The website below provides all of them with options to download them.

https://bzamayo.com/watch-all-the-apple-tv-aerial-video-screensavers

# Undoing a git bisect mistake

If one does a long git bisect it could happen that a commit is wrongly marked as bad/good. There is a nice way to fix this and not need to manually do the already done bisecting steps again.

Basically this boils down to dumping the history of the bisect with `git bisect log`, then edit the wrong bisect marking, reset it with `git bisect reset` and finally replay the log via `git bisect replay FILE`.

https://stackoverflow.com/questions/8594758/undoing-a-git-bisect-mistake#8594800

The log itself looks like this:

```bash
$ git bisect log
git bisect start
# good: [73c2ad293bc2ad12126fe42e8c3f3dd137bab2ab] Merge pull request #23998 from nextcloud/backport/23937/stable20
git bisect good 73c2ad293bc2ad12126fe42e8c3f3dd137bab2ab
# bad: [785029d69332d83773a19c75077f25779fabd55a] Merge pull request #24424 from nextcloud/dependabot/npm_and_yarn/css-vars-ponyfill-2.4.2
git bisect bad 785029d69332d83773a19c75077f25779fabd55a
# good: [e14ba58b6d2fec3702e4ad1f8445ccc74d922beb] Merge pull request #22794 from nextcloud/version/20.0.0/rc1
git bisect good e14ba58b6d2fec3702e4ad1f8445ccc74d922beb
# bad: [3076eb4e9bb4006a5670aa08a2b35d2cfa42cd6c] Merge pull request #23767 from nextcloud/techdebt/noid/add-deprecated-tag-to-all-methods
git bisect bad 3076eb4e9bb4006a5670aa08a2b35d2cfa42cd6c
# bad: [d51da5714b20544673aec6be3ff7fec1f67cb20b] Merge pull request #23371 from nextcloud/enhancement/psalm-typed-bootstrap-registration-context
git bisect bad d51da5714b20544673aec6be3ff7fec1f67cb20b
# good: [020ba0847ba4fd8af39e9a730e3d8460cc56c9e3] Bump moment from 2.27.0 to 2.29.0
git bisect good 020ba0847ba4fd8af39e9a730e3d8460cc56c9e3
```

# SQL for CSV/Excel files

`q` is a handy tool to run SQL-like queries on CSV or TSV files. (via [@danimo](https://twitter.com/danimo/status/1337363460280217604))

https://harelba.github.io/q/


# Minimal Viable Search using Postgres

This blog post describes how to build some quick and easy way to implement search within Postgres.

http://www.sheshbabu.com/posts/minimal-viable-search-using-postgres/

Feedback by [@brainafk](https://twitter.com/brainafk/status/1290897820711620608) is to maybe use a stored generated column instead of the trigger.
