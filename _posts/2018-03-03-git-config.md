---
layout: post
title:  "My git config"
date:   2018-03-03 13:30:00 CEST
categories: [howto]
---

Because I get quite often asked about my git config I will post it here and leave some notes.


```
[core]
	whitespace = cr-at-eol
	pager = diff-so-fancy | less --tabs=4 -RFX
	excludesfile = ~/.gitignore
[user]
	name = Morris Jobke
	email = ...
	signingkey = FE03C3A163FEDE68
[alias]
	lg = log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit
	cu = "!git branch --merged | grep -v '\\*' | grep -v master | xargs -n 1 git branch -d"
[color]
	ui = true
	branch = auto
	diff = auto
	interactive = auto
	status = auto
	pager = true
	ui = true
[push]
	default = simple
[color "status"]
	added = green
	changed = yellow
	untracked = red
[color "branch"]
	current = green reverse
	local = green
	remote = red
[commit]
	gpgsign = true
[receive]
	fsckObjects = true
[gpg]
	program = gpg
```

In the `core` section there is one config to use [Carriage return](https://en.wikipedia.org/wiki/Carriage_return) for all line endings, the `diff-so-fancy` diff highlighter (see brew install section) and a global ignore list for git.

The `user` section holds the information that all my commits should contain.

I have two aliases - `git lg` gives a nice and simple overview of the git history:

![git lg](/images/2018-03-03-git-lg.png)

The second alias cleans up the git repository from branches that are fully merged in the current branch. Usually you call this on your master and stable branches to cleanup merged feature or backport branches. Just call `git cu` and you are done.

The default colors in git are quite boring so I use basically green (staged), yellow (changed) and red (untracked) as indicator for different sections in the git status and git branch outputs.

![git status](/images/2018-03-03-git-status.png)

The remaining sections are just for push behaviour and that commits should be signed by default.
