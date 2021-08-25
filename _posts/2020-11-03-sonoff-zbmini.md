---
layout: post
title: "Sonoff ZBMINI Review"
author: blak
categories: [ review, zigbee ]
tags: [ ]
image: assets/images/header_zbmini.jpg
toc: true
---

The [Sonoff Mini](https://templates.blakadder.com/sonoff_mini.html) is the most popular device Sonoff offered. Making a Zigbee version was a no brainer. And they did!

_**Full disclosure:** This is a review sample sent to me free of charge by [Sonoff](https://www.anrdoezrs.net/links/100155210/type/dlg/https://www.itead.cc/). Review is not influenced by that fact and is solely my opinion. Shopping links in this article are affiliate links and I earn a small commission when you buy through them_

Sonoff ZBMINI is a ZigBee 3.0 two-way smart in-wall relay switch that is compatible with different hubs, including SONOFF ZBBridge, Amazon Hub, Samsung SmartThings Hub and Philips Hue Hub. But we don't care about those. It has a CE and FCC certification and should work on any electrical grid with no issues.

It is available now from the [Itead store](https://www.anrdoezrs.net/links/100155213/type/dlg/https://www.itead.cc/sonoff-zbmini-zigbee-smart-switch.html) for 9.90$ which is quite a bargain for a small Zigbee 3.0 relay module.

## The Inevitable Comparison

Same, but different... But still the same! 

![Packaging](/assets/images/zbmini/comparison1.jpg)

Once you take it out of the box it looks exactly like Sonoff Mini, but... Something is missing... There is no antenna but you can still see the hole for it on the side.

Sonoff ZBMINI is using the same shell as Sonoff Mini and therefore has kept the same small dimensions of 42.6x42.6x20 mm.  

![Size comparison](/assets/images/zbmini/comparison2.jpg)

Terminal layout has remained the same but the S1 and S2 switch terminals are now a different, gray, colour. 

The shell is easy to open by pushing with a smaller screwdriver in the middle of the left and right sides. After opening the PCB layout seems very similar to Mini and the same relay is used. The relay is rated up to 16A 250V while the ZBMINI is officially rated up to 10A 240V.

![Opened up](/assets/images/zbmini/comparison3.jpg)

But once you flip it its a completely different story.

![PCB](/assets/images/zbmini/pcbback.jpg)

The brains of the operation is the chip marked MG21 which is [Silabs EFR32MG21](https://www.silabs.com/wireless/zigbee/efr32mg21-series-2-modules) and the same chip used on [Sonoff ZBBridge](https://zigbee.blakadder.com/Sonoff_ZBBridge.html). 

There are quite a few broken out pads and amongst them the RX and TX pads but since it uses MG21 it  you cannot expand on its features yet like you could with the [Sonoff BASICZBR3](https://zigbee.blakadder.com/Sonoff_BASICZBR3.html) and [custom firmware by ptvo](https://ptvo.info/sonoff-basic-zbr3-with-the-configurable-firmware-283/) for CC2530.

![MG21](/assets/images/zbmini/MG21.jpg)

S1 and S2 switch terminals are now completely separated from mains and operate on low voltage (3.3V). That is probably the reason they're coloured differently from other, mains connecting, terminals. Which also begets a warning: do not connect any AC wire to S1 and S2 or you will see smoke, fire and maybe, pain.

![Switch Terminals](/assets/images/zbmini/switch_terminals.jpg)

You can connect a single rocker switch or two rocker switches in a 2-way configuration.

## Installation

_Almost_ following the wiring schematic from the manual I connected Lin and Lout and one Neutral wire to the ZBMINI and a classic rocker switch (Sonoff ZBMINI doesn't support momentary switches) to S1 and S2 with the incoming and outgoing switch wire.

Here you can observe the ZBMINI nestled in its natural habitat:
![ZBMINI nestled in an electrical box](/assets/images/zbmini/nestled.jpg)

Sonoff recommends to install the ZBMINI this way to avoid accidental contact between the switch and uncovered screw terminals. Only problem is the pairing button is on the front side.

Out of the box the ZBMINI is already in pairing mode. If you need to re-pair, press the reset button for 5-10 seconds until the green LED inside the case starts blinking. The LED is quite hard to see so peek a bit inside.

![ZBMINI reset button](/assets/images/zbmini/nestled2.jpg)

I've successfully paired the ZBMINI with [Zigbee2Tasmota](https://tasmota.github.io/docs/Zigbee/#usage).

![ZBMINI in Tasmota](/assets/images/zbmini/z2t.jpg)

Also with [ZHA for Home Assistant](https://www.home-assistant.io/integrations/zha/). 

![ZBMINI in ZHA](/assets/images/zbmini/zha.jpg)

ZBMINI supports on/off switching and serves as a Zigbee router. It also does all the standard Zigbee binding and groups. Looking at the available clusters it should work with Touchlink as well. 

It is fully supported in Zigbee2MQTT too!

## Conclusion

Buy one or TEN! While the Wi-Fi Sonoff Mini was quite good and extremely popular, the Zigbee brother is a much better proposition for its Zigbee ecosystem:

- a smart switch alternative where you keep your existing switches and score WAF points 
- strengthens your Zigbee 3.0 network with its routing feature
- so small it fits in almost any appliance that needs smart switching
- works with A LOT of Zigbee hubs and gateways
- less than 10 USD a piece 

Nuff said!
