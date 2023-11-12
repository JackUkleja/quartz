---
title: How to get Ericsson F3507g Mobile Broadband card working on Windows 7 & Lenovo Thinkpad T400s
date: 2009-08-28
layout: post
categories:
- Troubleshooting
tags:
- ericsson
- lenovo
comments: true
aliases:
    - ../how-to-get-ericsson-f3507g-mobile-broadband-card-working-on-windows-7-lenovo-thinkpad-t400s/
---

<span style="color: #ff0000;"><strong>Update: A new driver has been released which uses the Windows 7 Mobile Broadband driver framework. Please <a href="http://jack.ukleja.com/update-ericsson-f3507g-mobile-broadband-on-windows-7-with-lenovo-thinkpad/">see my new article</a>. Note: Information and comments on this article could still be relevant to you so read on...</strong></span>

I recently purchased a [Lenovo Thinkpad T400s](http://shop.lenovo.com/us/notebooks/thinkpad/t-series/t400s). I installed Windows 7 on it soon after, having heard that almost all hardware devices worked under Windows 7. For me it was crucial that the Ericsson F3507G internal mobile broadband card worked, and luckily there was a driver for it available on [Lenovo’s Windows 7 “Beta” drivers](http://www-307.ibm.com/pc/support/site.wss/document.do?sitestyle=lenovo&amp;lndocid=WIN7-BETA#f3507g)…

<img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="God knows what a &quot;Drever&quot; is!" src="https://s3-us-west-2.amazonaws.com/jack-ukleja-com/image20.png" border="0" alt="God knows what a &quot;Drever&quot; is!" width="456" height="265">

First thing to note is that even though it says “32-bit”, inside the zip there are actually 64-bit drivers as well. Phew! Unfortunately it seems that versions and publish dates are not supported by Lenovo’s knowledge base, so all we get is a crazy looking file name – 7uwc46ww.zip.

After you install this driver you will have several new devices available in Device Manager:


	
* Ericsson F3507g Mobile Broadband Minicard Network Adapter
	
* Ericsson F3507g Mobile Broadband Minicard Modem
	
* Ericsson F3507g Mobile Broadband Minicard Device Management (COM3)
	
* Ericsson F3507g Mobile Broadband Minicard GPS Port (COM6)
	
* Ericsson F3507g Mobile Broadband Minicard PC SC Port
	
* Ericsson F3507g Mobile Broadband Minicard Composite Device


[ad#Google Adsense #1]

I am not a big fan of using OEM software over what comes with Windows, as it’s invariably crap. Lenovos’s network manager tool is called [Access Connections](http://lenovoblogs.com/insidethebox/?p=270) and I would later discover my intuition would prove  to be well founded.

So at this point I decided to try using the device as a dial up modem, by using “Create a Dial-up Connection”, then picking one of the two modems that show up (never worked out what the difference was).

<img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="Create a Dial-up Connection dialog" src="https://s3-us-west-2.amazonaws.com/jack-ukleja-com/image21.png" border="0" alt="Create a Dial-up Connection dialog" width="632" height="464">

The problem is it will then ask you for a Dial-up phone number, and various other fields wholly inappropriate for a 3G WWAN card. After a bit of googling I worked out that you should use *99***1# in the phone number field (*99# may also “work”), and leave the user name and password fields blank.

<img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="Create a Dial-up Connection details" src="https://s3-us-west-2.amazonaws.com/jack-ukleja-com/image22.png" border="0" alt="Create a Dial-up Connection details" width="632" height="464">

The problem was after connecting I would get “Error 734: The PPP link control protocol was terminated”

<img style="border: 0pt none; display: inline;" title="Error 734: The PPP Link control protocol was terminated" src="https://s3-us-west-2.amazonaws.com/jack-ukleja-com/image_thumb13.png" border="0" alt="Error 734: The PPP Link control protocol was terminated" width="443" height="438">

Now there was a setting I definitely knew I needed to put in somewhere, because all 3G modems require an APN… but there is no self-evident place to put this setting (remember basically we are trying to use a piece of 2009 technology with a modem interface designed probably 30 or 40 years ago!). After a bit of hunting around it turns out that you can put this setting in the “Extra Initialization commands”. Not the easiest thing to find but its located in Phone and Modem –&gt; Properties –&gt;Advanced dialog.

The string to set the APN is some variant of


    AT+CGDCONT=1,”IP”,”dodolns1”

where dodolns1 is your broadband provider’s APN.

Even after figuring this out I would still get the Error 734 message! Bah!

## Lenovo Access Connections = No Good

At this point I gave up and decided to try using the Beta version of Access Connections. I downloaded the “[6jc718us.zip](http://www-307.ibm.com/pc/support/site.wss/license.do?filename=thinkvantage_en/6jc718us.zip)” version. I installed it and it worked.

Kind of.

Now I am all for OEM’s giving early access to software, but unfortunately Access Connections is no where near “Beta” standard. “Beta” in this case just seems to just be a label for lazy and poorly tested code. There are at least 3 huge bugs with Access Connections for Windows 7:


	
* A memory leak that causes explorer.exe to consume 900MB of RAM every 20 hours or so. At which point explorer would stop working, and I would have to kill and restart it. For a bug as huge as that to be released into the public, no matter what label you give it, is a sign of a rotten development process.
	
* It constantly forgets your customised APN settings. So each time I wanted to connect I would have to delve into the settings and enter them again.
	
* The WWAN connection it created for me was generally unreliably and would drop IP connectivity every 5-30 minutes (the wireless/PHY link would be fine, just the IP stack would stop working).
	
* It could, and often would, take ages to connect.
	
* Performance was pretty sluggish – it would take ages to load the main interface. All I want is for my broadband card to work!


I lived with this situation for a few weeks, but today I couldn’t handle it any more. I uninstalled Access Connections and will probably never use it again. As I said OEM software is invariably crap and this “Beta” was no exception.

[ad#Google Adsense #1]


## The Solution – Sony Ericsson Wireless Manager

I have to credit some [posters on Thinkpads Forum](http://forum.thinkpads.com/viewtopic.php?f=18&amp;t=74021) for the final, and only, solution I was happy with.

Basically you just have to download a tentatively related piece of software from Sony Ericsson, called “Wireless Manager” which they claim is for an apparently unrelated card, the EC400g.

[http://www.sonyericsson.com/cws/support/mobilebroadband/download/wirelessmanager/ec400?lc=en&amp;cc=gb](http://www.sonyericsson.com/cws/support/mobilebroadband/download/wirelessmanager/ec400?lc=en&amp;cc=gb)
[http://www.sonyericsson.com/cws/support/mobilebroadband/download/wirelessmanager/ec400g?lc=en&amp;cc=gb](http://www.sonyericsson.com/cws/support/mobilebroadband/download/wirelessmanager/ec400g?lc=en&amp;cc=gb)

Both links get you to the same software. Nowhere does it mention that the F3507g card is supported, but I can confirm **it does work** using version 5.2.2056.12.

The filename I downloaded was **WirelessManager5.2.2056.12.zip**.

After downloading and unzipping it, you just need to run “Sony Ericsson Wireless Manager 5.msi” which can be found inside the Install folder:

<img style="border-width: 0px; display: inline;" title="Showing the Wireless Manager installer" src="https://s3-us-west-2.amazonaws.com/jack-ukleja-com/image23.png" border="0" alt="Showing the Wireless Manager installer" width="808" height="437">

After that completes it will say something about “to install drivers, please click next”. Suffice to say that it just stops at this point and no drivers get installed. When you load it up you will be presented with this:

<img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="Wireless Manager UI, with setting button highlighted" src="https://s3-us-west-2.amazonaws.com/jack-ukleja-com/image24.png" border="0" alt="Wireless Manager UI, with setting button highlighted" width="420" height="260">

The first thing to do is click the spanner to setup your APN. (You may not have to do this – it depends on whether your provider is known in the standard list).

<img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="Creating a new profile in Wireless Manager" src="https://s3-us-west-2.amazonaws.com/jack-ukleja-com/image25.png" border="0" alt="Creating a new profile in Wireless Manager" width="764" height="490">

For my provider, [Dodo](http://www.dodo.com.au/), I had to untick the “Let Wireless Manager Choose the Connection Profile” and then I created a new profile, and put the Access Point Name (APN) in as shown above.

You then click “Enable Radio” and wait while it detects the mobile networks (hopefully including yours). If that is succesful you just press the big “Connect” button and you are online!

<img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="Wireless Manager connecting" src="https://s3-us-west-2.amazonaws.com/jack-ukleja-com/image26.png" border="0" alt="Wireless Manager connecting" width="420" height="260">

<img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="Wireless manager connected" src="https://s3-us-west-2.amazonaws.com/jack-ukleja-com/image27.png" border="0" alt="Wireless manager connected" width="420" height="260">

One great feature about this is that once the radio is enabled you have two ways of getting online. You can either use:


	
* The Wireless Manager connect button which activates the Ericsson F3507g Mobile Broadband Network Adapter (which means you don’t have to mess about with any dial-up stuff)
	
* Or you can use the RAS/Dial-up connection that Wireless Manager creates (It created one for me called WirelessManagerRasConnection).


<img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="Network Connections showing Ericsson F3507g Mobile Broadband Network Adapter" src="https://s3-us-west-2.amazonaws.com/jack-ukleja-com/image28.png" border="0" alt="Network Connections showing Ericsson F3507g Mobile Broadband Network Adapter" width="559" height="338">

What is strange is that you can have both of them connected at the same time. I have yet to work out if that has a detrimental effect :-)

One final tip: If you look inside the “Show hidden icons” pane in the Windows taskbar, you will see the Wireless Manager. If you right click it and select “Hide to notification area” it will then remove the app from the task bar. Even better is if you then go into the “Customize” menu and change the behaviour to “Show icon and notifications” you will get this neat and handy icon for quick access to mobile broadband:

<img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="Wireless Manager showing in icon tray" src="https://s3-us-west-2.amazonaws.com/jack-ukleja-com/image29.png" border="0" alt="Wireless Manager showing in icon tray" width="237" height="195">

Anyway, let me know how you go, and if I missed anything in my instructions.

[ad#Google Adsense #1]
