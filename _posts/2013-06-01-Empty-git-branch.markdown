---
layout: post
title:  "Empty git branch"
date:   2013-06-01 15:59:22 CET
categories: [howto]
---

If you do a complete rework of a project which is in a git repository, the previous history is really irrelevant. It would be great to start a new branch without any history. With the following commands you easily create such a branch. ([via])

	$ git symbolic-ref HEAD refs/heads/NEWBRANCH
	$ rm .git/index

[Via]: http://madduck.net/blog/2007.07.11:creating-a-git-branch-without-ancestry/

