---
layout: default
title:  "Collection #1"
date:   "2021-01-03T21:12:00Z"
categories: 
- collection
---

From time to time I have some blog posts, tools, extensions, guides or just little code snippets I want to keep listed somewhere and share it. So this is the first of those collections and was inspired by the [collections of kovah](https://blog.kovah.de/en/collection). 

# Scriptable (iOS)

Scriptable is an iOS app that allows to run JavaScript code snippets on your iOS device. Those can be used as short cuts, siri answers or widgets.

https://scriptable.app

# Dozer - Hiding macOS menu bar entries

The tool Dozer allows to hide specific macOS menu bar elements behind a little dot icon. 

https://github.com/Mortennn/Dozer/

# PXE boot via EdgeRouter and FreeNAS

Using the TFTP server in FreeNAS and the config settings from the reddit post below in an Ubiquity EdgeRouter makes booting via PXE possible.

https://www.reddit.com/r/Ubiquiti/comments/fu71b0/edgerouter_dhcp_configuration_for_pxe_booting/fmbf0yq/

# Beginner's guide to Error Handling in Rust

http://www.sheshbabu.com/posts/rust-error-handling/

# php-memprof #php

> php-memprof is a fast and accurate memory profiling extension for PHP that can be used to find the cause of memory leaks.

https://github.com/arnaud-lb/php-memory-profiler

```bash
brew install traildb/judy/judy
pecl install memprof
```

```php
<?php

if (function_exists('memprof_enabled') && memprof_enabled()) {
	memprof_dump_callgrind(fopen("/tmp/callgrind.out", "w"));
}
```

```bash
MEMPROF_PROFILE=1 php script.php
```
