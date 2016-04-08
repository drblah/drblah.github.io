---
layout: post
title:  "How to setup SDRsharp in WINE"
date:   2016-04-08 20:00:39 +0200
categories: RTL-SDR radio
---

**Reader beware! This post was written in 2012 when SDR# was still free. A lot may have changed since then so this guide might not work anymore.**


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

![winecfg]({{ site.url }}/assets/How-to-setup-SDRsharp-in-WINE/winecfg.png)

Step 2. Install SDRsharp
------------------------

The next thing we need to do is to install SDRsharp. The easiest way to do this is to get the automated installer from the official website.

Go to <http://sdrsharp.com/index.php/downloads> and click the link for sdr-install.zip. This zip contains a script that will download everything needed for SDRsharp to work.

Exctract the zip file and cd to the extracted folder.

```bash
$ unzip sdr-install.zip
```

```bash
$ cd sdr-install
```

Now we need wine to execute the install script since it is written in batch.

```bash
$ wineconsole install.bat
```

You should see a Windows cmd pop up. If it asks you to overwrite some files choose ‘yes’.

![WINE console]({{ site.url }}/assets/How-to-setup-SDRsharp-in-WINE/wineconsole.png)

The script has now created a new folder called sdrsharp. The folder should contain a bunch of dlls and the SDRsharp executable.

Step 3. Install Windows dependencies
------------------------------------

By default, Wine will use mono to run .net applications. Since that did not work, we will have to install a .net framework instead. SDRsharp requires .net 3.5 sp1. The easiest way to install .net is with winetricks:

```bash
$ winetricks dotnet35sp1
```

I ran into some problems during the installation. Winetricks ran thorugh .net 2, 2 sp1 and 3.5 but failed on 3.5 sp1, so we will have to install it manually.

NOTE: Installing .net 3.5 sp1 on its own does not work. Winetricks does some black magic that makes .net work so make sure you run the above command BEFORE manually installing .net 3.5 sp1.

Go to <http://www.microsoft.com/en-us/download/details.aspx?id=21> and hit 'Download’. It might recommend you to download .net 4 but just ignore it becuse .net 4 does not work that well in Wine.

When it is downloaded, go to your download directory and run the installer in Wine.

```bash
$ wine dotNetFx35setup.exe
```

The installation will download a couple of hundred MB so it might take a while.

The last thing is to tell Wine to use the native gdiplus.dll. This will make usre the waterfall works.

```bash
$ winecfg
```

Click the Libraries tab and find gdiplus in the drop-down menu. Click Add, select it and click Edit. Now select Native (Windows) and hit Ok. Then close Wine configuration.

![Overriding dlls]({{ site.url }}/assets/How-to-setup-SDRsharp-in-WINE/override.png)

That is it. We are done.

Now go to the folder containing SDRsharp and try and run it.

```bash
$ wine SDRSharp.exe
```

We now have a working SDRsharp environment! The only thing we need for this to be perfect is direct USB access but it does not work for some reason. Until someone finds a solution we will have to use rtl_tcp to communicate with the dongle.

![SDR# running in WINE]({{ site.url }}/assets/How-to-setup-SDRsharp-in-WINE/sdrinwine.png)

You might have noticed that USB access was not enabled by the installation script. If you want to try it out, although it does not really work, you can enable it in SDRSharp.exe.config. Find the line containing: “add key=“RTL-SDR / USB"” and uncomment the line. USB functionality should now show up in SDRsharp.
