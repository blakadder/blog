---
layout: post
title: "Home Assistant Dashboard on Sonoff NSPanel Pro"
author: blak
categories: [ how-to, home assistant ]
tags: [ ]
image: assets/images/header_nspanelpro-sideload.jpg
toc: true
---

How to sideload apps to NSPanel Pro and use Home Assistant dashboard on its screen.

_**Full disclosure:** This is a pre production unit sent to me free of charge by [Sonoff](https://itead.cc/ref/34/). Some aspects of it might be changed in official release. Shopping links in this article are affiliate links and I earn a small commission when you buy through them_

You can still grab an NSPanel Pro for a presale discount on [Itead store](https://itead.cc/product/sonoff-nspanel-pro-smart-home-control-panel/ref/34/).

After the initial shock and disappointment with the release firmware and a smaller scale repeat of that with the September upgrade I decided to go deeper. The simplest way to do that is with [Android Debug Bridge](https://developer.android.com/studio/command-line/adb).

## Disassembly

Remove the panel from the base. Unscrew the two screws on the back then carefully pry away the back plate starting at this point.

[![Unscrew two screws](/assets/images/nspanelpro-sideload/open1.jpg){: width="50%"}](/assets/images/nspanelpro-sideload/open1.jpg)[![Lift the panel](/assets/images/nspanelpro-sideload/open2.jpg){: width="50%"}](/assets/images/nspanelpro-sideload/open2.jpg)

Now unscrew the tiny screws circled. Remove the tape from the touch panel connector and disconnect the connector to lift up the PCB.

[![Unscrew tiny screws](/assets/images/nspanelpro-sideload/open3.jpg){: width="50%"}](/assets/images/nspanelpro-sideload/open3.jpg)[![Disconnect the touch controller](/assets/images/nspanelpro-sideload/open4.jpg){: width="50%"}](/assets/images/nspanelpro-sideload/open4.jpg)

Connect the data USB cable from your computer to the OTG port.

[![Connect the USB cable](/assets/images/nspanelpro-sideload/open5.jpg)](/assets/images/nspanelpro-sideload/open5.jpg)

## Install ADB and Drivers

Download [ADB drivers](https://developer.android.com/studio/run/win-usb) and install.

When the NSPanel Pro is connected to your computer via USB you should have a new device in Device Manager.

[![Front](/assets/images/nspanelpro-sideload/device_manager.jpg)](/assets/images/nspanelpro-sideload/device_manager.jpg)

Download the [Android platform-tools](https://dl.google.com/android/repository/platform-tools-latest-windows.zip) and unzip the contents to a folder. Open a Command Prompt and navigate to that folder. In my case it is located at `D:\adb`

## Get ADB Access

Run command `adb devices -l`. It will list all the connected devices with extra information.

```dos
D:\adb>adb devices -l
List of devices attached
F061512302021100016    device product:px30_evb model:px30_evb device:px30_evb transport_id:3
```

Run command `adb tcpip 5555` to set the NSPanel Pro to listen for a TCP/IP connection on port 5555

```dos
D:\adb>adb tcpip 5555
restarting in TCP mode port: 5555
```

Now you can connect to the NSPanel Pro wirelessly. If you don't know the IP address run `adb shell ip -o a` to find out.

Try it out while still wired to make sure everything is working.

```dos
D:\adb>
adb connect 10.1.1.144
connected to 10.1.1.144:5555
```

Reassemble the panel and plug it into the base again. Once it boots up, run `adb connect [ip_address]`.

## Using ADB

With ADB access you have a powerful tool at your disposal to take control of the NSPanel Pro. You can use to install and uninstall apps, list running processes, free memory and even gain root access.

Here's a list of [ADB commands](https://technastic.com/adb-commands-list-adb-cheat-sheet/) you can use on your NSPanel Pro.

A useful one is a simulated press of the Home button with `adb shell input keyevent 3`.

You can also easily gain root access with `adb root`.

Push a file to NSPanel Pro, f.e. a new wallpaper:

```shell
D:\adb>adb push wallpaper.jpg /sdcard/Download/wallpaper.jpg
wallpaper.jpg: 1 file pushed, 0 skipped. 18.5 MB/s (52101 bytes in 0.003s)
```

## Install a Launcher

To make working with the NSPanel easier you need to change the default launcher, eWeLink Control Panel, to a more standard one. My pick was [Lawnchair](https://f-droid.org/en/packages/ch.deletescape.lawnchair.plah/) because it's lightweight. Download the APK to the adb folder and install it to the NSPanel Pro using ADB.

```shell
D:\adb>adb install ch.deletescape.lawnchair.plah_2001.apk
Performing Streamed Install
Success
```

```shell
D:\adb>adb shell monkey -p ch.deletescape.lawnchair.plah 1
  bash arg: -p
  bash arg: ch.deletescape.lawnchair.plah
  bash arg: 1
args: [-p, ch.deletescape.lawnchair.plah, 1]
 arg: "-p"
 arg: "ch.deletescape.lawnchair.plah"
 arg: "1"
data="ch.deletescape.lawnchair.plah"
Events injected: 1
## Network stats: elapsed time=32ms (0ms mobile, 0ms wifi, 32ms not connected)
```

Set is as default launcher in *Settings - Apps & notifications - Advanced - Default apps* and set "Home app" to Lawnchair.

![Change default launcher to Lawnchair](/assets/images/nspanelpro-sideload/defaultlauncher.jpg)

To enable the navigation bar go to *Settings - Display* and toggle "show status bar" to on!

![Enable navigation bar](/assets/images/nspanelpro-sideload/statusbar.jpg)

Now you can navigate through the UI properly.

## Home Assistant

You can install Home Assistant Companion app but I had issues connecting, probably cause by the very old WebView component used in the system. Same thing goes for Fully Kiosk Browser, which complains the component is too old.

But you can still get access to your Home Assistant dashboard through a browser. Download [Chromium for Android](https://droidbang.com/link/chromium-android) and install with ADB command `adb install chromium-106.0.5228.0.apk` (version number will be different in the future).

```shell
D:\adb>adb install chromium-106.0.5228.0.apk
Performing Streamed Install
Success
```

This'll take a while, the apk is BIG! You can launch it using the launcher or with adb command `adb shell monkey - p org.chromium.chrome 1`. It will complain about the lack of Google Play services, just ignore that and proceed. Type in your Home Assistant dashboard url and open it up. 

[![Add HA to Chromium](/assets/images/nspanelpro-sideload/chromium1.jpg){: width="33%"}](/assets/images/nspanelpro-sideload/chromium1.jpg)[![Add HA to Chromium](/assets/images/nspanelpro-sideload/chromium2.jpg){: width="33%"}](/assets/images/nspanelpro-sideload/chromium2.jpg)[![Add HA to Chromium](/assets/images/nspanelpro-sideload/chromium3.jpg){: width="33%"}](/assets/images/nspanelpro-sideload/chromium3.jpg)

After logging in to Home Assistant dashboard, tap the three dots and add the page to Home Screen.

[![Add HA to Chromium](/assets/images/nspanelpro-sideload/chromium4.jpg){: width="33%"}](/assets/images/nspanelpro-sideload/chromium4.jpg)[![Add HA to Chromium](/assets/images/nspanelpro-sideload/chromium5.jpg){: width="33%"}](/assets/images/nspanelpro-sideload/chromium5.jpg)[![Add HA to Chromium](/assets/images/nspanelpro-sideload/chromium6.jpg){: width="33%"}](/assets/images/nspanelpro-sideload/chromium6.jpg)

Tapping the Assistant icon brings up a full screen view of the dashboard.

![Add HA to Chromium](/assets/images/nspanelpro-sideload/chromium7.jpg)

## Possibilities?!

This solution isn't perfect but its what I've managed for now. The best option would be to contact Itead en masse and demand support for HA Companion app and Fully Kiosk Browser out of the box. They get more sales, we get more options, its a win-win scenario and now we know it is possible. At the very least, demand an updated WebView to a compatible version. Tweet them at https://twitter.com/ITeadstudio, write them at support@itead.cc or through their [contact form](https://itead.cc/contact/) and tell them Blakadder sent you!

Digging into the adb shell revealed some interesting things like mosquitto, a bash script to interface with the Zigbee module, traces of RS485, Relay 1 and Relay 2 and probably more. I hope more Android savvy developers will hop on the train and really unlock the device.

You can still grab an NSPanel Pro for a presale discount on [Itead store](https://itead.cc/product/sonoff-nspanel-pro-smart-home-control-panel/ref/34/).