---
layout: post
title: "Sideload Apps to T6E Android Smart Home Panels"
author: blak
categories: [ how-to, android, T6E ]
tags: [ ]
image: assets/images/header_t6e-sideload.jpg
toc: true
---

How to sideload apps to T6E based Android smart home touch panels. 

_Shopping links in this article are affiliate links and I earn a small commission when you buy through them_

<p>This guide applies to touch control panels such as:</p>
<ul>
  <li>Moes T6E Smart Multi-functional Control Panel (<a href="https://www.moeshouse.com/products/tuya-smart-home-multi-functional-touch-screen-control-panel-4-inch-in-wall?ref=v4thya2eufek">Moes store</a>*, <a href="https://www.aliexpress.com/item/1005003799973429.html?aff_fcid=a6ecab89ce6641d88b11cf84aaf81932-1664369338596-08437-_Dee5hOB&amp;tt=CPS_NORMAL&amp;aff_fsk=_Dee5hOB&amp;aff_platform=shareComponent-detail&amp;sk=_Dee5hOB&amp;aff_trace_key=a6ecab89ce6641d88b11cf84aaf81932-1664369338596-08437-_Dee5hOB&amp;terminal_id=5328bb0326ad4ecea39a5766fa327b23&amp;afSmartRedirect=y">AliExpress</a>*)</li>
  <li>Aubess Ethernet Smart Control Panel (<a href="https://www.aliexpress.com/item/1005004639636958.html?aff_fcid=33974372f9ca4396a4ebc4d388677d06-1664369339410-05923-_DltEVer&amp;tt=CPS_NORMAL&amp;aff_fsk=_DltEVer&amp;aff_platform=shareComponent-detail&amp;sk=_DltEVer&amp;aff_trace_key=33974372f9ca4396a4ebc4d388677d06-1664369339410-05923-_DltEVer&amp;terminal_id=5328bb0326ad4ecea39a5766fa327b23&amp;afSmartRedirect=y">AliExpress</a>*)</li>
  <li>Corui Tuya Multi-functional Touch Screen with Alexa (<a href="https://www.aliexpress.com/item/1005004771330533.html?aff_fcid=3cca54898e7a48eca8e175afa87f980d-1664791198884-07428-_DlNe5lz&amp;tt=CPS_NORMAL&amp;aff_fsk=_DlNe5lz&amp;aff_platform=shareComponent-detail&amp;sk=_DlNe5lz&amp;aff_trace_key=3cca54898e7a48eca8e175afa87f980d-1664791198884-07428-_DlNe5lz&amp;terminal_id=5328bb0326ad4ecea39a5766fa327b23&amp;afSmartRedirect=y">AliExpress</a>*)</li>
  <li>Zemismart T6E (<a href="https://www.zemismart.com/products/t6e?DIST=QEVHGw%3D%3D">Zemismart store</a>*, <a href="https://www.aliexpress.com/item/1005004295932676.html?aff_fcid=fe04b4ef296b4831976c1806acc2035e-1664869926171-01577-_DnDbrb1&amp;tt=CPS_NORMAL&amp;aff_fsk=_DnDbrb1&amp;aff_platform=shareComponent-detail&amp;sk=_DnDbrb1&amp;aff_trace_key=fe04b4ef296b4831976c1806acc2035e-1664869926171-01577-_DnDbrb1&amp;terminal_id=5328bb0326ad4ecea39a5766fa327b23&amp;afSmartRedirect=y">AliExpress</a>*)</li>
</ul>
## Disassembly

Remove the panel from the base. Unscrew the two screws on the back then carefully pry away the back plate starting at this point.

[![Unscrew two screws](/assets/images/t6e-sideload/open1.jpg){: width="50%"}](/assets/images/t6e-sideload/open1.jpg)[![Lift the panel](/assets/images/t6e-sideload/open2.jpg){: width="50%"}](/assets/images/t6e-sideload/open2.jpg)

Now unscrew the tiny screws circled. Remove the tape from the touch panel connector and disconnect the connector to lift up the PCB.

[![Unscrew tiny screws](/assets/images/t6e-sideload/open3.jpg){: width="50%"}](/assets/images/t6e-sideload/open3.jpg)[![Disconnect the touch controller](/assets/images/t6e-sideload/open4.jpg){: width="50%"}](/assets/images/t6e-sideload/open4.jpg)

Connect the data USB cable from your computer to the OTG port.

[![Connect the USB cable](/assets/images/t6e-sideload/open5.jpg)](/assets/images/t6e-sideload/open5.jpg)

## Install ADB and Drivers

