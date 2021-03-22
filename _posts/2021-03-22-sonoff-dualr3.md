---
layout: post
title: "Sonoff DUALR3 Review"
author: blak
categories: [ review ]
tags: [ ]
image: assets/images/header_dualr3.jpg
toc: true
---

Sonoff's dual relay Wi-Fi smart switch with power metering and bluetooth (kind of)

_**Full disclosure:** This is a review sample sent to me free of charge by [Sonoff](https://www.anrdoezrs.net/links/100155210/type/dlg/https://www.itead.cc/). Review is not influenced by that fact and is solely my opinion. Shopping links in this article are affiliate links and I earn a small commission when you buy through them_

Sonoff DUALR3 is the successor of the DUAL and DUALR2 but is nothing alike its predecessors apart from having dual relays. Compared to them its way, way smaller and comes with power monitoring, two mains switch inputs and uses the ESP32 as its brains. It can be a dual switch relay, a motor controller or a dedicated power monitoring device with the relays permanently disabled. All that is crammed into a tiny 54x49x24mm case that fits in all electrical boxes worldwide or mounts on standard DIN rail with the accompanying clip-on mounting base.

It is available now from the [Itead store](https://www.anrdoezrs.net/links/100155213/type/dlg/https://www.itead.cc/sonoff-dualr3.html) for 13.98$ which is a great price for such a powerful package. 

## The Box

The box it comes in is a lot smaller than expected. 

![Packaging](/assets/images/dualr3/package.jpg)

Besides the DUALR3 you will receive the DIN rail mount and the usual tiny print manual with pairing instructions and wiring schematics for [various configurations](https://sonoff.tech/product/wifi-diy-smart-switches/dualr3).

![Device image](/assets/images/dualr3/inputs.jpg)

DUALR3 supports 100-240V 10A per relay but only 15A combined which is a bit of a downer but for intended uses (lights or curtains) you'll rarely exceed the specs. With a single neutral input you get two Live inputs and outputs. This might cause a slight issue if you have two separate neutrals at the location you want to install it. Always consult a qualified electrician to avoid smoke and/or fire.

While small, DUALR3 is still bigger than Sonoff MINI (in the image the original MINI version was used) but packs a LOT more for the size difference. 

![Comparison](/assets/images/dualr3/comparison.jpg)
![Comparison too](/assets/images/dualr3/comparison2.jpg)

## Installation

Out of the box the DUALR3 is already in pairing mode but its not your usual Wi-Fi pairing. Using the ESP32 to it fullest (he said sarcastically!) you need to choose the "Bluetooth pairing" when using the Add device option in eWeLink app. Once it finds the device with a very non descriptive name, or rather a bunch of numbers and letters, it will connect it to the Wi-Fi network of your choice.

Once it is paired you will select the way you intend to use the device. No worries, you can always change it in settings.

![App Selection](/assets/images/dualr3/app.jpg)

The app further offer the standard schedule and timer options alongside power consumption for each input separately. 

My power consumption measurements were showing my mains voltage 210V and below all the time and I didn't notice any way to calibrate that.

There is no Local mode option in the settings and the DIY mode which is proudly displayed on the box **does not work** with the version 1 of the firmware. Not a great decision by ITEAD since the DIY mode is why a lot of people would buy the DUALR3.

## Smart Home Integration

DUALR3 is partially supported in Home Assistant when using the [SonoffLAN](https://github.com/AlexxIT/SonoffLAN) custom component. It will automatically find both DUALR3 relays in cloud mode and present them as light entities in HA. There's no shutter mode or power monitoring support yet. You can follow the [GitHub thread](https://github.com/AlexxIT/SonoffLAN/issues/399) to catch any updates.

![HA Integration](/assets/images/dualr3/ha.jpg)

## Conclusion

While the hardware is truly good and form factor is practical the factory intended firmware is lacking as usual! 

Seems its back to the good ole [Tasmota flash](https://templates.blakadder.com/sonoff_DUALR3.html) to unlock the full power this device can offer. Once you do and you get a dual relay device that is miles ahead of the competition.

