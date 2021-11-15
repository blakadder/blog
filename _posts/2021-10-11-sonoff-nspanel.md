---
layout: post
title: "Sonoff NSPanel Smart Scene Wall Switch Review"
author: blak
categories: [ review ]
tags: [ ]
image: assets/images/header_nspanel.jpg
toc: true
---

Next level in your smart home

_**Full disclosure:** This is a pre production unit sent to me free of charge by [Sonoff](https://www.anrdoezrs.net/links/100155210/type/dlg/https://www.itead.cc/). Some aspects of it might be changed once it reaches production. Review is not influenced by that fact and is solely my opinion. Shopping links in this article are affiliate links and I earn a small commission when you buy through them_

Sonoff contacted me in August with an intriguing offer to test a touch-screen control switch. I was not about to decline that although my expectations were not that high, to be honest.

![Packaging](/assets/images/nspanel/packaging.jpg)

The NSPanel got here in a matter of days and I eagerly opened it up. Once I took it out of the box I literally yelped. They put a screen on a double switch. So simple, yet so brilliant. Retaining the classic switches greatly increases usability and are more intuitive to less technically inclined users. The touch screen is here as a bonus, to give you additional information and control over other devices.

Sonoff NSPanel is available through [Kickstarter](https://www.kickstarter.com/projects/sonoffnspanel/sonoff-nspanel-smart-scene-wall-switch?ref=4dli2n) for ~~50USD~~ 54$ as a backer discount. Expected MSRP once the campaign is finished is 75$.

**Update:**    
Kickstarter campaign is finished and funded.    
Sonoff NSPanel is now available only through [Itead store](https://itead.cc/product/sonoff-nspanel-smart-scene-wall-switch/?utm_source=Youtube+Influencer&utm_medium=Sasa+Milicevic&utm_campaign=NSPanel+Review).

## Hardware
The EU version of NSPanel is sized as a standard EU switch made to fit 60mm electrical boxes. The touchscreen plate with a striking metallic grey design is detachable from the power supply and relay part.

![Front](/assets/images/nspanel/front.jpg)

At the front you see a touch screen sporting roomy 3.5 inches albeit with quite thick bezels and two switches. Right side sports the Sonoff logo and the bottom part houses a temperature sensor and a reset hole.

![Logo](/assets/images/nspanel/logo.jpg)
![Bottom](/assets/images/nspanel/bottom.jpg)

The buttons at the bottom are large enough but the sample I got was very mushy with not a lot of feedback and it has to be pressed exactly in the middle. I informed Sonoff of this shortcoming and they promised the final design will improve on that.

The back of the unit is mounted in the wall and houses the power supply unit and two 10A relays. Despite that, NSPanel is rated only to 2A per relay or 4A combined which is a bit strange, more so because it has a thermostat feature, but more on that one later... 

![Boot Image](/assets/images/nspanel/bootimg.jpg)

Turning on the screen I'm greeted by the boot animation and then a nice looking GUI.

![GUI](/assets/images/nspanel/gui.jpg)

Viewing angles of the screen are quite good and it stays readable when looking at it from above or from the sides, which is an important factor since it will likely be mounted at a position that is not at eye level.

![Viewing Angles](/assets/images/nspanel/angles.jpg)

## Software

To fully utilize the NSPanel's features you need to configure it in eWeLink app. Once you do it will grab your location, sync the clock to your local time zone and display the weather forecast.

At the bottom are two lines that light up when the corresponding relay is on. While the switches are right there it would be nice to be able to control the relays through the touch screen as well.

To navigate the touch UI you use gestures. Swipe down brings you to settings where you can change screen brightness and sleep period.

![Settings](/assets/images/nspanel/settings.jpg)

Swiping left or right cycles through the main screen, widgets and thermostat menu.

![Widgets](/assets/images/nspanel/widgets.jpg)

Widgets are control icons for other supported eWeLink devices or scenes configure in the app. To add them go to NSPanel - Settings - Widgets menu in the app and add them. So far widgets donot support any of the Sonoff Zigbee devices directly. You could set up scenes to control the Zigbee devices but it really should be included in the app from the get go.

Once the widgets are added you can tap them to bring up the full control menu or simply activate it if its a scene. I wish there would be an option to long press a widget to toggle it instead of cumbersomely clicking each one then clicking its own toggle then navigating back to the widget screen.

![Bulb Widget](/assets/images/nspanel/bulb.jpg)

Every type of device will have on/off toggle then additional options depending on features. My Sonoff filament bulb has a color temperature and brightness slider.

The thermostat feature is an odd one. It is supposed to use the built in temperature sensor to control the built in relays. But the switch is rated to 2A only which would a very weak heater. I'm quite sure this is put here as a selling point with complete disregard for the fact you are not supposed to hook up a space heater or, worse, a boiler to it.

There is a distinct lack of any real customization. While I don't mind being locked in to the GUI theme and design I feel there should be some options to at least customize the main screen. Adding a couple favorite widgets would also be welcome.

Since it is a smart switch you can control all the features from the app as well and you get additional options not available from the screen settings such as: timers, power-on state, relay inching and interlocking, temperature unit change, timed energy saving mode for the screen.

![App](/assets/images/nspanel/app.jpg)

NSPanel works with Amazon Alexa or Google Home but so far all I can control is the two relays, no sign of a thermostat.

There is a LAN control toggle in the setting, so I have hopes it will be possible to integrate it in various smart home solutions in the future

![Lan](/assets/images/nspanel/lan.jpg)

## Conclusion
After the initial euphoria, caused by the striking and novel design, died down I'm left looking at some very nice hardware with average software that's not yet as feature rich as a user would wish. Granted, NSPanel is technically still in pre-production and there's been two firmware updates since I got the NSPanel and software can be improved gradually. Therefore I expect constant improvements and building a usable GUI is not as easy as you would think.

Being locked into the eWeLink ecosystem is another minus in my eyes since most users tend to have a mix of devices. I wish there would be an option to bridge control, at least by using scenes.

If you like what you saw hurry over to [Kickstarter](https://www.kickstarter.com/projects/sonoffnspanel/sonoff-nspanel-smart-scene-wall-switch?ref=4dli2n) and get Sonoff NSPanel for 50$ or even less as a backer.

Oh, did I mention there's an ESP32 inside and the future is looking bright for an alternative solution...