Download [ADB drivers](https://developer.android.com/studio/run/win-usb) and install.

When the T6E panel is connected to your computer via USB you should have a new device in Device Manager.

[![Front](/assets/images/t6e-sideload/device_manager.jpg)](/assets/images/t6e-sideload/device_manager.jpg)

Download the [Android platform-tools](https://dl.google.com/android/repository/platform-tools-latest-windows.zip) and unzip the contents to a folder. Open a Command Prompt 
and navigate to that folder. In my case it is located at `D:\adb`

## Get ADB Access

This command will list all the connected devices with extra information.

```sh
adb devices -l
```

_Output:_

```shellsession
D:\adb>adb devices -l
List of devices attached
F070712302013500125    device product:px30_evb model:px30_evb device:px30_evb transport_id:8
```

Run the following command to make ADB over TCP permanent:

```sh
adb shell setprop persist.adb.tcp.port 5555
```

To set the T6E panel to listen for a TCP/IP connection on port 5555 run:

```sh
adb tcpip 5555
```

_Output:_

```shellsession
D:\adb>adb tcpip 5555
restarting in TCP mode port: 5555
```

Now you can connect to the T6E panel wirelessly. If you don't know the IP address run `adb shell ip -o a` to find out.

Try it out while still wired to make sure everything is working.

```shellsession
D:\adb>adb connect 10.1.1.105
connected to 10.1.1.105:5555
```

Reassemble the panel and plug it into the base again. Once it boots, run:

```sh
adb connect [ip_address]
```

## Using ADB

With ADB access you have a powerful tool at your disposal to take control of the T6E panel. You can use to install and uninstall apps, list running processes, free memory and even gain root access.

Here's a list of [ADB commands](https://technastic.com/adb-commands-list-adb-cheat-sheet/) you can use on your T6E panel.

A useful one is a simulated press of the Home button with

```shell
adb shell input keyevent 3
```

You can get root access with `su` while in ADB shell.

Push a file to T6E panel, f.e. a new wallpaper:

```shellsession
D:\adb>adb push wallpaper.jpg /sdcard/Download/wallpaper.jpg
wallpaper.jpg: 1 file pushed, 0 skipped. 18.5 MB/s (52101 bytes in 0.003s)
```

Useful ADB commands:

- `adb input keyevent 26` - simulate power button press, used to wake device or put it to sleep
- `adb input keyevent 4` - simulate back key press
- `adb input keyevent 3` - simulate home key press
- `adb input keyboard text "your_text"` - send text as if typing on screen keyboard, useful for long passwords
- `adb shell cmd statusbar expand-notifications` - show notifications menu
- `adb shell dumpsys meminfo` - show memory usage and all the running apps

## Install a Launcher

To make working with your panel easier you need to install a standard launcher. I've found the smallest launcher possible (only 7kb), which doesn't take up a lot of precious device memory.

Download the [Ultra Small Launcher](/assets/files/ultra-small-launcher.apk) to the ADB folder and install it with:

```sh
adb install ultra-small-launcher.apk
```

Simulate a Home key press.

```sh
adb shell input keyevent 3
```

You will be offered a selection of launchers, set "Launcher" as default.

## Disable Tuya Panel App

The Tuya panel app is very pervasive. A swipe from the top will pull down the settings menu from the Tuya app and then you're stuck in it again unless you use `adb shell input keyevent 3`. It will also load over any default launcher you set after a reboot. Best way to handle that is to disable them.

Gain shell access with `adb shell` then switch to root with `su`. Disable with these two commands:

```sh
pm disable com.tuya.iotgateway.launcher
pm disable com.smartpad.fourinchneeu.smart
```

_Output:_

```shellsession
px30_evb:/ $ su
px30_evb:/ # pm disable com.tuya.iotgateway.launcher
Package com.tuya.iotgateway.launcher new state: disabled
px30_evb:/ # pm disable com.smartpad.fourinchneeu.smart
Package com.smartpad.fourinchneeu.smart new state: disabled
```

## Screen Navigation

T6E panels have removed the navigation bar from the system and I couldn't figure out a way to restore it. As a replacement I found the J Touch app to assist with navigation.

Download [J Touch](https://apkpure.com/j-touch/com.bs.smarttouch.gp/download) and install with ADB:

```sh
adb shell install "J Touch_v1.7.0-GP_apkpure.com.apk"
```

Open J Touch on the panel, grant the necessary permissions and configure gestures for navigation per your preference. I set single tap for back and swipe up for home screen.

## Update WebView

To run more modern apps based on WebView like Home Assistant Companion, Fully Kiosk Browser or Wallpanel you need to [update WebView component](/android-panel-webview).

Share with me what you've managed to do with it on [Twitter](https://twitter.com/blakadder_) or in comments!