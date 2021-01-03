---
categories:
- howto
- owncloud
date: "2015-01-09T00:39:45Z"
title: Issue Counting Formula
---

Yesterday I fetched [issue data](http://morrisjobke.de/2015/01/08/GitHub-Issue-Count-Graphs/) from GitHub. Today I extended this with a formula a coworker came up as he said that tickets aren't the same. You need to rate critical tickets higher, than little glitches that occur nearly never. So he proposed to rate critical tickets with 100, normal bugs with 10 and all other tickets with 1.

So I adjusted the script and also add the ability to create that ranking for sub components that are part of the same bug tracker.

In each graph you clearly see the period after a release with a higher amount of incoming tickets and then some time later a bug fixing period.

This is how the overall ranking graph looks like:

![ranking](/images/2014-01-09-ranking.png)

We have separate labels for the core apps, so I calculated their ranking and it looks like this:

![apps ranking](/images/2014-01-09-apps-ranking.png)

