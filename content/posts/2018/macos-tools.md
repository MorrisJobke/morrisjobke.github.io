---
categories:
- howto
date: "2018-05-09T11:47:00Z"
title: macOS tools
---

As I had the [unique opportunity](https://twitter.com/MorrisJobke/status/968878528660955137) to completely reinstall my MacBook I took notes while doing so to know what I actually installed and need on my system. May it help my future self (and maybe others) to get an idea what apps/tools make your life easier.

I will extend this list regularly once I notice missing pieces.

First I installed [Homebrew](https://brew.sh) - package manager for macOS. It keeps all your installed stuff up to date and easy to (un)install.

## brew installs

The following is installed via `brew install NAME`.

* `ack` - [ack!](https://beyondgrep.com) - nicer looking version of a recursive search on the command line - `grep -Rni TERM`
* `diff-so-fancy` - [diff-so-fancy](https://github.com/so-fancy/diff-so-fancy) - nicer to read diffs on the command line - I use it as the default pager for git: `git config --global core.pager "diff-so-fancy | less --tabs=4 -RFX"`
* `ffmpeg` - [ffmpeg](https://ffmpeg.org) - convert multimedia
* `gpg` - [GnuPG](https://gnupg.org) - mostly for signing git commits
* `hub` - [hub](https://hub.github.com) - command line wrapper around git to integrate GitHub (like `git checkout URL-TO-PULL-REQUEST`)
* `hugo` - [hugo](https://gohugo.io) - static website generator
* `iperf` - [iPerf](https://iperf.fr) - measuring network bandwidth
* `jhead` - [jhead](http://www.sentex.net/~mwandel/jhead/) - tool to manipulate images based on their EXIF data
	* shift "picture taken" date by a given amount of time - `jhead -ta+3:14:03` to shift by 3h 14m and 3s
	* rename images to something like `2018-03-04.13-45-34.jpg` with `jhead -n%Y-%m-%d.%H-%M-%S *.jpg`
* `jsonpp` - [jsonpp](https://github.com/jmhodges/jsonpp) - JSON pretty print for the command line
* `jq` - [jq](https://stedolan.github.io/jq/) - an easy to use command line JSON processor
	* `curl -s https://example.org/api/info/queue | jq .stats` to retrieve only the element `stats`
* `python3` - [Python](https://www.python.org) - programming language
* `nmap` - [NMAP](https://nmap.org) - looking around on the network
* `tmate` - [tmate](https://tmate.io) - sharing a terminal with someone else - nice for remote debugging and "screen" sharing (at least if "screen" basically refers to the terminal ;))
* `tmux` - [tmux](http://tmux.github.io/) - a terminal multiplexer - [an introduction how to use it](http://www.hamvocke.com/blog/a-quick-and-easy-guide-to-tmux/)
* `tree` - [tree](http://mama.indstate.edu/users/ice/tree/) - a directory listing command to show a folder and its subfolders in a tree
* `watch` - [watch](https://linux.die.net/man/1/watch) - a tool to periodically execute a command and show the output - I often use it together with ls, tree, etc to check the filesystem content - `watch -n 1 -d COMMAND` to execute `COMMAND` every second and highlight the changed content
* `wget` - [Wget](https://www.gnu.org/software/wget/) - for fetching stuff from the internet
* `yarn` - [yarn](https://yarnpkg.com/lang/en/) - JS package manager
* `youtube-dl` - [youtube-dl](http://rg3.github.io/youtube-dl/) - download videos from YouTube (and many other sites) - used also to archive my YouTube favorites ;)

### PHP

[PHP](https://secure.php.net) - the programming language I use most of the time:

* `brew tap homebrew/homebrew-php`
* `brew install php71 php71-imagick composer` - installs PHP 7.1, the [Imagick](https://secure.php.net/manual/en/book.imagick.php) extension and [Composer](https://getcomposer.org) - remaining stuff is usually executed within docker containers to not clutter my main system

## brew cask installs

Following apps can be installed via `brew cask install NAME`:

* `1password` - [1Password](https://1password.com) - password manager for macOS, Windows, iOS and Android
* `caffeine` - [Caffeine](http://lightheadsw.com/caffeine/) - don't let your mac fall asleep
* `dash` - [Dash](https://kapeli.com/dash) - offline documentation for programming languages, libraries, APIs and RFCs that is easily searchable
* `docker` - [Docker](https://www.docker.com) - container management
* `dozer` - [Dozer](https://github.com/Mortennn/Dozer) - hide menu bar items
* `foldingtext` - [FoldingText](http://www.foldingtext.com) - simple text editor with outline features
* `little-snitch` - [Little Snitch](https://obdev.at/products/littlesnitch/index.html) - firewall and network monitor
* `imageoptim` - [ImageOptim](https://imageoptim.com/mac) - image optimizer (compression, metadata removal)
* `insomnia` - [Insomnia](https://insomnia.rest) - tool to organize HTTP API requests (mainly for testing/debugging)
* `jetbrains-toolbox` - [Jetbrains Toolbox](https://www.jetbrains.com/toolbox-app/) - Toolbox to install and update Jetbrains IDEs
* `moneymoney` - [MoneyMoney](https://moneymoney-app.com) - awesome banking tool
* `phpstorm` - [PhpStorm](https://www.jetbrains.com/phpstorm/) - awesome PHP IDE
* `sublime-text` - [Sublime Text](https://www.sublimetext.com) - a fast and reliable editor with nice features (keeps all unsafed documents, multi cursor editing, ...) for everything where I don't use PhpStorm
	* [Package Control](https://packagecontrol.io) - to easily install following packages
	* [DashDoc](https://github.com/farcaller/DashDoc) - dash integration
	* [JSON Reindent](https://github.com/ThomasKliszowski/json_reindent)
	* [Predawn Twilight Theme](https://github.com/jrnewell/predawn-twilight-theme)
* `timeular` - [Timeular](https://timeular.com) - time tracking with a physical device
* `tunnelblick` - [Tunnelblick](https://tunnelblick.net) - easy to use OpenVPN client
* `tuxera-ntfs` [Tuxera NTFS](https://www.tuxera.com/products/tuxera-ntfs-for-mac/) - driver to write to NTFS disks
* `virtualbox` - [Virtual Box](https://www.virtualbox.org) - for running virtual machines (like the ones from [modern.ie](http://modern.ie) to test Internet Explorer and Edge)
* `vlc` - [VLC media player](https://www.videolan.org/vlc/) - video player for many, many video formats

## AppStore installs

Following apps are installed from the Mac App Store.

* [Magnet](http://magnet.crowdcafe.com) - window manager
* [Paste](https://pasteapp.me) - clipboard manager - `Cmd` + `Shift` + `V` to search through the recent copied elements (bought in app store and thus need to install from app store, also available via brew cask)
* [Shush](http://mizage.com/shush/) - microphone manager to mute your mic with a specified keyboard key 
* [Tweetbot](https://tapbots.com/tweetbot/mac/) - Twitter client for macOS and iOS

## Other apps

* [Chrome](https://www.google.de/intl/de/chrome/browser/) - mostly for testing stuff
* [Firefox](https://firefox.com) - mostly for testing stuff
* [Irvue](http://irvue.tumblr.com) - changes the wallpaper based on [unsplash.com](https://unsplash.com) images on a configured interval
* [Nextcloud](https://nextcloud.com) desktop client - I use this to sync most of my local data to a Nextcloud server (documents, photos, desktop, ...)
* [Pixelmator](http://www.pixelmator.com/) - to manipulate pixel based images
* [virtualenvwrapper](https://virtualenvwrapper.readthedocs.io/en/latest/) - a small tool to make managing virtual python environment even easier (`pip install virtualenvwrapper`)

## Configuration to backup

Locations of crucial configurations and state of apps/tool I use:

* git config - `~/.gitconfig` - see my [blog post](http://morrisjobke.de/2018/03/03/git-config/) for details
* gpg config - `~/.gnupg`
* ssh keys & config - `~/.ssh`
* Insomnia database - `~/Library/Application Support/Insomnia`
* MoneyMoney database -  `~/Library/Containers/com.moneymoney-app.retail/Data/Library/Application Support/MoneyMoney/`
* Tunnelblick config - `~/Library/Application\ Support/Tunnelblick/Users/`

