---
title: Running uTorrent as a Service on Windows Home Server 2011
date: 2011-06-20
layout: post
comments: true
categories:
- Tips &amp; Tricks
tags: []
---

## Step 1: Download and Install uTorrent
	
* [Download uTorrent 2.2.1](http://www.utorrent.com/downloads) (390 KB)
* Install uTorrent as normal, by opening the downloaded EXE
* It should install itself into `C:\Program Files (x86)\uTorrent`

[ad#Google Adsense #1]

## Step 2: Configure uTorrent Web UI
	
* Run uTorrent as normal (e.g. Start Menu)
* Go to Options menu and select Preferences
* Go to the Directories folder and set the folders where you want your torrents to be stored
    * Make a note of these directories, as you will need them in step 8

![](https://s3-us-west-2.amazonaws.com/jack-ukleja-com/image49.png)
	
* Go to the Web UI section and make three changes
    * Tick “Enable Web UI”
    * Set a username and password
    * Tick “Alternative Listening Port”; leave the number as 8080
        * Note: This sub-step is not strictly necessary but gives you a higher chance of things working first time

![](https://s3-us-west-2.amazonaws.com/jack-ukleja-com/image50.png)

## Step 3: Test uTorrent Web UI
	
* Using a different computer on the local network ensure you can get to the uTorrent Web UI
* The web address should of the form [http://homeserver:8080/gui/](http://homeserver:8080/gui/)
* Note: You **must** replace homeserver in the above URL with the name of the computer where you install uTorrent.
* The browser should ask for a username and password. Use the values provided in Step 2
* You should see the uTorrent Web UI.
* <span style="color: #ff0000;"><strong>If you do not see the uTorrent Web UI do not proceed any further. You have a networking problem. Seek help.</strong> </span>
* <span style="color: #000000;"><strong>At this stage you have a working uTorrent Web UI but it is</strong> <strong><span style="text-decoration: underline;">NOT </span></strong><strong><span style="text-decoration: underline;">running as a service</span>. This means that torrents can still only be downloaded when a user is logged on and running uTorrent.</strong> <strong>Continue with the following steps to run it as a service…</strong></span>
	
* **Important: Now Close down uTorrent by choosing File-&gt;Exit.**

[ad#Google Adsense #1]

## Step 4: Copy the uTorrent settings file
	
* Copy the settings.dat file from `C:\Users\<User>\AppData\Roaming\uTorrent` to `C:\Program Files (x86)\uTorrent`

**Note: Depending on your username you will need to substitute <User> in the above path**

## Step 5: Download and Install SRVANY.exe
	
* Download [Windows Server 2003 Resource Kit Tools](http://www.microsoft.com/downloads/en/details.aspx?familyid=9d467a69-57ff-4ae7-96ee-b18c4790cffd) (11.8 MB).
* Install the entire tool suite as normal, by opening the downloaded exe.
* By default srvany.exe will be installed to `C:\Program Files (x86)\Windows Resource Kits\Tools\srvany.exe`

## Step 6: Create Windows Service for uTorrent
	
* Open an <span style="text-decoration: underline;"><strong>administrative</strong></span> command prompt by following these steps:
  * Navigate to Start Menu –> Accessories –> Command Prompt
  * Right click and select "Run as Administrator"
* At the command prompt enter the following:

```dos
sc create uTorrent binPath= "C:\Program Files (x86)\Windows Resource Kits\Tools\srvany.exe" obj= "NT AUTHORITY\LocalService" start= auto
```

* **Note: Do not remove the spaces between the equals sign and parameters.** This is an oddity of sc command and is required
* If this works you should see something like the screenshot below:

![](https://s3-us-west-2.amazonaws.com/jack-ukleja-com/image311.png)

**If you get the “Access is denied” error then you are not running as administrator.**

## Step 7: Configure the SRVANY.EXE service
	
* If you want to perform this step manually you will need to know about regedit. Here are the steps:
* Run regedit
    * Navigate to `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\uTorrent\Parameters`
    * Create the Parameters key if it does not exist
    * Under the Parameters Key, add a new String Value named Application
    * Set the value to be `C:\Program Files (x86)\uTorrent\uTorrent.exe`
    * If you have done everything correctly it should look like the screenshot below:

![](https://s3-us-west-2.amazonaws.com/jack-ukleja-com/image51.png)

[ad#Google Adsense #1]

## Step 8: Modify permissions on your downloads directory

By default Windows will not allow your uTorrent service to read or write to the disk. In order for uTorrent to work you need to add read &amp; write permissions on the directory or directories you **specified in Step 2**.
	
* Navigate to the directory
* View the directory properties
* Select the Security tab
* Click the Edit… button
* Click the Add… button
* Enter `LOCAL SERVICE` (including the space) then click OK
* Ensure "Allow Modify" is ticked, and OK everything
* It should look something like the screeshot below:

![](https://s3-us-west-2.amazonaws.com/jack-ukleja-com/image52.png)

## Step 9: Ensure the uTorrent service is running
	
* Load the services control panel (Start->Run->Services.msc)
* Locate the service named uTorrent and verify the Status column says "Started".
* If it does not say "Started", then click the Start Service button

## Finally…
	
* Log off your Windows session and repeat **Step 3** to verify that the web UI is still working and you can download torrents successfully.

## General trouble shooting:
	
* Double check that uTorrent.exe is not running on your desktop.
    * Note: By default clicking the close button in top right of the uTorrent window will **not** actually exit the application
* Double check that uTorrent.exe is running as a service.
    * In task manager, “Show processes from all users”, ensure you can see uTorrent.exe running with LOCAL SERVICE displaying in the User Name column
* Please leave a comment if you have any issues with this guide

[ad#Google Adsense #1]