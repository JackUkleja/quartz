---
title: How to make a bootable Vista DVD using MKISOFS/CDRTOOLS
date: 2007-02-01
layout: post
categories:
- Tips &amp; Tricks
tags:
- vista
comments: true
---


*UPDATE: This is a repost of an old article that is still referenced on some forums.*
  
I managed to get hold of an MSDN copy of Vista on DVD. Unfortunately the DVD was corrupt and several of the sectors were unreadable. I managed to salvage all the files from the disc apart from a couple named license.rtf (no big loss there then!). The question was, how do I then make another bootable DVD from these salvaged files? No big deal you say - just create a bootable ISO like you did with Windows XP: [extract boot image](http://www.nu2.nu/bbie/) then use that to burn the files to a bootable DVD with Nero.
  
Unfortunately this wouldn't work because one of the files on the Vista DVD is &gt; 2GB (install.wim) and the most common ISO modes (1 and 2) don't support files that big. Microsoft have a solution of course: There is a utility on the [Windows Automated Installation Kit (WAIK)](http://www.google.com.au/search?q=waik) called [Oscdimg](http://technet.microsoft.com/en-us/library/cc749036%28WS.10%29.aspx)**** which is designed to make ISO images from Vista trees. Perfect, the download is [here](http://download.microsoft.com/download/8/6/d/86d6ba9c-98ff-444e-87ed-3e76772eb2a6/vista_6000.16386.061101-2205-LRMAIK_EN.img). Unfortunately its 900MB and I'll be damned if im gonna download all that just for a 50k utility! So I decided to try using makeisofs (from the [cdrtools](http://cdrecord.berlios.de/private/cdrecord.html) package. N.B. for windows you will need to download a [windows binary](http://smithii.com/cdrtools)). After 20 minutes of fiddling around I got the following to work: 
  
    mkisofs -udf -v -b boot.bin -no-emul-boot -hide boot.bin -hide boot.catalog -o MyVistaIso.iso Vista
Where *boot.bin* is the name of the boot sector extracted from the orignal corrupt DVD, and *Vista* is the name of the directory containing the salvaged contents of the original DVD. Note: The *boot.bin* file must be within the *Vista* directory for mkisofs to find it. Anyway, I burned the iso to a DVD, restarted my PC, and it booted into the Vista installer. I proceeded to install Vista and everything worked fine. Yay! 

