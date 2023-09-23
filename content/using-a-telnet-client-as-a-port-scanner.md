---
title: Using a telnet client as a port scanner
date: 2009-08-22
layout: post
categories:
- Tips &amp; Tricks
tags: []
comments: true
---

If you ever need to check if a TCP port is open or reachable, one easy way to do it is using a telnet client.

A telnet client actually comes with Windows, but for some annoying reason it is not installed by default. To install the windows telnet client you need to add it using the “Turn Windows features on and off” like so:

![](https://s3-us-west-2.amazonaws.com/jack-ukleja-com/image1.png)

Because Telnet is essentially just characters sent over a TCP connection, you can use the telnet client as a poor man’s TCP port scanner. To manually check if TCP port 80 is open or reachable on a server you can type **telnet myserver.com 80**.

As an example lets see if Google.com has port 80 (HTTP) open … ;-)

![](https://s3-us-west-2.amazonaws.com/jack-ukleja-com/image2.png)

![](https://s3-us-west-2.amazonaws.com/jack-ukleja-com/image3.png)

If you get a blank screen with a blinking cursor you have successfully connected to that TCP port.

In this example, if you are feeling particularly sadistic you can now engage in a HTTP conversation. Type **GET index.html HTTP/1.0** and press enter and you will see Google’s home page being sent to you. Same idea would apply if you connected to most other TCP-based protocols; POP mail server etc.

![](https://s3-us-west-2.amazonaws.com/jack-ukleja-com/image8.png)

On the other hand if the TCP port is not open or not reachable you will see this:

![](https://s3-us-west-2.amazonaws.com/jack-ukleja-com/image4.png)

Troubleshooting ports and firewalls can be a pain. This is the simplest and quickest way I know for checking connectivity.