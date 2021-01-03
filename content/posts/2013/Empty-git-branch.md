---
categories:
- howto
date: "2013-06-01T13:59:22Z"
title: Empty git branch
---

If you do a complete rework of a project which is in a git repository, the previous history is really irrelevant. It would be great to start a new branch without any history. With the following commands you easily create such a branch. ([via](http://madduck.net/blog/2007.07.11:creating-a-git-branch-without-ancestry/))

	$ git symbolic-ref HEAD refs/heads/NEWBRANCH
	$ rm .git/index
