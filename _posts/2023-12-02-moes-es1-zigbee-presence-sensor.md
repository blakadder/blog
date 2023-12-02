---
layout: post
title: "Moes ES1 (ZSS-LP-HP02) Zigbee Human Presence Sensor Review"
author: blak
categories: [ zigbee, home assistant, review ]
image: assets/images/header_moes_presence.jpg
toc: true
---

It was hard to resist the new Moes labelled sensor on AliExpress because it looked identical to the [Linptech ES1](https://www.aliexpress.com/item/1005005669301043.html?aff_fcid=484c0be12e2444f4ab919e7e3895cf9b-1696883239582-00352-_DkX4sHh&tt=CPS_NORMAL&aff_fsk=_DkX4sHh&aff_platform=shareComponent-detail&sk=_DkX4sHh&aff_trace_key=484c0be12e2444f4ab919e7e3895cf9b-1696883239582-00352-_DkX4sHh&terminal_id=f6d770ce532d41d9aee8c03b1a87a6b5&afSmartRedirect=y) sensor which works with Xiaomi's Mijia ecosystem but I avoided ordering it because it was Bluetooth only. When the Moes Zigbee version popped up I had to snag one to see if its any good.

_**Full disclosure:** This device is purchased with the help of your donations. Links in this article are affiliate links. By purchasing through them I also get a small commission that funds future projects!_

Moes Zigbee presence sensor was purchased from [MOES Official Store on AliExpress](https://www.aliexpress.com/item/1005005962280933.html?aff_fcid=69a54db816fa428c9bddce424d903ee1-1696883449008-00404-_DBSJht9&tt=CPS_NORMAL&aff_fsk=_DBSJht9&aff_platform=shareComponent-detail&sk=_DBSJht9&aff_trace_key=69a54db816fa428c9bddce424d903ee1-1696883449008-00404-_DBSJht9&terminal_id=f6d770ce532d41d9aee8c03b1a87a6b5&afSmartRedirect=y) for 25€ after coupons and discounts provided on the product page.

## Unboxing

The box it came with is hefty and looks really well designed  and designed solely for this sensor. It is a far cry from the typical brandless Tuya box with spelling errors. The side effect of this is that my expectations shot up higher so Moes better doesn't let me down with the build quality.  

[![Box](/assets/images/moes-presence/box.jpg){: width="33%"}](/assets/images/moes-presence/box.jpg)[![Box side](/assets/images/moes-presence/boxback.jpg){: width="33%"}](/assets/images/moes-presence/boxback.jpg)[![Box side](/assets/images/moes-presence/contents.jpg){: width="33%"}](/assets/images/moes-presence/contents.jpg)

Inside we find the sensor, a 2m long USB-C cable, metal articulating base and a manual. The short manual is mainly in English but the quick start guide is available in 6 other languages.


[![Sensor case front](/assets/images/moes-presence/front.jpg){: width="50%"}](/assets/images/moes-presence/front.jpg)[![Sensor case side profile](/assets/images/moes-presence/profile.jpg){: width="50%"}](/assets/images/moes-presence/profile.jpg)

Sensor shell is only 50mm in diameter but it's relatively thick at 20mm. Made out of slightly off-white plastic with a grey pill shaped cutout adding some pizzazz to the looks. The "peephole" gives off a significant surveilancy vibe. The presence sensor carries a small "Moes" logo on the front which I'm not a huge fan of but at the same time its not obnoxiously annoying. On the bottom is the USB-C plug which should and is a standard feature at this time.

[![Sensor back](/assets/images/moes-presence/back.jpg){: width="50%"}](/assets/images/moes-presence/back.jpg)[![Sensor side](/assets/images/moes-presence/side.jpg){: width="50%"}](/assets/images/moes-presence/side.jpg)

On the back is the model number and some sensor information encircled by a rubber ring. The middle is slightly raised to snap into the articulating base.

[![Base](/assets/images/moes-presence/base.jpg){: width="50%"}](/assets/images/moes-presence/base.jpg)[![Base 180](/assets/images/moes-presence/base180.jpg){: width="50%"}](/assets/images/moes-presence/base180.jpg)

The base is heavy and made out of thick ferrous metal with a double sided tape at the bottom. It articulates on a single axis from 0 to 180 degrees so you can just tilt or angle the sensor, can't do both at once. The packing list in the manual calls it "Magnetic base" but that's simply not true. The magnet is in the back cover of the sensor and that's a better solution.


## Moes, Tuya and/or Smart Life App

The grey pill on the front doubles as a button which is used for pairing to your Zigbee gateway. Holding the reset button for about 5 seconds puts the device in pairing mode indicated by blue LED flashing through the pill shape. After being discovered by the Smart Life app it is added as "Human Presence Sensor" which it actually is and that's a good sign.

[![Smart Life app when noone detected](/assets/images/moes-presence/smartlife_noone.jpg){: width="50%"}](/assets/images/moes-presence/smartlife_noone.jpg)[![Smart Life app](/assets/images/moes-presence/tuya_app_settings.jpg){: width="50%"}](/assets/images/moes-presence/tuya_app_settings.jpg)

When no one is detected it displays Presence none and the measure light value in lux. On detected presence you get additional information about target distance and duration of presence being detected but only in minute increments.

There aren't that many options for this sensor. The "nobody time" is the time to set presence to no one after the sensor stops detecting a human and its limited to a minimum of 10 seconds which I do not approve of. Other sensors can go down to even 0.1 seconds, albeit with varying results.

You can set only the maximum distance for detecting presence which tops out at 6 meters. Two sensitivity options are for motion and motionless sensitivity. This might be confusing to many since the sensor only detects presence or no presence. As you increase motion sensitivity the responsiveness of presence detection increases with it. The motionless sensitivity setting is for detecting non moving subjects and increasing it can cause false detections which will keep the presence on detected long after leaving the range.

Honestly I expected more configurability from a this sensor considering the price.

## Performance

Presence detection is equally fast as the other presence sensors I've tested (such as the [ZY-M100](/zy-m100)). Motion detection distance is slightly above the promised 6m range while the motionless presence detection is accurate up to 4 meters. The sensor uses 0.48W of power during operation. It serves as a Zigbee router.

What grinds my gears is the front LED which blinks very brightly when presence is detected which would be passable but it keeps on blinking in a steady interval as long as its detecting presence. Engineers did not think this would be annoying to some users so they didn't leave any way to disable the LED from the software. Guess we'll have to do it physically.

[![LED is very bright](/assets/images/moes-presence/brightled.jpg){: width="50%"}](/assets/images/moes-presence/brightled.jpg)[![Blinky LED](/assets/images/moes-presence/blinkyled.gif){: width="50%"}](/assets/images/moes-presence/blinkyled.gif)


## Integrations

### Zigbee2MQTT

Support for the sensor is recently added as Linptech ES1ZZ(TY) sensor (available on [Amazon US](https://www.amazon.com/dp/B0C7C6L66J?tag=blakadders-20)) but thsi version has the same Zigbee id's, `TS0225` and `_TZ3218_awarhusb`. You get all the options available in the Tuya app. 

[![Zigbee2MQTT Dashboard](/assets/images/moes-presence/z2m_dashboard.jpg){: width="50%"}](/assets/images/moes-presence/z2m_dashboard.jpg)

[![Device card in HA via Zigbee2MQTT](/assets/images/moes-presence/z2m_device_card.jpg){: width="50%"}](/assets/images/moes-presence/z2m_device_card.jpg)

Followers on my [Twitter](https://twitter.com/blakadder_) have inquired about its "spammyness". It sends out messages in irregular frequencies ranging between one and five seconds in general. I personally have no issues with Zigbee network congestion due to these and I run a half a dozen of various presence sensors on my Zigbee network but it seems to be an issue in certain Zigbee network setups.

### Home Assistant ZHA

This sensor is partially discovered by ZHA with occupancy and identify (used to blink the device LED when activated) entities which allow you to use the basic presence functionality. The light sensor and the configuration options are missing and will require a custom quirk which is not even close to being done. You can follow [GitHub issue #2599](https://github.com/zigpy/zha-device-handlers/issues/2599) for quirk development.

[![ZHA in Home Assistant](/assets/images/moes-presence/zha_device_card.jpg){: width="50%"}](/assets/images/moes-presence/zha_device_card.jpg)

### Home Assistant Tuya

If you have a Tuya Zigbee gateway you can add the sensor to Home Assistant using the Tuya integration but you only get partial support with presence sensor only and just a few configuration options.

[![Tuya integration in Home Assistant](/assets/images/moes-presence/tuya_device_card.jpg){: width="50%"}](/assets/images/moes-presence/tuya_device_card.jpg)

## Teardown

After removing the back rubber ring you gain access to 4 screws. Lift the back of the case and you have access to the PCB. As you can see the back cover has a sizeable magnet on it.

[![Back with screws access](/assets/images/moes-presence/backscrews.jpg){: width="50%"}](/assets/images/moes-presence/backscrews.jpg)[![Magnet](/assets/images/moes-presence/magnet.jpg){: width="50%"}](/assets/images/moes-presence/magnet.jpg)

Well, well, well, doesn't this radar sensor look familiar. Moes went to the trouble of grinding of the radar MCU label and tried to remove the PCB silkscreen which says WiGig, exactly the same as the LD2410. Cute but I've seen through your attempted deception Moes. One the other side is a common Tuya Zigbee module [ZS3L](https://developer.tuya.com/en/docs/iot/zs3l?id=K97r37j19f496).

[![PCB](/assets/images/moes-presence/pcb.jpg){: width="50%"}](/assets/images/moes-presence/pcb.jpg)[![Sensor module side](/assets/images/moes-presence/pcbmodule.jpg){: width="50%"}](/assets/images/moes-presence/pcbmodule.jpg)

It is indeed the LD2410 which is already proven to be a very capable presence detection radar and is widely used in the DIY community. That explains the very good performance but at the same time makes me wonder why they didn't allow for more configurability because the radar is capable of much, much more.

[![Scratched out chip](/assets/images/moes-presence/scratchedoutchip.jpg){: width="50%"}](/assets/images/moes-presence/scratchedoutchip.jpg)[![Scratched out name](/assets/images/moes-presence/scratchedoutname.jpg){: width="50%"}](/assets/images/moes-presence/scratchedoutname.jpg)

It is worthy to note that this device doesn't have an additional Tuya MCU between the connectivity module and the sensors. The LD2410 is connected directly to the Zigbee module via the RX and TX lines which contributes to its responsiveness.

The light sensor is sandwiched in the middle of the two LEDs and the blinking offender is the one on top marked LED1. There are a few options of "configuring" the LED. You can mask it with electrical tape which will make it still shine through a little bit. For a total blackout paint over it or just snip/solder it off as the nuclear solution. 

[![LED light location](/assets/images/moes-presence/ledlocation.jpg){: width="50%"}](/assets/images/moes-presence/ledlocation.jpg)

## Is It Good Enough?

The stupid blinking LED is extremely annoying. Yes, there's a way to deal with it but why isn't there a simple toggle in the settings. On top of that, the minimum of 10 seconds for non presence detection and limited configuration options do not make it any more desirable. The size of the sensor, specifically its thickness is something I'm personally not a fan of when there are thinner options floating around but it's not that big of a deal.

You can get a lot more from a presence sensor for less than 20€ with a [ZY-M100](/zy-m100) or the new candidate I'm testing: ZG-205 ([AliExpress](https://www.aliexpress.com/item/1005005951964477.html?aff_fcid=2296362ef35549d1b085e7f7188f7b4d-1696890047820-09063-_DmuFAd1)).

If you're not at all deterred by the above, can't deny the fact it uses a proven 24Ghz radar that's responsive and accurate. The power usage is very low and it acts as a Zigbee router.

If you still like it it is available at these stores:
- [Moes Official AliExpress store](https://www.aliexpress.com/item/1005005962280933.html?aff_fcid=69a54db816fa428c9bddce424d903ee1-1696883449008-00404-_DBSJht9&tt=CPS_NORMAL&aff_fsk=_DBSJht9&aff_platform=shareComponent-detail&sk=_DBSJht9&aff_trace_key=69a54db816fa428c9bddce424d903ee1-1696883449008-00404-_DBSJht9&terminal_id=f6d770ce532d41d9aee8c03b1a87a6b5&afSmartRedirect=y)
- [Moes Online Store](https://moeshouse.com/products/zigbee-human-presence-sensor/?ref=v4thya2eufek) - use coupon BLAKADDER
- [Amazon US](https://www.amazon.com/dp/B0C7C6L66J?tag=blakadders-20) (The identical Linptech ES1 version)

Once again, thanks to all my donators via [Ko-Fi](https://ko-fi.com/blakadder) and [PayPal](https://www.paypal.me/tasmotatemplates) and everyone using the affiliate links for funding this review and making it as objective as possible.