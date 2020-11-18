---
layout: post
title: "Sonoff B02 LED Bulb Review"
author: blak
categories: [ review ]
tags: [ ]
image: assets/images/header_sonoff_bulbs.jpg
toc: true
---

New Sonoff bulbs! Are they better than the outdated B1?

_**Full disclosure:** This is a review sample sent to me free of charge by [Sonoff](https://www.anrdoezrs.net/links/100155210/type/dlg/https://www.itead.cc/). Review is not influenced by that fact and is solely my opinion. Shopping links in this article are affiliate links and I earn a small commission when you buy through them_

Itead released a new lineup of LED bulbs. It includes two classic A60 bulbs in CCT and RGBT CCT version and two dimmable vintage filament bulbs coming in A60 and ST64 bulb shapes. All those bulbs are coming only as E27 screw base and work only on 220V-240V. They will not work on a 120V grid!

They are available now from the [Itead store](https://www.anrdoezrs.net/links/100155213/type/dlg/https://www.itead.cc/):

- 9.90$/10.90$ for [CCT or RGBCCT A60](https://www.anrdoezrs.net/links/100155213/type/dlg/https://www.itead.cc/smart-home/sonoff-wi-fi-smart-led-bulb.html)
- 11.90$/12.90$ for [Filament A60/ST64](https://www.anrdoezrs.net/links/100155213/type/dlg/https://www.itead.cc/smart-home/sonoff-b02-f-smart-led-filament-bulb.html)

## Vintage Line

![Vintage](/assets/images/sonoff_bulbs/vintage.jpg)

Filament bulbs use two pairs of cold and warm white COB LED sticks. I got the ST64 Vintage looking bulb which has an amber hue to the glass and looks quite impressive and ornamental. Deep in the screw base you can see the PCB of the bulb encased in, probably, epoxy. Due to that and the glass dome you can forget about hacking it easily.

The light the bulb produces is great and definitely more than the 700lm written on the box. 

![Vintage Glow](/assets/images/sonoff_bulbs/vintageglow.jpg)

## Classic Line

![A60](/assets/images/sonoff_bulbs/a60.jpg)

Of the classic line I received the CCT version which is the standard A60 bulb size. The housing used is identical to many other Wi-Fi bulbs in the market and it's quite light which means there isn't a massive heat sink inside. The dome of the bulb is plastic.

It gives of very good light and is definitely up to specced 806lm, maybe even more since I have a feeling the 806lm is the brightness when using warm white only.

![Pick removal](/assets/images/sonoff_bulbs/disassembly.jpg)

After removing the dome with a prying tool (or guitar pick) you can see **a lot** of warm and cold white LEDs which explains the good light quality. While typical cheap Wi-Fi bulbs use a small number of LEDs (anywhere from 12 to 20 located in the outer rim), Sonoff jammed in an astounding 32 LEDs.

LED PCB is glued to the dome with thermal glue and is connected to the rest of the bulb with an unsoldered pin header.

![CCT LEDs](/assets/images/sonoff_bulbs/cctled.jpg)

After removing the top pcb we can see the Wi-Fi module. The way it is mounted and the fact the PCB is soldered to the base makes a transplant of an ESP8266 very difficult and not really worth the effort.

![Wi-Fi Module](/assets/images/sonoff_bulbs/wifimodule.jpg)

## One Giant Issue
It may be an insignificant nag for some but these bulbs have a problem. They turn on after their power has been cut and there is no way to disable that in the app. You don't want the bulbs blasting you if there's a power glitch in the middle of the night. 

The funny part is that it stores the color and dimming levels already. Adding the power state option shouldn't be an issue then.

## Smart Home Integration

Sadly Sonoff has yet again made a step backwards from their old "open to hacking" design and instead chosen to use a non Espressif Wi-Fi module, similar to the one used in Sonoff Micro. They've also opted to not include Sonoff LAN control in the firmware, making integration into a smart home extremely limited and cloud dependent. 

A glimpse of hope shined through when I was offered a firmware upgrade in eWelink app to v1.3.1. Sadly no additional local control features were added (or any new features). They should really open up LAN control of these bulbs to make them a worthy purchase.

The only way I found to, somewhat, integrate them in Home Assistant is using the [SonoffLAN](https://github.com/AlexxIT/SonoffLAN) custom component which will control the power of the bulbs through the eWelink cloud account. The responsiveness is as you would expect, not even close to instantaneous and since the bulbs use a short fade in/out when turned on/off it seems like forever until they turn on from the moment you click the switch in Home Assistant.

As there are no color or dimming controls yet all this is quite useless in real world application until the component gets updated. Luckily the community is strong and a fix is already proposed [here](https://github.com/AlexxIT/SonoffLAN/issues/269) and is waiting for release.

## The Verdict
While designed well Sonoff made a massive misstep by not allowing any kind of local control of these bulbs and with that move they're immediately outclassed by the competition. Add to that the fact it doesn't work worldwide shrinking the potential market even further.

Skip these for now and stick to the more open Sonoff devices!