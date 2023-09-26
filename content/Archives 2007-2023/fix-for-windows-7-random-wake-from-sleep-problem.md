---
title: Fix for Windows 7 "random wake from sleep" problem
date: 2009-08-07
layout: post
categories:
- Troubleshooting
tags: []
comments: true
---

For the last few months I've had Windows 7 RC installed on two home computers, and both of them have exhibited a strange problem: They would randomly wake from sleep .

The computers would often wake up just seconds after being shut down. At other times they would wake up in the middle of the night. To make matters worse one PC, which had recently had its graphics card upgraded, would go into an endless loop of restarting without ever getting to the Windows login screen. I suspected a power problem relating to the new card.

I spent quite some time investigating these wake problems (which I believed were unrelated) but could not diagnose the issue. Eventually I gave up and had to disable sleep on both machines (which pained me greatly!).

[ad#Google Adsense #1]

Last week I had a second wind and was determined to resolve this problem once and for all. I figured there must be a tool that could tell me what caused the wake up events, and after some research I discovered the Vista/Windows 7 [powercfg](http://technet.microsoft.com/en-us/library/cc748940(WS.10).aspx) tool.

To use this tool you simply type **powercfg –lastwake** at the command prompt

<img style="display: inline; border: 0px initial initial;" title="powercfg with lastwake command" src="https://s3-us-west-2.amazonaws.com/jack-ukleja-com/powercfg1.png" border="0" alt="powercfg" width="681" height="226">

In the above screenshot you can see the computer was woken by something on the USB hub (my mouse in this case). But much to my annoyance, when I ran this tool after a phantom wake it reported the type was “Unknown”!

Because I had a suspicion this could be a wake-on-lan (WOL) behaviour I used the [WireShark](http://www.wireshark.org/) network analysis tool to monitor network traffic for WOL packets. Perhaps a third computer had been infected with malware and was trying to wake other computers to have its wicked way? Unfortunately nothing showed up on the wire.

No further progress was made until I randomly stumbled across a forum posting about a network adaptor setting called "**Wake on pattern match**". I checked both my PCs and this setting <span style="text-decoration: underline;">was</span> enabled. It appears this obscure setting is **enabled by default in Windows 7** for Realtek RTL8168B/8111B and RTL8168C(P)/8111C(P) Family Gigabit Ethernet NICs**.**

What does "Wake on pattern match" actually do?

[ad#Google Adsense #1]

Most people are familiar with Wake-on-LAN which uses a "magic packet" (hard-coded) to trigger a wake event. Well it seems that feature has been superseded by the more flexible "Wake on pattern match" capability. According to this [Microsoft Article on Power Management for Network Devices](http://www.microsoft.com/whdc/archive/netpm.mspx):
<blockquote>The packet patterns that define the wake-up frames [for "wake on pattern match"] are provided to the NDIS 5.0 miniport driver by the operating system. At runtime, the protocol sets the wake-up policy using OIDs. These are, for example, <em>Enable Wakeup</em><em>Set Packet Pattern</em><em>Remove Packet Pattern</em>. Currently, the Microsoft TCP/IP is the only Microsoft protocol stack that supports network power management. <strong>It will register the following packet patterns at miniport initialization</strong>:
<ul>
	<li>Directed Layer Two packet</li>
	<li>Address resolution protocol (ARP) broadcast for station IP address (frames with DIX header)</li>
	<li>NetBIOS over TCP/IP broadcast for station's assigned <em>computername</em> (frames with DIX header)</li>
</ul>
</blockquote>
I am no networking expert but the last two sound like they could be pretty common activities on your typical home network. If so it's no wonder my computers were exhibiting such crazy wakeup symptoms. (Either that or I have got a renegade PC on my networking sending out these packets).

The solution? Simply disable the "Wake on pattern match" setting under the network adaptors advanced properties tab, as shown below:

[](https://s3-us-west-2.amazonaws.com/jack-ukleja-com/realtek_wake_on_pattern_match.png)

My biggest mistake? I could have short circuited the resolution of this problem by testing my computers without the Ethernet cable plugged in right at the start. The reason I didn't was I couldn't understand how a WOL packet could possibly be being sent so I ignored the possibility.

Now I know about "Wake on pattern match", next time I will be prepared.

[ad#Google Adsense #1]