---
layout: post
title: "Sonoff NSPanel Pro Smart Home Control Panel Review"
author: blak
categories: [ review, android, T6E ]
tags: [ ]
image: assets/images/header_nspanel_pro.jpg
toc: true
---

NSPanel, but for professionals?!

_**Full disclosure:** This is a pre production unit sent to me free of charge by [Sonoff](https://itead.cc/ref/34/). Some aspects of it might be changed once it reaches production. Review is not influenced by that fact and is solely my opinion. Shopping links in this article are affiliate links and I earn a small commission when you buy through them_

Another summer, another NSPanel from Sonoff. I, once again, gladly accepted to review an early production sample of the new NSPanel, now with the added Pro moniker. 

Sonoff NSPanel is available from the [Itead store](https://itead.cc/product/sonoff-nspanel-pro-smart-home-control-panel/ref/34/?campaign=nspanelpro). In the current <s>flash sale it is only 69.99$</s> <s>early bird sale it is only 79.99$</s> preorder phase the price is only 89.90$. The final price is expected to be 119.90$.

[![Packaging](/assets/images/nspanel_pro/packaging.jpg)](/assets/images/nspanel_pro/packaging.jpg)

## Hardware

The NSPanel Pro is sizes similarly to the NSPanel but there are very notable differences. The most obvious thing is the lack of the two physical buttons. When a company is advertising something as a "Pro" model, which would mean a better or more advanced version of an existing product, you'd expect they'd keep the features that were a key component of that device. The lack of the two physical buttons does get you a bigger 3.95" screen.

[![Front](/assets/images/nspanel_pro/front.jpg)](/assets/images/nspanel_pro/front.jpg)

The back of the switch reveals another big difference. There are no power output terminals! There's only the two terminals used to connect power to it. Sonoff later confirmed there are currently no plans to have relays or any other physical controls attached to the switch.

[![Back](/assets/images/nspanel_pro/back.jpg)](/assets/images/nspanel_pro/back.jpg)

While installing the NSPanel onto my shabby testing rig another difference surfaces. The orientation of the connectors is not the same as in the old NSPanel or SwitchMan M5 series.

[![Base differences](/assets/images/nspanel_pro/base.jpg)](/assets/images/nspanel_pro/base.jpg)

No point in running a new device before checking out its internals. The main chip is hidden under the metal shielding. Under it hides a [Rockchip PX30](https://www.rock-chips.com/a/en/products/rkpower/2018/0709/913.html) chip, often used for embedded Android based systems such as those [cheap car infotainment](https://s.click.aliexpress.com/e/_De2Vn1V) units. Considering the PX30 has a small development community it is not impossible to eventually be able to flash a custom ROM to the NSPanel Pro.

[![PCB](/assets/images/nspanel_pro/pcb.jpg)](/assets/images/nspanel_pro/pcb.jpg)

What is revealed is the Zigbee chip which is the, now standard, MG21. There's also an eMMC storage chip and an USB OTG port for some reason.

[![OTG](/assets/images/nspanel_pro/otg.jpg)](/assets/images/nspanel_pro/otg.jpg)

The device also has speakers, a microphone and an ambient light sensor.

Time to fire this baby up!

## Installation

Turning on the screen I'm greeted by the boot animation and a very lengthy one at that. Boots a lot slower than the old NSPanel. Screen looks a lot brighter and viewing angles better than the original. There's an improvement.

After the forever boot you're greeted with an Android-ish welcome message. Once the keyboard pops up you know it's Android. 

[![GUI](/assets/images/nspanel_pro/firstboot.jpg)](/assets/images/nspanel_pro/firstboot.jpg)

After connecting Wi-Fi, pair the panel with the eWeLink app using the displayed QR code.

[![Initial Screen](/assets/images/nspanel_pro/initialscreen.jpg)](/assets/images/nspanel_pro/initialscreen.jpg)

The UI is well designed but not perfectly fluid. Touch screen is responsive but not perfect at all. Sometimes the gestures or taps don't register or the device is stalled for a second or two.

Swiping left or right cycles through the main screen, devices screen and scenes screen.

[![Devices](/assets/images/nspanel_pro/devices.jpg)](/assets/images/nspanel_pro/devices.jpg)

Swipe down brings you to the settings submenu with some oddly named menus.

Subject is actually Wallpaper selection. Notice lists recent notifications from NSPanel. Others are self explanatory.

[![Settings](/assets/images/nspanel_pro/settings.jpg)](/assets/images/nspanel_pro/settings.jpg)

## Features

NSPanel Pro does a lot of new things in the smart home space and some things better than existing Sonoff products.

### Camera Feed Display

You can display camera feeds right there on the screen. Any camera from the eWeLink app is supported as well as RTSP video feeds and [ESP32-CAM](https://github.com/easytarget/esp32-cam-webserver).

[![eWeLink Camera app](/assets/images/nspanel_pro/ewelinkcamera.jpg)](/assets/images/nspanel_pro/ewelinkcamera.jpg)

An [eWeLink Camera app](https://play.google.com/store/apps/details?id=com.coolkit.ewelinkcamera) was available immediately after setup. I've also added an RTSP feed from my [hacked Xiaomi Dafang camera](https://github.com/EliasKotlyar/Xiaomi-Dafang-Hacks) and that also worked flawlessly.

[![Dafang Camera](/assets/images/nspanel_pro/dafang.jpg)](/assets/images/nspanel_pro/dafang.jpg)

### ZigBee Gateway

You can pair Sonoff ZigBee devices through the NSPanel Pro and use them the same way as paired using the ZBBridge.  

[![ZigBee](/assets/images/nspanel_pro/zigbeepairing.jpg)](/assets/images/nspanel_pro/zigbeepairing.jpg)

Once paired you can access the device history and any new event will be displayed at the top of the screen.

[![Scenes](/assets/images/nspanel_pro/zigbeeevent.jpg)](/assets/images/nspanel_pro/zigbeeevent.jpg)

### Home Security Panel

Using the ZigBee sensors you can create a makeshift security system for your home with the NSPanel Pro acting as the alarm panel and control hub with the help of Sonoff ZigBee sensors.

You can, for example, create an intruder alarm with the [Sonoff Door/Window Sensor](https://zigbee.blakadder.com/Sonoff_SNZB-04.html) that will sound the alarm on the NSPanel Pro and alert you via the eWeLink app.

Or, make a doorbell that, when you press the [Sonoff Zigbee Switch](https://zigbee.blakadder.com/Sonoff_SNZB-01.html), plays a doorbell chime on the NSPanel.

### Scene Trigger

Use the touch display as a switch to trigger eWeLink scenes. After battling with the horrible eWeLink app UI to create scenes and adding them to the NSPanel Pro the end result is quite nice. I'm not a fan of the scrolling screen once you have more than 4 scenes though, it would be nice if it flips to the next page on a down swipe.

[![Scenes](/assets/images/nspanel_pro/scenes.jpg)](/assets/images/nspanel_pro/scenes.jpg)

### Future Features

Most of the "cool" features are slated for later revisions of the firmware starting with September this year.

Home Assistant support is one of them. Hopefully it will not be using the god awful official add-on hack solution. It would help out if Sonoff gets with the program and [officially integrates](https://www.home-assistant.io/blog/2022/07/12/partner-program/) with Home Assistant. We can only hope.

Power consumption and/or temperature graphing is ok but nothing groundbreaking. Integration with Yeelights also interesting but limited.

In October you can expect [eWeLink remote](https://itead.cc/ewelink-remote-control/ref/34/) support that will make better use of the [S-MATE](https://itead.cc/product/sonoff-s-mate-switch-mate/ref/34/) or [SwitchMan R5](https://itead.cc/product/sonoff-switchman-scene-controller-r5/ref/34/). Customisable wallpapers and intercom feature are also expected in this firmware update.

For the far future, Matter support is expected in 2023 so that'll be interesting... or disappointing, depending on what happens with Matter until then.

A re-review will be necessary to cover all the new upcoming features, so watch this space!

Edit 2022-09-26: Keep an eye on Sonoff's list of [NSPanel Pro supported products](https://sonoff.tech/product-review/nspanel-supported-devices-list/)

I will maintain a [YouTube playlist](https://youtube.com/playlist?list=PLWw0rdtVN1J7SAYq5mLJRst8gUxyy69BO) of shorts showing NSPanel functions and features

## Remarks

There's a bit of a disconnect between the eWeLink app and NSPanel Pro. For example you cannot set up the time zone or weather location in the app and neither does it automatically pick up those from the app settings.

Clock/timer options can only be set up on the NSPanel.

You cannot yet effectively use any of the NSPanel Pro features from the eWeLink app. For example: it is not possible to set a scene in the app that will play any of the ringtones available in NSPanel.

I find it weird that it is not possible to bring up a camera feed once an alarm or doorbell automation is triggered.

My sample's front panel mount is not the greatest and there's some rattling and movement when the panel is pushed.

<iframe width="560" height="315" src="https://www.youtube.com/embed/TYrKluoxYg8" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

One of the bugs that plagued me in the beginning was that the NSPanel stopped syncing with the app once I set up the time zone and weather location. A fix is expected.

## Conclusion

This is definitely not a new NSPanel, but a completely different device with totally different use cases. Naming this device "NSPanel Pro" set up some expectations that were not met at all. On the other hand, it does bring some new and exciting features to your smart home and promises more of them later in the year. I'm looking forward to more than a few of them.

Its use is somewhat limited but it is a good ZigBee gateway if you're just planning to enter that space.

I know a lot of people always wanted the camera feed display feature on the old NSPanel.

For now, it isn't even well integrated into the eWeLink ecosystem. There's lot of room for improvement but most of the improvements will be on the software side. Hardware is capable enough for what the NSPanel Pro strives to be and that's a smart home control panel.

If you're interested in the device, like being an early adopter and have faith that all the promised future features will be delivered on, you can grab one in the preorder sale on [Itead store](https://itead.cc/product/sonoff-nspanel-pro-smart-home-control-panel/ref/34/?campaign=nspanelpro).

But you can open up the NSPanel Pro and use it the way you want if you're willing to follow some guides:

- [Sideload apps](/nspanel-pro-sideload)
- [Update WebView component](/android-panel-webview)
- [Use proximity sensor for events](/android-panel-proximity)
