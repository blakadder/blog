---
layout: post
title: "BlitzWolf BlitzHome BH-AP1 Smart Air Purifier Review"
author: blak
categories: [ review ]
tags: [ ]
image: assets/images/header_blitzhomeairpurifier.jpg
toc: true
---

A smart air purifier for smaller rooms.

_**Full disclosure:** This is a review sample sent to me free of charge by [Banggood](https://www.banggood.com?p=CM27171011078201412U&custlinkid=1201630). Review is not influenced by that fact and is solely my opinion._

BlitzWolf, or BlitzHome, as they brand their home appliances now, offers a cheap air purifier for smaller spaces up to 30m<sup>2</sup>.

[BlitzHome BH-AP1 Smart Air Purifier](https://www.banggood.com/BlitzHome-BH-AP1-Smart-Air-Purifier-220m-or-h-CADR-26dB-Quiet-Air-Cleaner,Removes-Allergies,Smoke,Dust,Mold,Pollen,Pet-Dander,Activated-Carbon-Eliminates-Odors-and-Deodorizes-HEPA-Filter-with-Night-Light-APP-Remote-Control-p-1721669.html?cur_warehouse=CZ&p=CM27171011078201412U&custlinkid=2124316) is available from [Banggood](https://www.banggood.com/BlitzHome-BH-AP1-Smart-Air-Purifier-220m-or-h-CADR-26dB-Quiet-Air-Cleaner,Removes-Allergies,Smoke,Dust,Mold,Pollen,Pet-Dander,Activated-Carbon-Eliminates-Odors-and-Deodorizes-HEPA-Filter-with-Night-Light-APP-Remote-Control-p-1721669.html?cur_warehouse=CZ&p=CM27171011078201412U&custlinkid=2124316), delivered from their EU warehouse in a matter of days. 

## Features

![Air Purifier in its natural habitat](/assets/images/bh-ap1/unpacked.jpg)
<figcaption class="figure-caption text-center">Air Purifier in its natural habitat.</figcaption>

![Manuals and paperwork](/assets/images/bh-ap1/user_manual.jpg)

Once unpacked from a single brown box you're left with a not so large device, only 41 cm tall and 23.8 cm in diameter. You also get a warranty card and a multi language manual explaining all the functions and the pairing process. 

Make sure to open the bottom lid and remove the protective bag from the filter before turning it on.

![Remember to remove the plastic bag from the filter](/assets/images/bh-ap1/unpackfilter.jpg)

Main control panel is situated on the top with its overly glossy, very prone to scratches, black plastic.

![The black plastic is easily scratched](/assets/images/bh-ap1/scratches.jpg)
<figcaption class="figure-caption text-center">To be fair, I did turn it upside down and rested it on the hardwood floor while installing the filter so that one is on me.</figcaption>

It comes with a Type F plug and is intended only for 220/240V power. After plugging it in a LED strip in the bottom lights up. It glows green when the air quality is excellent, yellow when its good and red when the PM 2.5 levels go into unhealthy ranges.

![The light show](/assets/images/bh-ap1/led.jpg)

The purifier knows the air quality with the built in PM 2.5 sensor, located in the back, behind the vent left of the product label.

![PM 2.5 sensor location](/assets/images/bh-ap1/sensor.jpg)

The label noise level listed on the label applies to "speed 2", the highest speed. In "speed 1" mode it is somewhat audible in a calm environment but in sleep mode it is whisper quiet. I was genuinely surprised by how quiet it was! Additionally, in sleep mode, all the lights are turned off and there is no sound from the built in beeper. I am very thankful for that mode.

## The App(s)

Since the manual recommends using the BlitzHome app I went with that first. Pairing it to the BlitzHome app was not as straightforward as expected. Despite the WiFi icon blinking, it was blinking to slow. I had to hold it until it went into pairing mode and the icon started blinking fast. Scanning for new devices in the app didn't find the BH-AP1 so I had to look for it and find it under "Small Home Appliances" and select Air Purifier (WiFi).

All the features from the control panel are available in the app plus an outdoor PM reading and weather based on your location.

You can switch it on/off, change the working modes between "sleep", "auto" and manual and change between 2 fan speeds when in manual mode. In "Setting" option you reset the filter life counter back to 100% or start the built in countdown timer.

![App UI](/assets/images/bh-ap1/app.jpg)

Filter life is displayed in percentages and is calculated from the estimated 2000 hours or 6 months life span of the filter. There is no special filter sensor, filter life counter kept decreasing even when the filter wasn't installed. Once the counter runs out the device will notify you through the app and by flashing the LEDs in the bottom.

Since the description says it supports Tuya and Smart Life, I paired it to those apps as well. This time, during the pairing process both of the apps gave a friendly, visual reminder on how fast the WiFi icon needs to blink. I still had to manually search of the device in the list to add it, scanning for it yielded nothing.

The UI and options in Tuya and Smart Life are identical to BlitzHome.

There is another, potentially important, issue with the PM sensor: it doesn't work when the Air Purifier is turned off which is a shame because it's an important feature to automatically turn on when the air becomes too polluted.

## Google Home and Alexa Support

Nope. Nothing. Not possible. The manual does have some interesting wording about the support:

![Integration lies](/assets/images/bh-ap1/integrationmanual.jpg)

When you do as written there are no buttons for Google Home, Alexa nor IFTTT (not that anyone wants to use IFTTT anymore). Does that mean there is no support afterall?

## Home Assistant Integration

Since it pairs to Tuya or Smart Life app, it only makes sense to try and integrate it in Home Assistant using the official [Tuya integration](https://www.home-assistant.io/integrations/tuya/) since Google Home and Alexa were a bust.

After adding the Tuya integration the device was discovered as "Blitzwolf (unsupported)". That does not bode well at all!

![Home Assistant UI](/assets/images/bh-ap1/homeassistant.jpg)

 Opening the device card I saw the two entites. Main power switch is separate from the fan entity. The fan entity does not have mode selection, only the on/off function and the speed slider which is awkward since changing the speed enters "manual" mode automatically and it switches to "auto" mode when turned on. Not a lot of control options.

![The limited fan entity](/assets/images/bh-ap1/fan_entity.jpg)

You do get the PM 2.5 and filter life counter as sensors. There's also a switch entity to reset the filter life countdown. That's something at least. If only the PM 2.5 sensor worked when the device is off.

A selector entity is for the built in timers mode but it only offers the 1h, 2h and 4h modes. I guess the 8h timer got lost in translation. 

The device description later changed to "BlitzWolf (ajiovq2gnazbslep)" but nothing changed with the entities. 

Maybe it'll improve in the future but who wants to wait...

## Will it Tasmotize?

Yes! Lucky for us, even in late 2022 it still uses the TYWE3S module which is easily exposed by popping off the top control panel from the fan guard. [Device template for Tasmota](https://templates.blakadder.com/blitzhome_BH-AP1.html) has the disassembly instructions and Tasmota configuration.

I'm also working on a guide and blueprints to fully integrate it in Home Assistant with all the features.

## Conclusion
As it is it's still a decent air purifier but a poor smart device unless you plan on using solely Tuya's ecosystem. I don't see why its so difficult to at least have basic on/off and speed controls through Google or Alexa. 

If you're going to flash Tasmota (or ESPhome) on it and if you find it under 75$ on a sale [on Banggood](https://www.banggood.com/BlitzHome-BH-AP1-Smart-Air-Purifier-220m-or-h-CADR-26dB-Quiet-Air-Cleaner,Removes-Allergies,Smoke,Dust,Mold,Pollen,Pet-Dander,Activated-Carbon-Eliminates-Odors-and-Deodorizes-HEPA-Filter-with-Night-Light-APP-Remote-Control-p-1721669.html?cur_warehouse=CZ&p=CM27171011078201412U&custlinkid=2124316) it is a small and quiet air purifier. Will work perfectly in the bedroom or, in my case, the office.


