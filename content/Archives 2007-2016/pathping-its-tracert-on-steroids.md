---
title: 'Tip for pathping '
date: 2009-08-22
layout: post
categories:
- Tips &amp; Tricks
tags: []
comments: true
aliases:
    - ../pathping-its-tracert-on-steroids
---

This lesser known command line utility, introduced on Windows 2000, is something of a hybrid between ping and tracert.

Instead of just determining the path, pathping also sends a barrage of pings to each host along the route. After pathping completes (which can take 5 minutes) it generates statistics for failed pings at each node making it easy to indentify hosts in the route where packet loss is occurring. It also shows averaged round trip times (RTT) to each node, allowing you to identify slow hops, and visualize a packet's journey.

If you are anything like me though, you may never have paid close attention to pathping’s (somewhat confusing) output...

<img style="border: 0pt none; display: inline;" title="Image of pathping results with connecting bars highlighted" src="https://s3-us-west-2.amazonaws.com/jack-ukleja-com/image_thumb7.png" border="0" alt="image" width="644" height="374">

If you look carefully (ok, not that carefully – I’ve circled the section in red) you will see the ASCII art bars “connecting” each host. What’s interesting is pathping shows you both the packet loss* between yourself and the host*, as well as packet loss on the links *between hosts* (the bars). As you can see in the screen shot above its perfectly possible to have a host (telstra310) that is not directly reachable, yet is perfectly able to forward on packets. There is an example in the the [full pathping documentation](http://www.microsoft.com/resources/documentation/windows/xp/all/proddocs/en-us/pathping.mspx?mfr=true) that describes such a result like this:
<blockquote>The routers [snip] are dropping packets addressed to them, but this loss does not affect their ability to forward traffic that is not addressed to them.</blockquote>
One thing I still haven’t figured out is what the value in the “This Node/Link” means in a row that has a “Source to Here” entry. I am assuming it means routed packets that entered the host but never left. Answers on a postcard.