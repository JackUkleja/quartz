---
title: 'Windows 7 ISO: How to easily switch edition'
date: 2009-08-30
layout: post
categories:
- Tips &amp; Tricks
tags: []
comments: true
aliases:
    - ../windows-7-iso-how-to-easily-switch-edition
---

There have been quite a [few](http://windows7center.com/news/how-to-install-any-version-or-sku-of-windows-7/) [blog](http://www.askvg.com/how-to-choose-desired-windows-7-edition-version-during-setup/) posts about how to manually switch edition of the Windows 7 ISO, for example to change from Ultimate to Home Premium.

What I wanted was a program that automated all the steps. I was going to write one myself, but thankfully someone has already released a tool to do just that. The guy responsible is [Kai Liu](http://www.kailiu.com) and in fact he has two programs:


	
* One to turn your ISO into an “All editions” ISO, just like the RC used to work (**eicfg_removal_utility**)
	
* One to switch ISO editions which results in a “bit perfect” rendition of the original MSDN ISOs, verified against the MSDN hash codes (**windows7_iso_image_edition_switcher**)


You can download them both here: [http://code.kliu.org/misc/win7utils/](http://code.kliu.org/misc/win7utils/)

[](http://code.kliu.org/misc/win7utils/)

Here is the [original neowin post](http://www.neowin.net/forum/index.php?showtopic=809014&amp;st=0&amp;p=591407990&amp;#entry591407990) where he talk about the utilities. For the untrustworthy, both of these utilities come bundled with the source code so you can see exactly what they are doing.

Notes: Both these utilities modify the ISO files in-place (so you don’t need to worry about multiple GB file copies).

One potential draw back is both programs use dialogs to select the ISOs so you won’t be able to automate use of these utilities in a script very easily.

As is often the case it can be hard to find the “original” (trustworthy) executables for utilities like this as they get shared widely, copied, and referenced on random forums. Hence after a bit of research I wanted to do my bit to add Google juice to the genuine “[Windows 7 ISO Edition Patching/Switching](http://code.kliu.org/misc/win7utils/)” article.