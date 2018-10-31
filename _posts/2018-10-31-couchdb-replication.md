---
layout: post
title:  "CouchDB - filtered replication by example"
date:   2018-10-31 22:53:00 CET
categories: [howto]
---

I just want to document what is needed to get a filtered replication between two databases inside CouchDB 2.2.0 up and running. I spend quite some time figuring it out and couldn't find any useful resource that sums it up.

I used capital cased names where specific names need to be placed - if there is the same placeholder it needs to be the same name.

The goal is to replicate `SOURCE_DATABASE` to `TARGET_DATABASE` while applying a filter function that is named `DESIGNDOCUMENT/NAME`. The filter function in the example checks if the attribute `_id` starts with `abc`.

First we create a document that describes the filter. This document is a design document and needs to be placed in the source database under `/SOURCE_DATABASE/_design/DESIGNDOCUMENT`:

```json
{
  "_id": "_design/DESIGNDOCUMENT",
  "filters": {
    "NAME": "function(doc, req) { if (doc._id.substr(0, 3) === 'abc') { return true; } return false; }"
  }
}
```

To actually test the filter I used a query against the `/_changes` endpoint of the database:


```bash
$ curl "http://DOMAIN:5984/SOURCE_DATABASE/_changes?filter=DESIGNDOCUMENT%2FNAME&feed=normal&style=all_docs&since=0&timeout=10000"
```

This shows the output and potential errors in the JavaScript code of the filter.

For the actual replication you need to create following document for the one time replication of existing documents:

```json
{
  "_id": "ID",
  "source": "http://DOMAIN:5984/SOURCE_DATABASE",
  "target": "http://DOMAIN:5984/TARGET_DATABASE",
  "filter": "DESIGNDOCUMENT/NAME"
}
```

And I created a continuous replication as well:

```json
{
  "_id": "ID-continuous",
  "source": "http://DOMAIN:5984/SOURCE_DATABASE",
  "target": "http://DOMAIN:5984/TARGET_DATABASE",
  "filter": "DESIGNDOCUMENT/NAME",
  "continuous": true
}
```

Following documentation pages helped me to understand how the replication works in general:

* [Introduction to Replication](http://docs.couchdb.org/en/stable/replication/intro.html)
* [Replicator Database](http://docs.couchdb.org/en/stable/replication/replicator.html)
* [Filter function documents](http://docs.couchdb.org/en/stable/ddocs/ddocs.html#filterfun)

Quite interesting in this regard could also be the blog post ["Filtered replication: from Couch to Pouch and back](https://pouchdb.com/2015/04/05/filtered-replication.html) at the PouchDB blog.


