---
layout: post
title:  "Frankenstein Saturday"
author: blak
categories: [ Tasmota]
tags: [ diy, tasmota ]
image: assets/images/frankenstein_header.jpg
toc: true
---

What do you do when you have a box of unused devices? **Frankenstein 'em into something new!**

Some time ago I received a [Sonoff IW101](http://s.click.aliexpress.com/e/_dU9bwtx) switch for testing and discovering a [template](https://templates.blakadder.com/sonoff_IW101.html). 

Since I'm based in EU and the switch is made for US electrical standard it was relegated to the "box 'o stuff" but the fact its relay is rated to 16A 250V and the switch had built in power monitoring gave me an idea...

## The Idea

Why not take the guts of the switch and turn it into a power monitoring device? 

After disassembling the switch once again and checking the layout I noticed it has a fuse and the form factor is smaller than a Sonoff Basic. That meant is should neatly fit in a [Sonoff IP66 electrical box](http://s.click.aliexpress.com/e/_d9mwqAN) which, fortunately, I also had laying around. 

## The Build

I scrounged a 16A rated power cable from a broken power strip, got some wire terminals and the IP66 box. The cable socket was bought in a hardware store.

I removed the switch PCB from the IW101 board since its is not really needed here. IW101 board is secured with a small screw to the existing hole in the box which lined it up squarely in the middle. Perfect!

Everything fit neatly inside with some space to spare, much better than trying to jam a Sonoff POW R2 into that box. 

![](/assets/images/frankenstein_1.jpg)


Connected Lin and Lout to live wires, neutral had to be broken out to IW101 using some scrap wire and ground was simply bridged with a screw terminal.

![](/assets/images/frankenstein_2.jpg)

## The Configuration

Tasmota is already flashed on the device but the template had to be changed to remove the, now nonexistant, button and LEDs.

Set the relay to always on with `PowerOnState 4` and this way it can't be turned off randomly. That is fine since we're only interested in power monitoring.

Now just some [power monitoring calibration](https://tasmota.github.io/docs/Power-Monitoring-Calibration/) using a 100W incadescent bulb and a cheap power meter I got years ago.

![](/assets/images/frankenstein_3.jpg)

All that's left is configuring the topic and Device Name for it to be autodiscovered in Home Assistant and put to work as the dryer monitor.

## Finished Product

![](/assets/images/frankenstein.jpg)

Hopefully it won't burst into flames. ;)
