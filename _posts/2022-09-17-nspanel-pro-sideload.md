---
layout: post
title: "Sideload Apps to Sonoff NSPanel Pro"
author: blak
categories: [ how-to, home assistant, android, touch panel ]
tags: [ ]
image: assets/images/header_nspanelpro-sideload.jpg
toc: true
---

How to sideload apps to NSPanel Pro. 

_**Full disclosure:** This is a pre production unit sent to me free of charge by [Sonoff](https://itead.cc/ref/34/). Some aspects of it might be changed in official release. Shopping links in this article are affiliate links and I earn a small commission when you buy through them_

You can still grab an NSPanel Pro for a presale discount on [Itead store](https://itead.cc/product/sonoff-nspanel-pro-smart-home-control-panel/ref/34/?campaign=nspanelpro).

After the initial shock and disappointment with the release firmware and a smaller scale repeat of that with the September upgrade I decided to go deeper. The simplest way to do that is with [Android Debug Bridge](https://developer.android.com/studio/command-line/adb).

***Edit 2022-09-26*** Upon further inspection it seems ADB over TCP is enabled by default on some devices. On those you can skip the disassembly and USB cable. Install ADB and connect immediately with `adb connect [ip_address]` and jump to [Using ADB](#using-adb)

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

Download the [Android platform-tools](https://dl.google.com/android/repository/platform-tools-latest-windows.zip) and unzip the contents to a folder. Open a Command Prompt 
and navigate to that folder. In my case it is located at `D:\adb`

## Get ADB Access

Run command `adb devices -l`. It will list all the connected devices with extra information.

```bash
D:\adb>adb devices -l
List of devices attached
F061512302021100016    device product:px30_evb model:px30_evb device:px30_evb transport_id:3
```

Run command `adb tcpip 5555` to set the NSPanel Pro to listen for a TCP/IP connection on port 5555

```bash
D:\adb>adb tcpip 5555
restarting in TCP mode port: 5555
```

Now you can connect to the NSPanel Pro wirelessly. If you don't know the IP address run `adb shell ip -o a` to find out.

Try it out while still wired to make sure everything is working.

```bash
D:\adb>adb connect 10.1.1.144
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

Useful ADB commands:

- `adb input keyevent 26` - simulate power button press, used to turn screen on or off
- `adb input keyevent 4` - simulate back key press
- `adb input keyevent 3` - simulate home key press
- `adb input keyboard text "your_text"` - send text as if typing on screen keyboard, useful for long passwords
- `adb shell cmd statusbar expand-notifications` - show notifications menu
- `adb shell dumpsys meminfo` - show memory usage and all the running apps

## Install a Launcher

To make working with your panel easier you need to install a standard launcher. I've found the smallest launcher possible (only 7kb), which doesn't take up a lot of precious device memory.

Download the [Ultra Small Launcher](/assets/ultra-small-launcher.apk) to the ADB folder and install it with:

```sh
adb install ultra-small-launcher.apk
```

Simulate a Home key press.

```sh
adb shell input keyevent 3
```

You will be offered a selection of launchers, set "Launcher" as default.

## Enable Navigation Bar

To enable the navigation bar go to *Settings - Display* and toggle "show status bar" to on!

![Enable navigation bar](/assets/images/nspanelpro-sideload/statusbar.jpg)

Now you can navigate through the UI properly instead of using ADB commands.

## Update WebView

To run more modern apps based on WebView like Home Assistant Companion, Fully Kiosk Browser or Wallpanel you need to [update WebView component](/android-panel-webview).

## Possibilities?!

This solution isn't perfect but its the best we got for now. The ideal option would be to contact Itead en masse and demand support for HA Companion app and Fully Kiosk Browser out of the box. They get more sales, we get more options, its a win-win scenario and now we know it is possible. At the very least, demand an updated WebView to a compatible version. Tweet them at https://twitter.com/ITeadstudio, write them at support@itead.cc or through their [contact form](https://itead.cc/contact/) and tell them Blakadder sent you!

Digging into the adb shell revealed some interesting things like [mosquitto broker](https://mosquitto.org/), a way to interface with the Zigbee module, code remnants  of RS485, Ethernet, Relay 1 and Relay 2 and probably more. I hope even more Android savvy developers will hop on the train and really unlock the device's potential.

So far it can do the following:

- sideload apps
- Home Assistant dashboard via HA Companion, Fully Kiosk, Wallpanel or web browser
- run Alexa with voice recognition
- run Tuya Smart Life app (albeit the performance is not great)
- serve as MQTT broker
- ZHA coordinator over TCP
- run Zigbee2MQTT with frontend
- wake screen on proximity
- a music/video player
- iBeacon to MQTT tracker

Share with me what you've managed to do with it on [Twitter](https://twitter.com/blakadder_) or in comments!

You can still grab an NSPanel Pro for a presale discount on [Itead store](https://itead.cc/product/sonoff-nspanel-pro-smart-home-control-panel/ref/34/?campaign=nspanelpro).