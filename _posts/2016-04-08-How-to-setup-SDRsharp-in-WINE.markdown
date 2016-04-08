---
layout: post
title:  "How to setup SDRsharp in WINE"
date:   2016-04-08 20:00:39 +0200
categories: RTL-SDR radio
---
If you are running Linux or OS X, and are playing around with RTL-SDR, you might want to run SDRsharp. The problem is that SDRsharp is programmed in C#, thus needing the .net runtime to work. Some might be able to make it run with mono but I had no such luck. Instead, I decided give Wine a go and it turned out to work beautifully.

I did this on a MacBook running OS X 10.8.5 but it should work the same way on any other OS capable of running Wine.

NOTE: This will NOT make SDRsharp work directly with a USB dongle. You will have to use rtl_tcp, and connect to the dongle with TCP/IP for now.

Step 1. Install Wine
--------------------
The easiest way to install Wine, is through your distribution’s built-in package manger. It will take care of all the dependencies and make it easier to update the installed software in the future.

I used Macports. Also note that I choose to use Wine 1.7 and I have not tested this on 1.6. I would also recommend installing winetricks since we will need it later.

```bash
$ sudo port install wine-devel winetricks
```

On Debian based Linux, you would do something like:

```bash
$ sudo apt-get install wine winetricks
```

When the installation is done, we need to setup the Wine environment.

```bash
$ winecfg
```

This should open the Wine configuration window. Leave everything at the default settings for now and close the window.
![My helpful screenshot]({{ site.url }}/assets/How-to-setup-SDRsharp-in-WINE/winecfg.png)