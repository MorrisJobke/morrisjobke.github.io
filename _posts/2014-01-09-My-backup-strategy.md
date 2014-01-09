---
layout: post
title:  "My backup strategy"
date:   2014-01-09 01:10:59 CET
categories: [howto]
---

For a long time I didn't have a real backup strategy. I just had some HDDs
with - hopefully - more than one copy of the important files. Then I decided
to not trust into such a more or less random backup and build up a software
RAID (RAID 5) using `mdadm`. Yeah, I know RAID isn't a real backup but it's a
lot better than just having single HDDs which could fail and then the data is
gone. After a while I wrote a little script to push essential files to the
universities file system - the first solution that was allowed to name itself
backup.

## Hardware

As my data grows it was time for the next step - a backup with more capacity.
So I bought an external hard drive and a timer clock. Put the latter into
plug-socket, setting up the timer for an suitable time slot, connect the power
supply of the external hard drive to the timer clock and the USB into the PC -
ready to set up the software part.

The actual backup is done using [rsnapshot][] ([Ubuntuusers wiki - german][]).
To invoke this a litte helper script is called using a udev rule.

## udev rule

I used following udev rule (located in `/etc/udev/rules.d/10-usb-hdd-backup.rules`).
The rule needs to be applied before the udisks2 rules (on ArchLinux the rule is
named [80-udisks2.rules][]).

	SUBSYSTEMS=="usb",ACTION=="add",KERNEL=="sd?1",ATTRS{serial}=="aabbccdd",
		SYMLINK+="backup",RUN+="path/to/backup.sh",ENV{UDISKS_IGNORE}="1"

This calls the script `path/to/backup.sh` right after the recognition of the
usb device. `SYMLINK+="backup"` creates the device `/dev/backup` for an easier
device handling and `ENV{UDISKS_IGNORE}="1"` disables the [auto mount of
udisks2][].

To retrieve the serial just plug the drive in and look up the serial with
following command (where `/dev/sdf` is the external device).

	$ udevadm info -a -p  $(udevadm info -q path -n /dev/sdf) | grep serial

## backup.sh

This is the script I used to invoke rsnapshot. It mounts the partition, checks
for the folders `daily.0`, `weekly.0` and `monthly.0` inside of the
destination folder to call the daily part once a day, the weekly part once
every 7 days and the monthly once every 30 days. Then it unmounts the
partition and suspends the USB device using [this script][].

	#!/bin/bash
	#
	#  script to use rsnapshot with a (not always mounted) external hard
	#  drive (this script gets called by an udev rule)
	#  see http://morrisjobke.de/2014/01/09/My-backup-strategy/
	#
	#  Copyright (C) 2014, Morris Jobke <morris.jobke@gmail.com>
	#
	#  This program is free software: you can redistribute it and/or modify
	#  it under the terms of the GNU General Public License as published by
	#  the Free Software Foundation, either version 3 of the License, or
	#  (at your option) any later version.
	#
	#  This program is distributed in the hope that it will be useful,
	#  but WITHOUT ANY WARRANTY; without even the implied warranty of
	#  MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
	#  GNU General Public License for more details.
	#
	#  You should have received a copy of the GNU General Public License
	#  along with this program.  If not, see <http://www.gnu.org/licenses/>.

	DEV_NAME=/dev/backup
	MNT_NAME=/mnt/backup
	BACKUP_FOLDER=${MNT_NAME}/wigrid.snapshots
	SUSPEND_USB_SCRIPT=path/to/suspend-usb-device.sh

	# create directory if needed
	if [ ! -d "$MNT_NAME" ]; then
		mkdir "$MNT_NAME"
	fi

	# mount
	mount -t auto "$DEV_NAME" "$MNT_NAME"

	logger "rsnapshot started"

	if [ ! -d "${BACKUP_FOLDER}/daily.0" ] || [ $(find ${BACKUP_FOLDER}/daily.0
		-maxdepth 0 -type d -mtime +1 | grep daily) ]; then
		# if no daily backup is available or daily backup is older than 1 day
		# then run daily
		logger "rsnapshot daily"
		rsnapshot daily
	fi

	if [ ! -d "${BACKUP_FOLDER}/weekly.0" ] || [ $(find ${BACKUP_FOLDER}/weekly.0
		-maxdepth 0 -type d -mtime +7 | grep weekly) ]; then
		# if no weekly backup is available or weekly backup is older than 7 days
		# then run weekly
		logger "rsnapshot weekly"
		rsnapshot weekly
	fi

	if [ ! -d "${BACKUP_FOLDER}/monthly.0" ] || [ $(find ${BACKUP_FOLDER}/monthly.0
		-maxdepth 0 -type d -mtime +30 | grep monthly) ]; then
		# if no monthly backup is available or monthly backup is older than 30 days
		# then run monthly
		logger "rsnapshot monthly"
		rsnapshot monthly
	fi

	logger "rsnapshot finished"

	#unmount
	umount "$MNT_NAME"

	# suspending USB device
	# from https://raw2.github.com/vain/suspend-usb-device/master/suspend-usb-device
	${SUSPEND_USB_SCRIPT} "$DEV_NAME"

I'm open for suggestions to improve this.

[auto mount of udisks2]: https://bbs.archlinux.org/viewtopic.php?pid=1157811#p1157811
[80-udisks2.rules]: https://www.archlinux.org/packages/extra/x86_64/udisks2/
[this script]: https://github.com/vain/suspend-usb-device/blob/master/suspend-usb-device
[rsnapshot]: http://www.rsnapshot.org/
[Ubuntuusers wiki - german]: http://wiki.ubuntuusers.de/rsnapshot