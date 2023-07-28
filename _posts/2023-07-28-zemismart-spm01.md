---
layout: post
title: "Zemismart SPM01 Zigbee Power Monitor"
author: blak
categories: [ review, zigbee ]
tags: [ ]
image: assets/images/header_spm01.jpg
toc: true
---

Zemismart SPM01 is a small, high accuracy, bi-directional energy monitor with Zigbee or Wi-Fi connectivity.

_**Full disclosure:** This is a review sample sent to me free of charge by [Zemismart](https://www.zemismart.com/?DIST=QEVHGw%3D%3D). Review is not influenced by that fact and is solely my opinion. Shopping links in this article are affiliate links and I earn a small commission when you buy through them_

I've received the Zigbee version of SPM01 for review from Zemismart.

[![Box](/assets/images/spm01/box.jpg)](/assets/images/spm01/box.jpg)

It provides real time monitoring of voltage, current and power via a current transformer (CT) sensor. The metering is bi-directional allowing you to also measure electricity production from f.e. solar and base automations off of that.

Official specifications are the following:

- Rated operating voltage Un: 110 ~ 240 VAC, 50/60 Hz
- Poles: 1P+N
- Basic current Ib: 10 A
- Starting current Ist: 50 mA
- Max current Imax 63 A
- Over-voltage category: III
- Rated insolating voltage Ui 250V
- Energy measurement accuracy class (IEC 61557-12): Class 1.0 (± 1%)
- Power consumption: Normal using 0.5 Watt / Pairing mode 1 Watt
- Rated operating temperature: -25 ~ 60 ℃

It is available from:
- [Zemismart store](https://www.zemismart.com/products/spm01-d2tz-zm?DIST=QEVHGw%3D%3D) - use special code `4VBQF07F` for 10% discount
- [AliExpress](https://www.aliexpress.com/item/1005005589632247.html?aff_fcid=b12239cc38404a5e8dbcb585eed38a66-1690543806965-07541-_DmSpQML&tt=CPS_NORMAL&aff_fsk=_DmSpQML&aff_platform=shareComponent-detail&sk=_DmSpQML&aff_trace_key=b12239cc38404a5e8dbcb585eed38a66-1690543806965-07541-_DmSpQML&terminal_id=6db88f7b3fff4670be83ec2d245af448&afSmartRedirect=y)

With a small footprint it can be easily installed under a DIN rail circuit breaker taking no additional space in the distribution box. It can also be mounted as a stand alone device in an electrical box or behind a wall outlet or an appliance.

Immediately out of the box I notice that this is a higher quality product than the majority of Tuya smart devices I encounter. The shell is well built and the two wires for powering the module are real silicone wires with crimped ends.

[![Front side](/assets/images/spm01/front.jpg){: width="50%"}](/assets/images/spm01/front.jpg)[![Side view](/assets/images/spm01/side.jpg){: width="50%"}](/assets/images/spm01/side.jpg)

"Powered by BITUOTECHNIK" is written on one side. I immediately punch that into Google and discover the sensor is developed by [Bituo Technik](https://bituo-technik.com/product/spm01-1pn-63a/). The company specialises in current monitoring and protection which is a good omen that I'm not reviewing a dud.

## Installation

Hardware installation is simple and the instruction booklet has images for all types of installations and wire orientations. The most important part is to match the arrow next to the device LED with the flow of electricity. During testing I installed it in my small test bed in an electrical box. I'll leave messing with circuit breakers to professionals.

[![Test setup](/assets/images/spm01/testsetup.jpg)](/assets/images/spm01/testsetup.jpg)

## Operation

Being a Tuya branded device it is added to Smart Life app via a Tuya Zigbee gateway. I had to press the pairing button to enter pairing mode and after that proceeded to pair it with the Smart Life app. It was quickly discovered as the SPM01_1PN_Zigbee.

![App add](/assets/images/spm01/appadd.jpg)

There are a lot of options in the app besides power monitoring. There are alerts for everything and even an option for prepaid electricity users where you can manage the expenditure and get alerts to top up your account on time.

[![App device card](/assets/images/spm01/appcard.jpg){: width="33%"}](/assets/images/spm01/appcard.jpg)[![Device options](/assets/images/spm01/appoptions.jpg){: width="33%"}](/assets/images/spm01/appoptions.jpg)[![Prepaid monitoring](/assets/images/spm01/appcharge.jpg){: width="33%"}](/assets/images/spm01/appcharge.jpg)

Power monitoring works as advertised. I've hooked up a standard calibrated power meter in line and all the readings were spot on. I then reversed the wire to change electricity flow to test power production metering and that also works as it should.

## Integrations

### Tuya

Official Tuya integration in HA added the SPM01 with all the readings.

[![Tuya device card](/assets/images/spm01/tuya_device_card.jpg)](/assets/images/spm01/tuya_device_card.jpg)

### Zigbee2MQTT

It is completely supported in Zigbee2MQTT with all the options.

[![Zigbee2MQTT device card](/assets/images/spm01/z2mdevice.jpg){: width="50%"}](/assets/images/spm01/z2mdevice.jpg)[![Zigbee2MQTT exposes](/assets/images/spm01/z2mexposes.jpg){: width="50%"}](/assets/images/spm01/z2mexposes.jpg)

It integrated in Home Assistant via the MQTT integration.

[![Home Assistant device card for Zigbee2MQTT](/assets/images/spm01/z2m_device_card.jpg)](/assets/images/spm01/z2m_device_card.jpg)

### ZHA

Sadly there are no entities discovered in ZHA. A support request is opened, you can follow the [GitHub issue](https://github.com/zigpy/zha-device-handlers/issues/2463) and this article will be updated when support is added.

## Teardown

Can't end this review without cracking it open. There are 4 little clips on the back that needed a little bit of cajoling to separate the shell.

The device is deceptively simple, an AC to DC converter, the coil, Zigbee module, a button and the MCU that does all the calculation below.

[![Inside the device](/assets/images/spm01/inside.jpg){: width="50%"}](/assets/images/spm01/inside.jpg)[![Coil removed](/assets/images/spm01/coil.jpg){: width="50%"}](/assets/images/spm01/coil.jpg)

The Zigbee module is a standard [Tuya ZS2S](https://developer.tuya.com/en/docs/iot/zs2s-datasheet?id=K97s05sctcqvy) Zigbee module. Since its a standard footprint I expect the Wi-Fi version to have a CB2S or similar module 

Main MCU responsible for power monitoring calculations is a [Nuvoton MINI57TDE](https://www.nuvoton.com/products/microcontrollers/arm-cortex-m0-mcus/mini51-base-series/mini57tde/). 
![MCU](/assets/images/spm01/chip.jpg)

## Conclusion

This is a practical little device with versatile installation options. It can go from a whole house power meter to a lowly dryer power monitor. The power monitoring is very accurate and safe for high current applications because it just loops around the live wire and doesn't pass any current through the device itself. I'll be using this review sample as the whole house power monitor device.

It's main advantages are higher build quality and small size. You can buy it from:
- [Zemismart store](https://www.zemismart.com/products/spm01-d2tz-zm?DIST=QEVHGw%3D%3D) - use special code `4VBQF07F` for 10% discount
- [AliExpress](https://www.aliexpress.com/item/1005005589632247.html?aff_fcid=b12239cc38404a5e8dbcb585eed38a66-1690543806965-07541-_DmSpQML&tt=CPS_NORMAL&aff_fsk=_DmSpQML&aff_platform=shareComponent-detail&sk=_DmSpQML&aff_trace_key=b12239cc38404a5e8dbcb585eed38a66-1690543806965-07541-_DmSpQML&terminal_id=6db88f7b3fff4670be83ec2d245af448&afSmartRedirect=y)

## Wi-Fi and Three Phase Alternatives

If you're not into Zigbee there's a Wi-Fi version with an identical footprint and same features. Considering the Zigbee module footprint, the Wi-Fi version module can likely be replaced with an [ESP-02S](http://templates.blakadder.com/ESP-02S) or [ESP8685-WROOM-03](https://templates.blakadder.com/ESP8685-WROOM-03.html) to install Tasmota or ESPHome.

Available from:
- [Zemismart store](https://www.zemismart.com/products/spm01-d2tw-zm?DIST=QEVHGw%3D%3D)
- [AliExpress](https://www.aliexpress.com/item/1005005589632247.html?aff_fcid=b12239cc38404a5e8dbcb585eed38a66-1690543806965-07541-_DmSpQML&tt=CPS_NORMAL&aff_fsk=_DmSpQML&aff_platform=shareComponent-detail&sk=_DmSpQML&aff_trace_key=b12239cc38404a5e8dbcb585eed38a66-1690543806965-07541-_DmSpQML&terminal_id=6db88f7b3fff4670be83ec2d245af448&afSmartRedirect=y)

If you need a three phase power monitor, Zemismart has got you covered as well.

Available from:
- [Zemismart store Wi-Fi version](https://www.zemismart.com/products/spm02-d2tw?DIST=QEVHGw%3D%3D)
- [Zemismart store Zigbee version](https://www.zemismart.com/products/zemismart-tuya-zigbee-wifi-3-phase-electric-energy-meter-63a-smart-power-consumption-monitor-sensor-zigbee2mqtt-home-assistant?DIST=QEVHGw%3D%3D)
- [AliExpress both](https://www.aliexpress.com/item/1005005880071701.html?aff_fcid=ff87f27ed90f48848a3bfd7dce9bb6d7-1690544217930-05870-_Dl7XVB5&tt=CPS_NORMAL&aff_fsk=_Dl7XVB5&aff_platform=shareComponent-detail&sk=_Dl7XVB5&aff_trace_key=ff87f27ed90f48848a3bfd7dce9bb6d7-1690544217930-05870-_Dl7XVB5&terminal_id=6db88f7b3fff4670be83ec2d245af448&afSmartRedirect=y)