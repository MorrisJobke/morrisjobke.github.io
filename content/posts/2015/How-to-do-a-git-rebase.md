---
categories:
- howto
date: "2015-12-03T09:51:00Z"
title: How to do a git rebase
---


How I do the git rebase:

* checkout master and fetch the latest stuff:

		$ git checkout master
		$ git pull

* switch to the branch that should be rebased and start an interactive rebase:

		$ git rebase --interactive master

* an editor will open that lists of all your commits that should be rebased - close it which means that all commits should stay as they are (e.g. no squashing of multiple commits into one)
* (a) this will apply each commit and will stop once it can’t be applied properly (conflict) or once all commits are applied (goto (b))
* check carefully the `git status` output and resolve the conflicts (`both modified` entries)

<img src="/images/2015-12-03-merge-conflicts.png" alt="merge conflict" style="max-width: 400px; display:block; margin:auto;"/>

* once this is done - add the changed files as usual with

		$ git add FILENAME

* once all files with conflict are fixed the rebase can be continued:

		$ git rebase --continue

* this will use the old commit message and bundle the merge conflict changes into it
* now it will continue at (a)
* (b) once the rebase is done checking the current branch is always good

I use my own alias [`git lg`](https://twitter.com/MorrisJbk/status/661459993422639104) that shows clearly how many commits are between the current branch and the master (should only be yours). A `git lg` with 8 commits above master on branch `def` looks like this:

<img src="/images/2015-12-03-git-lg.png" alt="merge conflict" style="max-width: 700px; display:block; margin:auto;"/>

Note: The tricky part is to keep calm during the initial rebase session ;)
