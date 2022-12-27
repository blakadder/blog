---
layout: post
title: "Sonoff ZBMINIL2 Extreme No Neutral Zigbee Smart Switch Review"
author: blak
categories: [ review, zigbee ]
tags: [ ]
image: assets/images/header_zbminil2.jpg
toc: true
---

A tiny solution to a big problem in the smart home world!

_**Full disclosure:** This is a review sample sent to me free of charge by [Sonoff](https://www.itead.cc/ref/34/). Review is not influenced by that fact and is solely my opinion. Shopping links in this article are affiliate links and I earn a small commission when you buy through them_

Sonoff ZBMINIL2 is a successor to the [ZBMINI](https://zigbee.blakadder.com/Sonoff_ZBMINI) and [ZBMINI-L](https://zigbee.blakadder.com/Sonoff_ZBMINI-L) in-wall relay modules. It takes the best of those two and combines them into the smallest smart relay module I ever saw. And it doesn't even need neutral wire to run!

It is available now from the [Itead store](https://itead.cc/product/sonoff-zbminil2-extreme-zigbee-smart-switch-no-neutral-required/ref/34/).

## The Inevitable Comparison: Part 2

As the newest in the ZBMINI line, it carries the DNA of its predecessors but is now 40% smaller and 100% better designed. It looks much better with the new curved edge design and the orange stripe.

[![Front side](/assets/images/zbminil2/zbmini1.jpg){: width="33%"}](/assets/images/zbminil2/zbmini1.jpg)[![Back side](/assets/images/zbminil2/zbmini2.jpg){: width="33%"}](/assets/images/zbminil2/zbmini2.jpg)[![Terminal side](/assets/images/zbminil2/zbmini3.jpg){: width="33%"}](/assets/images/zbminil2/zbmini3.jpg)

It has managed to squeeze into a miniscule 39.5x32x18.4mm shell and will now be able to fit into the tightest of electrical boxes. For a better comparison here it is next to a ZBMINI.

[![Front side](/assets/images/zbminil2/comparison2.jpg){: width="33%"}](/assets/images/zbminil2/comparison2.jpg)[![Back side](/assets/images/zbminil2/comparison1.jpg){: width="33%"}](/assets/images/zbminil2/comparison1.jpg)[![Terminal side](/assets/images/zbminil2/comparison3.jpg){: width="33%"}](/assets/images/zbminil2/comparison3.jpg)

The shell can be opened by separating the shell down the middle. Inside is a small 8A relay which partly allowed for the significant size reduction. Zigbee communication is running on a modern [Silabs EFR32MG22](https://www.silabs.com/wireless/zigbee/efr32mg22-series-2-socs) chip.

[![Inside](/assets/images/zbminil2/pcb1.jpg){: width="50%"}](/assets/images/zbminil2/pcb1.jpg)[![Relay](/assets/images/zbminil2/pcb2.jpg){: width="50%"}](/assets/images/zbminil2/pcb2.jpg)
[![Zigbee module](/assets/images/zbminil2/pcb4.jpg){: width="50%"}](/assets/images/zbminil2/pcb4.jpg)[![Back side](/assets/images/zbminil2/pcb3.jpg){: width="50%"}](/assets/images/zbminil2/pcb3.jpg)

## Installation

Installation is easy following the wiring schematic from the manual. Connect Lin and Lout and the classic rocker switch to S1 and S2 and you're ready to go. 
Once installed into a standard 63mm EU box you realize how much it shrunk.

[![Installed into a 63mm electrical box](/assets/images/zbminil2/zbminiinstalled.jpg)](/assets/images/zbminil2/zbminiinstalled.jpg)

S1 and S2 switch terminals allow for multiple switch configurations including a 2-way switch setup. Be warned that they're connected to mains electricity so you need to use AC rated switches.

![Switch Wiring](/assets/images/zbminil2/switchwiring.jpg)

ZBMINIL2 supports classic rocker switches (as factory default) or momentary switches. 

To change the switch type used: short press the button 3 times until the green LED blinks 3 times quickly. That indicated the switch type is successfully changed.

Out of the box ZBMINIL2 is already in pairing mode. If you need to re-pair, hold the device button for 5 seconds until the green LED slowly blinks.

I connected mine to a couple of classic bulbs and a candle type smart bulb and it just worked.

## Compatibility and Features

ZBMINIL2 is naturally supported by the eWeLink app and will also pair with Amazon Echo Zigbee hub.

ZBMINIL2 supports on/off switching and setting the default power on state but is not a Zigbee router/repeater. 

It is sorely missing an option to decouple the switch from the relay which would make it a great solution with all smart lighting.

### Zigbee2MQTT

I've successfully paired the ZBMINI with [Zigbee2MQTT](https://www.zigbee2mqtt.io/).

![ZBMINIL2 in Zigbee2MQTT](/assets/images/zbminil2/z2m_exposes.jpg)

### ZHA for Home Assistant

Worked out of the box with [ZHA for Home Assistant](https://www.home-assistant.io/integrations/zha/) as well. It must be following Zigbee specifications unlike many Tuya branded relays.

![ZBMINIL2 in ZHA](/assets/images/zbminil2/ha_device_card.jpg)


## Conclusion

If you have old electrical installations like me, with no neutral wires with switches and undersized shallow electrical boxes the ZBMINIL2 is here to help. 

It does have limitations you have to keep in mind. It will not work with inductive loads (like fans) and requires the connected device to use at least 3W power. It is rated as 6A 250V max. In my mind it seem it will be used mostly for lighting.

It is currently priced at 13.99$ which initially seemed a bit high to me. I guess global inflation and component shortage is a thing. Since I'm not aware of a similar solution of this size with a competitive price you don't have many choices anyway and you can drop the price a bit with discount offers.

Buy it from the [Itead store](https://itead.cc/product/sonoff-zbminil2-extreme-zigbee-smart-switch-no-neutral-required/ref/34/).
