---
title: Microsoft Expression Web 3 UI Built With WPF; Expression Studio Moves Forward
date: 2009-09-10
layout: post
categories:
- Uncategorized
tags: []
comments: true
aliases:
    - ../microsoft-expression-web-3-ui-built-with-wpf-expression-studio-moves-forward/
---


A few months ago [I asked a question on Somasegar’s blog](http://blogs.msdn.com/somasegar/archive/2009/06/05/expression-web-3.aspx) about the new Expression Web 3, specifically whether the UI would be getting updated to match the rest of the Expression suite.
  
Having downloaded Expression Web 3 this afternoon I can safely say one thing: **Expression Web 3 definitely uses WPF**. The screenshot below was taken using [Snoop](http://blois.us/Snoop/), a utility that lets you do 3D exploded views, and shows all the layers in the Expression Web UI.
  
<img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="Expression Web is built with WPF" border="0" alt="Expression Web is built with WPF" src="https://s3-us-west-2.amazonaws.com/jack-ukleja-com/expression_web_3_exploded2_thumb.jpg" width="644" height="463">
  
This does not confirm whether Expression Web is using the same UI framework as Expression Blend, in the sense that a similar look and feel *could* be achieved independently, i.e. without the two teams working from a shared UI code base. It’s hard to pick out any UI elements that are definitively shared with Blend, except perhaps the tool windows. Without delving into the code, it’s my guess we are seeing the first steps of merging the UIs (perhaps some shared styles) but we do not have a unified code base yet. Another thing which is obvious: the finesse of Blend is certainly not yet present in Web – just witness the number of retro-fitted Win32-esque dialogs:
  
&#160;<img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="One of many fugly dialogs. Yes this is real!" border="0" alt="One of many fugly dialogs. Yes this is real!" src="https://s3-us-west-2.amazonaws.com/jack-ukleja-com/image31.png" width="477" height="384"> 
  
## So what?
  
To explain why I am so interested in Expression Web’s UI we have to delve into the background of Expression…
  
It’s no secret that Expression Studio was never written from the ground up as suite of closely integrated tools. It was basically pulled together out of thin air when some marketing guy at Microsoft noticed they had a collection of upcoming software packages that could fall under a single “creative” banner. That banner was Expression.
  
OK, that’s the cynical explanation. :) Microsoft had a very good strategic reason to create a new design suite...
  
Microsoft had early realised the importance of having a large developer community. In fact today, the software ecosystem built around Windows is pretty much the cornerstone of Microsoft’s business. 
  
<img style="border-bottom: 0px; border-left: 0px; display: inline; margin-left: 0px; border-top: 0px; margin-right: 0px; border-right: 0px" title="Expression Studio 3 box" border="0" alt="Expression Studio 3 box" align="left" src="https://s3-us-west-2.amazonaws.com/jack-ukleja-com/US_Prd_Bx_Tilt_L_Expression_Studio_3_thumb.png" width="202" height="239"> 
  
But as time passed expectations of how software should look &amp; feel began to sky rocket. With software, as with most things in life, **sex sells** – just look at Apple’s resurgence for proof of that. Software had finally become less about features, and more about usability, enjoyment and style (users want to experience “Delight” as [Shane Morris](http://blogs.msdn.com/shanemo/) likes to put it).
  
The problem is, the people with the skills to improve UX – well, basically they live and breath Adobe. While Microsoft owns massive developer mindshare it owns very little designer mindshare – **both** ingredients are required to build compelling applications these days. Microsoft needs to capture designer mindshare and get them comfortable using Expression suite.
  
## Practice what you preach
  
The problem is: if your design tool looks/feels like a turd (e.g. Web 2), why is any designer going to take you seriously? And if a designer doesn’t take you seriously how will you gain designer mindshare? And without designer mindshare how can Microsoft win the UX battle?
  
**They can’t.** That’s why when you looked at previous versions of Expression suite there was definitely a bad smell left by the inconsistent UIs. This may be subtle to some people but it’s what UX is all about – you might have a a good software product, but if your supposed suite of applications clearly don’t share the same UI framework (or dialogs boxes look like they were lifted straight out of Windows 95!) you are going to be judged and punished by the market.
  
That’s why its good to see Expression Web move to WPF – it was the biggest eye sore in the suite. I should add that I am a very big fan of majority of Expression, in particular Blend’s UI. It’s just that when you are selling a tool for designers/creative's you better make sure you practice what you preach because you will be judged *very harshly*. Considering the pressure that is on the Expression teams (again a big shout out to the amazing job done by [Unni](http://blogs.msdn.com/unnir/), [Lutz Roeder](http://blog.lutzroeder.com/) etc on Blend) and the standards they have to live up to they have done reasonably well. Unfortunately in this market segment they simply cannot rest :)
  
## Step in the right direction
  
For end-users, the eventual unification of Expression Studio into a single, compelling UI framework will do wonders for the Expression UX, increasing user productivity and hopefully growing Microsoft’s designer mindshare. For Microsoft it will allow them to develop the Expression suite more efficiently, meaning higher quality, more frequently released applications. It will also continue to demonstrate that Microsoft are serious about using their own technologies and that WPF is a viable.
  
As a random aside, I am hopeful that one day Microsoft decide to take Expression a step further, growing the suite to include a Photoshop- or Fireworks-like tool, and thus create some genuine competition in the moribund designer tool market; perhaps even break Adobes dominant position it has abused for so long (see prices they charge!). Whether you like Microsoft or not there is no doubt that competition in the creative suite market benefits us all.
