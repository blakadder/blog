---
layout: post
title: "Sonoff NSPanel Pro Secrets, Tips and Tricks"
author: blak
categories: [ how-to, android, nspanel pro, touch panel ]
tags: [ ]
image: assets/images/header_nspanel-pro-secrets.jpg
toc: true
---

A collection of secrets, tips and tricks to making your NSPanel Pro even better.

_Shopping links in this article are affiliate links and I earn a small commission when you buy through them_

This guide applies to:

{% include nspanelpro.html %}

I assume you already have [adb shell access](/nspanel-pro-sideload) to the panel.

## Emergency Recovery

Sonoff NSPanel Pro has a protection mechanism that will boot the firmware from a recovery partition in case of a firmware update failure. If you messed up something big time you can trigger this emergency recovery and factory reset everything with the following procedure:

Power on the device and wait until the Sonoff logo boot animation starts playing then power it off. Power it on again and repeat the process 5 times. After that the panel will boot into "Update Firmware Protect Mechanism" and factory reset everything to the version stored in the recovery partition.
​
![Emergency Recovery](/assets/images/nspanel_pro/emergency_recovery.jpg)

## Make Any App Default Launcher

If you want your dashboard app to start as the default launcher. Download and install [Target Home Launcher](https://www.apkpure.com/target-home-launcher/com.bh.android.TargetHomeLauncher/download?from=details). Open it and select your dashboard as the default. A warning will pop-up to set it as the default home app. Set "Target Home Launcher" as ALWAYS.

![Default Launcher](/assets/images/nspanel_pro/default_launcher.jpg)

Now your dashboard app will pop up on boot or when pressing Home button.

You can open the Ultra Small Launcher via ADB:

```sh
adb shell monkey -p l.l 1
```

## Turn Off Startup Sound

You know that annoying sound that plays while the NSPanel Pro boots. Well, it's easy to turn off. Just navigate to **Settings > Sound > Advanced** and toggle **Charging sounds** off.

![Turn off charging sound](/assets/images/nspanel_pro/charging_sound.jpg)

## Upgrade TTS Voice

PicoTTS installed on NSPanel sounds horrible and too robotic. To make it a lot better download and install [Speech Services by Google arm64-v8a](https://www.apkmirror.com/apk/google-inc/google-text-to-speech-engine/).

Once installed navigate to **System > Languages and input > Advanced > Text-to-speech output > Preferred engine** and select “Speech Services by Google”

![Speech Services](/assets/images/nspanel_pro/speech_services.jpg)

![Select Speech Services by Google](/assets/images/nspanel_pro/select_google.jpg)

See more about sending [TTS notifications](https://companion.home-assistant.io/docs/notifications/notifications-basic/#text-to-speech-notifications) from Home Assistant to HA Companion.

## Mock Location

Some apps require location services enabled. Since NSPanel Pro doesn't have a GPS we'll have to fake the location.

Download and install a fake location app such as [Fake GPS](https://m.apkpure.com/fake-gps-location/com.lexa.fakegps/download) or [Fake Traveler](https://f-droid.org/en/packages/cl.coders.faketraveler/).

Navigate to  **Settings -> System -> Developer options -> Select mock location app** and choose your app.

![Choose mock location app](/assets/images/nspanel_pro/mock_location.jpg)

Open your fake location app and set the coordinates.

## Debloat Apps

To save on memory you can debloat the NSPanel Pro and disable or uninstall some of the apps. It is best to be in `su` shell for this.

You can disable the app to prevent it from running but it will still stay in the system. You can disable some apps from the **Settings > Apps** menu. For others use:

```sh
pm disable [package.name]
```

![Apps List](/assets/images/nspanel_pro/apps.jpg)

Here's a list of the apps that should be safe to disable:

- com.eWeLinkControlPanel - The default app and launcher
- com.eWeLinkNSPro.dev - eWeLink's version of the testing app
- com.rockchip.devicetest - Device Test 
- android.rockchip.update.service - Update Service, it runs by itself occasionally
- com.android.gl2jni - Thomas's Room 
- com.smatek.test - hardware testing app
- android.rk.RockVideoPlayer - Video Player 
- acr.browser.barebones - Lightning Browser 
- org.chromium.webview_shell -  WebView Browser Tester
- com.android.music - Music Player 
- com.android.nfc - NFC service which we don't need since there's no NFC
- com.DeviceTest - yet another device test app
- com.cghs.stresstest - and another testing app, runs occasionally in background

To free up space on the device you have to uninstall the app but be aware some system apps cannot be removed this way. To uninstall use:

```sh
pm uninstall --user 0 [package.name]
```

### List Packages

To list all the packages:

```sh
pm list packages
```

To list only disabled ones:

```sh
pm list packages -d 2>/dev/null
```

## MQTT Broker

There's a fully running mosquitto MQTT broker running on your NSPanel Pro. It is there because it's used by the eWeLink Panel app to communicate with the Zigbee module. 

The broker doesn't require authentication so just connect with an MQTT client to the IP address and default port of 1883. 

If you wish to change the configuration get root access with `su` and change the `mosquitto.conf` configuration file in `/oem/siliconlabs_host/`. Here's quick guide on [setting up mosquitto broker](http://www.steves-internet-guide.com/mossquitto-conf-file/).

## SSH Access

There's also an SSH server running on standard port 22. To be able to login and control the panel via SSH instead of ADB you need to change the configuration file and add in your RSA public key. Needs `su`

```sh
mount -o remount,rw /system
```

You have to edit the file `/etc/ssh/sshd_config`. You can use [Mixplorer](https://mixplorer.com/) and edit the file in the GUI or install [Busybox](https://f-droid.org/packages/ru.meefik.busybox/) and edit in adb shell using `busybox vi /etc/ssh/sshd_config`.

uncomment line with:

```conf
#PubkeyAuthentication yes
```
to 

```conf
PubkeyAuthentication yes
```

To harden the security a bit and since I haven't been able to find out the username and password to log in change `PasswordAuthentication` to

```conf
PasswordAuthentication no
```

add you RSA public key to `/data/ssh/authorized_keys`

```sh
echo [your_pubkey] > /data/ssh/authorized_keys
```

The shell is started with the script in `/system/bin/start-ssh`.

## Install microG Services

This tip is from Mephis007 in comments, published here with edits:

You can also install [microG](https://microg.org/) services to use apps that require Google Play services functions. I've done it successfully and can now enjoy push notifications on my favourite smarthome app, Jeedom Connect.


To do so you must :
- Install & activate [FakeGApps Xposed module](https://f-droid.org/en/packages/com.thermatk.android.xf.fakegapps/)
- Install [F-Droid](https://f-droid.org/en/) and add microG repo: https://microg.org/download.html
- Install Gmscore and GsfProxy from F-Droid
- Install UnifiedNlp backend of your choice from F-Droid (I chose the Mozilla one)
- Install FakeStore app from F-Droid
- Open microG parameters and ensure log in and cloud messaging are enabled
- Grant every permission to microG (the usual Android way)
- Reboot the NSPanel Pro
- Deactivate battery optimisation for microG
- Ensure everything is setup correctly with "self check" function in microG parameters.

Enjoy!

## Install HikVision App

Submitted by Vlad Kivika in comments, published here with edits: 

I have installed, <b>Hikvision</b> app for DVR/NVR. Works good but needs some easy tricks to run properly.

First of all, Hik-Connect is not working on NSPanel. Last version couldn't work on rooted devices, but earlier versions couldn't login to the account, because they are too old.

I've tried some additional apps from <b>Hikvision</b> and stopped on <b>iVMS4500</b>, which worked good enough to check surroundings.
First of all, you need to download from [APKMirror](https://www.apkmirror.com/apk/hikvision-hq/ivms-4500/) or [Apkpure](https://m.apkpure.com/ivms-4500-hd/com.mcu.iVMSHD) and sideload the app. It will take some time.

when installation is complete, you will see the icon in the launcher. Open the app and:
1. Select your Region
2. We go to the main screen, looks like demo
3. On the top right screen, push icon, which looks like 2 cameras
![](https://uploads.disquscdn.com/images/eb016a850fcdcf8223e12d122f005d3df7d25c3edebd316492362e34f69694b9.jpg)
4. + New Device
![](https://uploads.disquscdn.com/images/a1c733e1df761142852a2aeac694a1a75f3c412d75746821dc4cd40372bd455a.jpg) 
5. Adding mode, choose IP/Domain or most relevant for you, input necessary data and save
![](https://uploads.disquscdn.com/images/9b61c28b104d5268d333725daf6e0e5cbe0464731ed20977a58fa4a7a2451229.jpg) 
6. Choose your NVR / DVR and choose Live View

There you can manage your cameras, and default views.

But the problem is that the screen of our NSPanel Pro is damn small. To bring all this to more or less compatible view, you need to go to **Settings --> Display --> Advanced --> Display Size** and move the toggle to **Small**. Don't hurry to run away from the **Display** Settings. You need to go up to **Screen Rotation** and set value to **90**. If you have your panel already fixed, you will need to disassemble it from the mount place and physicaly turn it by 90 degrees. I will explain why it's necessary just right now (but may be some one will found better solution for this).

When all above were managed, you will need to come back to <b>iVMS4500</b> app, push the 3 lines icon on the left top corner, then go to Configuration and turn on **Tablet Mode**.

![](https://uploads.disquscdn.com/images/ff85a6f88eeae65696da4ea9b808321d9ac9794bbd041a18ded2fd3ce663e8ec.jpg) 

You will be dropped from the application, but when you come back, it will work. When you turn on Tablet Mode, application will turn your screen bz 90 degrees, that's why we already rotated the screen in **Display Settings**.

## Share What You've Done

Share with me what you've managed to do with it on [Twitter](https://twitter.com/blakadder_) or in comments!
