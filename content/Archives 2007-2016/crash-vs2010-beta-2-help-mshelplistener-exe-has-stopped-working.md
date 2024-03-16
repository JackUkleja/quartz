---
title: 'Crash when trying to open VS2010 Beta 2 Help: MSHelpListener.exe has stopped working'
date: 2009-10-22
layout: post
categories:
- Troubleshooting
tags:
- vs2010
comments: true
aliases:
    - ../crash-vs2010-beta-2-help-mshelplistener-exe-has-stopped-working/
---

**Update: There is a Microsoft Connect bug logged for this here**.
<span style="color: #ff0000;"><strong>Update 2: "R0b0tz" has got the workaround for this on his <a href="http://r0b0tz.com/?p=35">blog</a>. As always it pays to RTFM :)</strong></span><span style="color: #ff0000;"><strong> It's still definitely a bug though...</strong></span>

One big change in Visual Studio 2010 is Microsoft Help version 3. It’s all based on HTML, and when you install the help locally it does a smart trick and installs its own web server (MSHelpListener.exe), allowing the pages to be accessed via the url [http://127.0.0.1:80/help](http://127.0.0.1:80/help).

The problem was the first time I ran the help after installing VS2010 I was getting the following crash…

<img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="MSHelpListener.exe has stopped working dialog" src="https://s3-us-west-2.amazonaws.com/jack-ukleja-com/image42.png" border="0" alt="MSHelpListener.exe has stopped working dialog" width="536" height="290">

If you check the Windows Application event logs you will see the following error:

**The port is already in use please select a different port**

**** For some reason, MSHelpListener is unable to bind to port 80 (even though I believe HTTP.sys does allow multiple applications to bind to the same port).

If you go to the command line and run

    netsh http show urlacl
You can see that it has also reserved the following url, although it appears this does not help at all in guaranteeing access to the port…

<img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="netsh showing urlacl reservation for MS Help 3" src="https://s3-us-west-2.amazonaws.com/jack-ukleja-com/image43.png" border="0" alt="netsh showing urlacl reservation for MS Help 3" width="681" height="166">

I had a quick look in Resource Monitor and this is what I saw:

<img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="Resource Monitor showing Skype listening on port 80" src="https://s3-us-west-2.amazonaws.com/jack-ukleja-com/image44.png" border="0" alt="Resource Monitor showing Skype listening on port 80" width="719" height="501">

You can see Skype was listening on port 80.

**It turns out the solution was as simple as quitting Skype!**

After I quit Skype I was able to load up the help…

![img](https://s3-us-west-2.amazonaws.com/jack-ukleja-com/image45.png)

Once HelpListener.exe is running it is safe to start Skype up again. I have yet to work out why Skype is using port 80, or what circumstances cause one process to block another’s use of a port, given the fact HTTP.sys is supposed to let multiple process share the same port. If you know please leave a comment.

If you find yourself having problems getting the local help working after installing it using “Find content on disk”, the trick that worked for me is to switch to online help and then back to local help.

![img](https://s3-us-west-2.amazonaws.com/jack-ukleja-com/image46.png)

<img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="Help Library Manager screen in VS2010 Beta 2" src="https://s3-us-west-2.amazonaws.com/jack-ukleja-com/image46.png" border="0" alt="Help Library Manager screen in VS2010 Beta 2" width="575" height="392">

<img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="MS Help 3 Find Content Online" src="https://s3-us-west-2.amazonaws.com/jack-ukleja-com/image47.png" border="0" alt="MS Help 3 Find Content Online" width="575" height="392">

Overall, MS Help v3 looks very nice and works as we probably wished it always had! What is very cool is you can “Find” content either online, or from your installation media. Best of all **it just works** – getting content from the web is straight forward, and so is installing it from media. Either way the help files can be kept up-to-date easily using “Check for updates online”. Nice