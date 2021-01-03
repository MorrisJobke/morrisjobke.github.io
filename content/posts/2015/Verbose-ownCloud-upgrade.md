---
categories:
- howto
- owncloud
date: "2015-07-23T07:46:00Z"
title: Verbose ownCloud upgrade
---

ownCloud offers the ability to upgrade an owncloud instance via command line. In the past this was a very quiet command and only listed the very rough steps that were executed. On instances with more user data this results in long times without any visible progress.

    $ ./occ upgrade # 7.0 -> 8.0
    Turned on maintenance mode
    Checked database schema update
    Checked database schema update for apps
    Updated database
    Disabled 3rd-party app: gallery
    Updated <files_sharing> to 0.6.2
    Turned off maintenance mode
    Update successful

This is totally fine if everything works as it should. But there were a few drawbacks with this output. When an app gets updated this is only printed once it is done. It would be more helpful to see [which app gets updated next](https://github.com/owncloud/core/pull/17090), because then you exactly know where to look for problems.

Beside that there are repair steps executed before and after the DB update. It would be nice to also [list these steps](https://github.com/owncloud/core/pull/17088) and know a bit more about what happens during and upgrade.

These two enhancements landed in ownCloud 8.1.1 and the upcoming 8.2.0 when you specify the command line option `-v` which is widely used in other command line tools to increase verbosity too.

    $ ./occ upgrade -v # 8.0 -> 8.1
    Turned on maintenance mode
    Repair step: Repair MySQL database engine
    Repair info: Not a mysql database -> nothing to do
    Repair step: Repair MySQL collation
    Repair info: Not a mysql database -> nothing to no
    Repair step: Repair SQLite autoincrement
    Repair step: Repair duplicate entries in oc_lucene_status
    Repair info: lucene_status table does not exist -> nothing to do
    Repair step: Repair config
    Checked database schema update
    Checked database schema update for apps
    Updated database
    Repair step: Repair mime types
    Repair step: Repair legacy storages
    Repair step: Repair config
    Repair step: Clear asset cache after upgrade
    Repair info: Asset pipeline disabled -> nothing to do
    Repair step: Generate ETags for file where no ETag is present.
    Repair info: ETags have been fixed for 0 files/folders.
    Repair step: Clean tags and favorites
    Repair info: 0 tags for delete files have been removed.
    Repair info: 0 tag entries for deleted tags have been removed.
    Repair info: 0 tags with no entries have been removed.
    Repair step: Drop old database tables
    Repair info: Table locks has been deleted
    Repair step: Drop old background jobs
    Repair step: Repair outdated OCS IDs
    Update successful
    Turned off maintenance mode

Because it is more fine granular you also know more precise what the upgrade process is actually doing.

### Upcoming enhancements

In the next version of ownCloud we also will [add a timestamp to each line of output](https://github.com/owncloud/core/pull/17093) while the `-v` option is specified. Then the admin knows exactly at which time a step happened and can give rough estimates which steps took how long.

    $ ./occ upgrade -v # 8.1 -> 8.2
    2015-07-23T07:32:25+00:00 Turned on maintenance mode
    2015-07-23T07:32:25+00:00 Repair step: Repair MySQL database engine
    2015-07-23T07:32:25+00:00 Repair info: Not a mysql database -> nothing to do
    2015-07-23T07:32:25+00:00 Repair step: Repair MySQL collation
    2015-07-23T07:32:25+00:00 Repair info: Not a mysql database -> nothing to no
    2015-07-23T07:32:25+00:00 Repair step: Repair SQLite autoincrement
    2015-07-23T07:32:26+00:00 Repair step: Repair duplicate entries in oc_lucene_status
    2015-07-23T07:32:26+00:00 Repair info: lucene_status table does not exist -> nothing to do
    2015-07-23T07:32:26+00:00 Repair step: Repair config
    2015-07-23T07:32:26+00:00 Checked database schema update
    2015-07-23T07:32:26+00:00 Checked database schema update for apps
    2015-07-23T07:32:26+00:00 Updated database
    2015-07-23T07:32:26+00:00 Repair step: Repair mime types
    2015-07-23T07:32:26+00:00 Repair step: Repair legacy storages
    2015-07-23T07:32:26+00:00 Repair step: Repair config
    2015-07-23T07:32:26+00:00 Repair step: Clear asset cache after upgrade
    2015-07-23T07:32:26+00:00 Repair info: Asset pipeline disabled -> nothing to do
    2015-07-23T07:32:26+00:00 Repair step: Generate ETags for file where no ETag is present.
    2015-07-23T07:32:26+00:00 Repair info: ETags have been fixed for 0 files/folders.
    2015-07-23T07:32:26+00:00 Repair step: Clean tags and favorites
    2015-07-23T07:32:26+00:00 Repair info: 0 tags for delete files have been removed.
    2015-07-23T07:32:26+00:00 Repair info: 0 tag entries for deleted tags have been removed.
    2015-07-23T07:32:26+00:00 Repair info: 0 tags with no entries have been removed.
    2015-07-23T07:32:26+00:00 Repair step: Drop old database tables
    2015-07-23T07:32:26+00:00 Repair step: Drop old background jobs
    2015-07-23T07:32:26+00:00 Repair step: Remove getetag entries in properties table
    2015-07-23T07:32:26+00:00 Repair info: Removed 0 unneeded "{DAV:}getetag" entries from properties table.
    2015-07-23T07:32:26+00:00 Update successful
    2015-07-23T07:32:26+00:00 Turned off maintenance mode
