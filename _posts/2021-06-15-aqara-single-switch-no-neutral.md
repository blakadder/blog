---
layout: post
title: "Aqara Single Switch T1 (No Neutral) SSM-U02 Review"
author: blak
categories: [ review, zigbee ]
tags: [ ]
image: assets/images/header_aqara_relay.jpg
toc: true
---

Best Zigbee solution to keep your existing wall switches without a neutral wire.

_**Full disclosure:** This is a review sample sent to me free of charge by [Aqara](https://www.aqara.com/eu/home.html). Review is not influenced by that fact and is solely my opinion._

Aqara created a small in-wall relay designed to work without the neutral wire to solve the issues of many old installations where there is no neutral wire in the light switch box. This module is used to retrofit your existing wall switches by mounting it behind the switch and letting you keep the existing ones much to the approval of your significant other.

![Packaging](/assets/images/aqara_relay/package.jpg)

It comes in a cardboard box that is way larger than the module itself. You could fit 4 of them but they left a lot of room for the antenna. Alongside the module you also get a tiny but thick manual in 8 languages, amongst them English, German, Russian, French and more. While cute, the text is microscopic and I recommend to use the online [PDF manual](https://cdn.aqara.com/cdn/website/mainland/static/docs/Single-Switch-Module-T1-No-Neutral_Manuals.pdf) instead of fiddling with a magnifying glass.

![Module](/assets/images/aqara_relay/module.jpg)

The module is very small, only 43x40mm in height and width and 21,5mm thick, designed to fit almost any switch box and particularly the standard European or British ones. It uses the standard high quality plastic, similar to the one used on Aqara door/window sensors and follows the white and gray Aqara design language.

Here it is next to a [Sonoff ZBMINI](/sonoff-zbmini) for comparison:

![Module](/assets/images/aqara_relay/size-comparison.jpg)

It is fully Zigbee 3.0 compliant which should enable it to work with any compatible gateway. It also supports Homekit depending on whether the gateway used is also Homekit compliant. It goes without saying that all of that works flawlessly when using Aqara’s gateways, such as the G2H Homekit camera.

Since it's designed for the European market it is CE and ROHS compliant. There’s also overheat protection for maximum safety.

Because of the no neutral design it requires a minimum load of 3W and can take a maximum of only 1250W max so don’t go connecting it to your water boiler! It will work on a range of voltages, from 100V to 250V.

![Pinouts](/assets/images/aqara_relay/pinouts.jpg)

Wall installation is simple when following all the precautions and wiring scheme laid out in the manual. Of course, if you have no idea which wire is what, it is always better to get a professional electrician to do the installation. One of the wiring methods even support a 2 way switch.

When the physical installation is done and the device is powered you have to pair the module to your Zigbee gateway. The module is in pairing mode out of the box, otherwise pairing mode is entered by holding the button on the bottom left side for 8 seconds or by toggling the external switch 5 times. 

Except using the Aqara app, I’ve tried pairing the module with ZHA, Zigbee2MQTT and Tasmota and it worked on all 3 without any issues.

There’s not a lot of competition for this market segment but Aqara brings its typical bang for your buck quality and design while solving all the issues you have smartifying your home without access to neutral wires.

It can be found in [many stores across Europe](https://www.aqara.com/eu/where-to-buy.html?region=Europe&country=All) and in other parts of the world.

