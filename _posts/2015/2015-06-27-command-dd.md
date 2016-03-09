---
layout: post
title: "command dd"
description: ""
category: [command]
tags: [dd, copy, convert, format]
---
{% include JB/setup %}


### info

1. `dd` command copies a file

	1. converting the format of the data in the process

	1. according to the operands specified

### operands

1. `bs` read and write bytes at a time

	1. `ibs` read bytes at a time (default 512)

	1. `obs` write bytes at a time (default 512)

1. `cbs` convert bytes at a time

1. `conv` comma separated symbol list

	1. `ascii` from [ebcdic](http://www.computerhope.com/jargon/e/ebcdic.htm) to [ascii](http://www.computerhope.com/jargon/a/ascii.htm)

	1. `ebcdic` from ascii to ebcdic

	1. `ibm` from ascii to alternate ebcdic

	1. `block unblock lcase nocreat excl notrunc ucase swab noerror sync fdatasync fsync`

1. `count` copy only input blocks

1. `if` read from file instead of stdin

1. `of` write to file instead of stdout

### demo

1. [from computerhope](http://www.computerhope.com/unix/dd.htm)

	1. create a iso disc image from cd

			$ dd if=/dev/sr0 of=/home/hope/example.iso bs=2048 conv=noerror,sync

	1. create an img file of `/dev/sda` hard drive

			$ dd if=/dev/sda of=~/disk1.img

			# restore that image
			$ dd if=disk1.img of/dev/sda

	1. copy contents

			$ dd if=/dev/sda of=/dev/sdb

1. [from tuxgarage](http://www.tuxgarage.com/2011/06/ubuntu-switched-to-hybrid-disc-images.html)

	1. create a startup usb from a hybrid image

			# !!!!!
			# warning: make sure to set the correct device path
			# as this process will delete all data
			# that was on the specified device previously

			# !!!!!
			# don't include an integer for the usb drive
			# e.g. `/dev/sdx1` as it would refer to
			# `the existing partition` on the drive
			# not the drive itself

			$ dd if=~/Desktop/ubuntu-15.04-desktop-amd64.iso of=/dev/sdx oflag=direct =1048576

			# figure out the target usb drive path
			$ fdisk -l

1. [from osxdaily](http://osxdaily.com/2009/09/09/convert-a-dmg-to-iso-and-convert-iso-to-dmg-on-mac/)

	1. convert a `dmg` file to `iso`

			$ hdiutil convert imagefile.img -format UDTO -o converted.iso

	1. convert an `iso` to `dmg`

			$ hdiutil convert converted.iso -format UDRW -o imagefile.img