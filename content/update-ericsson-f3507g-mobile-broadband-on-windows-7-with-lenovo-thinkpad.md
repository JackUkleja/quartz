---
title: 'Update: Ericsson F3507g Mobile Broadband on Windows 7 with Lenovo Thinkpad'
date: 2009-10-14
layout: post
categories:
- Troubleshooting
tags: []
comments: true
---

It’s become clear from [comments on my previous article](http://jack.ukleja.com/how-to-get-ericsson-f3507g-mobile-broadband-card-working-on-windows-7-lenovo-thinkpad-t400s/), that Lenovo have released a new Windows 7 driver for the Ericsson F3507g Mobile Broadband card.

You would never know from glancing at the [downloads page](http://www-307.ibm.com/pc/support/site.wss/WIN7-BETA.html) because it doesn’t list versions numbers or publish dates...<img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="Screenshot of F3507g driver on Lenovo downloads page" src="https://s3-us-west-2.amazonaws.com/jack-ukleja-com/image36.png" border="0" alt="Screenshot of F3507g driver on Lenovo downloads page" width="451" height="243">

The driver is now listed under **Vodafone and AT&amp;T Wireless WAN (HSPA) Driver**. Just to make things extra confusing (and harder to find) they removed the actual card model number from the description. The new filename  is [7uw706ww.zip](http://download.lenovo.com/ibmdl/pub/pc/pccbbs/mobiles/7uw706ww.zip) (direct download link), and comes in at 40MB (10 MB smaller than the last driver). It comes as a Setup.exe, including both 32-bit and 64-bit drivers.

After installing the new driver (no need to uninstall the previous one), you will have the following new devices in device manager:

* F3507g Mobile Broadband Driver (Network Adapters)

* F3507g Mobile Broadband Device Management (COM3)
* F3507g Mobile Broadband GPS Port (COM6)
* F3507g Mobile Broadband Device

The crucial thing about the new driver is the change in version from 4.4.50.6.0 (dated 16/12/2008) to 4.54.7103.1 (dated 26/06/2009). The drivers are also HCL certified by Microsoft. Some devices are dated 29/07/2009 and version 1.0.0.52. Either way, all the drivers are much newer :)

[ad#Google Adsense #1]
[](https://s3-us-west-2.amazonaws.com/jack-ukleja-com/image40.png)

This driver is radically different from the previous one. The previous driver installs your broadband card as an old school modem (with COM ports et al). This is what made it particularly tricky to setup.

The new driver actually gets the broadband card to show up as… (drum roll)... a **Mobile Broadband Connection!**

<img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="Ericsson F3507G showing as Mobile Broadband  in Network Connections" src="https://s3-us-west-2.amazonaws.com/jack-ukleja-com/image37.png" border="0" alt="Ericsson F3507G showing as Mobile Broadband  in Network Connections" width="578" height="271">

The upshot of this is that, **after installing these drivers you don’t need any third party software to get online!**

I am not sure if this device type was available in Vista, but I have certainly never seen it before. It integrates with Windows differently to a dialup modem. One obvious change is that it shows up in the networks list accessed via the system tray like so:

<img style="border: 0pt none; display: inline;" title="Available connections" src="https://s3-us-west-2.amazonaws.com/jack-ukleja-com/image_thumb24.png" border="0" alt="Available connections" width="174" height="244">

You can set it to automatically connect when there is no other connection available, which is quite handy and works quite well.

There is only one trick you may need for **setting the APN**. To do this you need to go to the networks list and right click, show properties. *Note: This is completely different than if you select properties in the Network Connections windows*. You should see the following:

<img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="Mobile Broadband connection properties under Windows 7, showing APN setting" src="https://s3-us-west-2.amazonaws.com/jack-ukleja-com/image39.png" border="0" alt="Mobile Broadband connection properties under Windows 7, showing APN setting" width="393" height="509">

Here you can enter the APN and username/password if required by your ISP.

There are only a couple of negatives I have discovered so far:


	
* Using Wireless Manager it was possible to force the connection to only use HSDPA etc. I can’t find a way to do this with the new setup
	
* There is not as much status information available as when using Wireless Manager. For instance, when I am connected, it ALWAYS seems to display UMTS as the connection type, even though I know I am on HSDPA. This may or may not be a bug.


One *very *exciting feature that I just stumbled upon when researching this article is the Windows 7 Sensor compatible GPS Location driver that gets installed.

<img style="border-bottom: 0px; border-left: 0px; display: inline; border-top: 0px; border-right: 0px" title="F3507g GPS Location Sensor driver for Windows 7!" src="https://s3-us-west-2.amazonaws.com/jack-ukleja-com/image41.png" border="0" alt="F3507g GPS Location Sensor driver for Windows 7!" width="644" height="379">

This new feature for Windows 7 allows applications to access your GPS data through a standard API, for example Google Chrome can use this to position you on Google maps! I have not got this to work so far but I am hoping that’s because I have no GPS signal indoors.

Overall I am happy with the new drivers – it’s certainly a better way to integrate with Windows, and the connection seems to be much more reliable.

[ad#Google Adsense #1]