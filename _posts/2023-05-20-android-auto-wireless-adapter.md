---
layout: post
title: "Android Auto Wireless Adapter Review and Guide"
author: blak
categories: [ review, auto ]
tags: [ ]
image: assets/images/header_android-auto.jpg
toc: true
---

Convert your old car's wired Android Auto to wireless with no effort!

_**Full disclosure:** This device is purchased with the help of your donations. Links in this article are affiliate links. By purchasing through them I also get a small commission that funds future projects!_

Android Auto Wireless Adapter can be found at:

- [AliExpress](https://www.aliexpress.com/item/1005005340640887.html?aff_platform=portals-tool&sk=_dYc72Dj&aff_trace_key=ad46a8783ef8414cb3077511738cc481-1595436156311-00915-_dYc72Dj&terminal_id=3ac645b4aa5741e4bebe6d5c100f96fc&tmLog=new_Detail&aff_request_id=ad46a8783ef8414cb3077511738cc481-1595436156311-00915-_dYc72Dj)
- [Amazon US](https://www.amazon.com/Wireless-Android-Adapter-Factory-Converts/dp/B0BXFSC8N7?&linkCode=ll1&tag=blakadders-20&linkId=fdd1337f29f1722704d5a6e541042464&language=en_US&ref_=as_li_ss_tl)
- [Amazon CA](https://www.amazon.ca/Android-Wireless-Converter-Receiver-Compatible/dp/B0BVMT2LCF?&linkCode=ll1&tag=tasmotatemp03-20&linkId=39eb1c7bc410bcb459646ce07fd74ea8&language=en_CA&ref_=as_li_ss_tl)
- [Amazon DE](https://www.amazon.de/-/en/Pomrone-Wireless-Android-Multimedia-Hands-Free-black/dp/B0BXN6PY7G?&linkCode=ll1&tag=blakadders-20&linkId=77d373ae698e2d45ea4c7631dbfc8d2f&language=en_GB&ref_=as_li_ss_tl)
- [Amazon UK](https://www.amazon.co.uk/Android-Wireless-Multi-function-Converts-Gesuter-Black/dp/B0BXN7HS5C?&linkCode=ll1&tag=blakadders-20&linkId=4b91766c1df3bec50b49c145c977b044&language=en_GB&ref_=as_li_ss_tl)
- [Amazon FR](https://www.amazon.fr/MASAYA-Adaptateur-Connexion-Automatique-2016-2022y/dp/B0BVZDVMKB?&linkCode=ll1&tag=blakaddertemp-21&linkId=c867b3c581deb11b0e9d4e6eaba07c2d&language=fr_FR&ref_=as_li_ss_tl)

I randomly found this small dongle to convert your car's wired Android Auto to wireless and it looked like a cheap and good solution. I was looking for one since my car only has wired Android Auto and plugging in the phone for shorter trips isn't practical at all.

I ordered one for 36€, shipped from France.

![Product image](/assets/images/android-auto/product_image.jpg){: width="50%"}

It arrived in a week in a plain, unbranded box.

[![The box](/assets/images/android-auto/box.jpg){: width="50%"}](/assets/images/android-auto/box.jpg)

You get the dongle, a USB-C to USB-A adapter, 3M double sided tape and the instructions leaflet. 

[![Contents of the box](/assets/images/android-auto/contents.jpg){: width="50%"}](/assets/images/android-auto/contents.jpg)

The adapter shell and the USB cable are made from a nice rubbery soft touch plastic.

## Installation and Use

The dongle is extremely easy to install. It doesn't require any additional apps except from Android Auto being installed on your phone. All you have to do is plug the adapter into the USB port for Android Auto and activate Bluetooth and Wi-Fi on your phone.

![Adapter plugged into car](/assets/images/android-auto/plugin.jpg){: width="50%"}

Pair your phone to the `smartBox-xxxx` Bluetooth device. Once paired, the phone will connect to the `smartBox-xxxx` access point and the notification that Android Auto is available will pop up on your phone.

![Pairing screen](/assets/images/android-auto/pairbt.jpg){: width="50%"}![Pairing screen](/assets/images/android-auto/pairbt2.jpg){: width="50%"}

You're now connected to you car's Android Auto via 5Ghz Wi-Fi and you can use Android Auto as normal.

![Wi-Fi connection](/assets/images/android-auto/wificonnection.jpg){: width="50%"}![Connected to Android Auto](/assets/images/android-auto/connected.jpg){: width="50%"}

Your phone will now automatically connect to the adapter via Wi-Fi every time the smartBox Bluetooth device is discovered. Connecting to Android Auto takes only a few seconds after Bluetooth connects.

Yup, that easy!

You can pair multiple phones to the adapter but it will only automatically connected to the last phone used. Other phones have to connect via Bluetooth manually first.

## How Does it Work?

Cracking it open reveals the adapter is based on an Allwinner [V831](http://www.sochip.com.cn/v831/index.php?title=What_is_V831_%3F) SoC. Interestingly enough, that SoC is designed and intended for use in cameras. It is a single-core ARM Cortex-A7 with supported frequencies up to 800MHz.

[![PCB front](/assets/images/android-auto/pcb_front.jpg){: width="50%"}](/assets/images/android-auto/pcb_front.jpg)[![PCB back](/assets/images/android-auto/pcb_back.jpg){: width="50%"}](/assets/images/android-auto/pcb_back.jpg)

Next to it is what looks like a Wi-Fi module, most likely responsible for both Bluetooth and Wi-Fi connectivity.

[![V831 SoC](/assets/images/android-auto/chip.jpg){: width="50%"}](/assets/images/android-auto/chip.jpg)[![V831 SoC](/assets/images/android-auto/wifi.jpg){: width="50%"}](/assets/images/android-auto/wifi.jpg)

There is also the peculiar button at the bottom right that isn't possible to activate from the outside. 

When you connect the device to a Windows computer and pair your phone to it, a new device named "smartlinkBox" will appear. The device is shown as "Android Accessory Interface" in Device Manager.

![smartlinkBox device](/assets/images/android-auto/smartbox.jpg)![Device manager device](/assets/images/android-auto/devicemanager.jpg)


The device small 3.74 Megabyte disk drive with some files on it. The files `bt_conf.ini` holds the mac addresses and names of the phones previously paired to it.

![Files on the device](/assets/images/android-auto/files.jpg)

I assume the device emulates an Android device and simply bridges the USB data to TCP packets over the Wi-Fi connection established with your phone. Simple solution but it just works.

## Final Words

If you have an older car with just wired Android Auto this is a cheap and elegant way to upgrade it. I love it and couldn't live without it anymore. Sometimes you just find the easy solution to an issue that was bothering you for a long time.

Android Auto Wireless Adapter can be found at:

- [AliExpress](https://www.aliexpress.com/item/1005005340640887.html?aff_platform=portals-tool&sk=_dYc72Dj&aff_trace_key=ad46a8783ef8414cb3077511738cc481-1595436156311-00915-_dYc72Dj&terminal_id=3ac645b4aa5741e4bebe6d5c100f96fc&tmLog=new_Detail&aff_request_id=ad46a8783ef8414cb3077511738cc481-1595436156311-00915-_dYc72Dj)
- [Amazon US](https://www.amazon.com/Wireless-Android-Adapter-Factory-Converts/dp/B0BXFSC8N7?&linkCode=ll1&tag=blakadders-20&linkId=fdd1337f29f1722704d5a6e541042464&language=en_US&ref_=as_li_ss_tl)
- [Amazon CA](https://www.amazon.ca/Android-Wireless-Converter-Receiver-Compatible/dp/B0BVMT2LCF?&linkCode=ll1&tag=tasmotatemp03-20&linkId=39eb1c7bc410bcb459646ce07fd74ea8&language=en_CA&ref_=as_li_ss_tl)
- [Amazon DE](https://www.amazon.de/-/en/Pomrone-Wireless-Android-Multimedia-Hands-Free-black/dp/B0BXN6PY7G?&linkCode=ll1&tag=blakadders-20&linkId=77d373ae698e2d45ea4c7631dbfc8d2f&language=en_GB&ref_=as_li_ss_tl)
- [Amazon UK](https://www.amazon.co.uk/Android-Wireless-Multi-function-Converts-Gesuter-Black/dp/B0BXN7HS5C?&linkCode=ll1&tag=blakadders-20&linkId=4b91766c1df3bec50b49c145c977b044&language=en_GB&ref_=as_li_ss_tl)
- [Amazon FR](https://www.amazon.fr/MASAYA-Adaptateur-Connexion-Automatique-2016-2022y/dp/B0BVZDVMKB?&linkCode=ll1&tag=blakaddertemp-21&linkId=c867b3c581deb11b0e9d4e6eaba07c2d&language=fr_FR&ref_=as_li_ss_tl)

Take note that Amazon prices are significantly higher than AliExpress.
