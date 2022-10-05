---
layout: post
title: "Use Proximity Sensor to Wake Screen on Android Smart Home Panels"
author: blak
categories: [ how-to, android, T6E ]
tags: [ ]
image: assets/images/header_android-panel-proximity.jpg
toc: true
---

Use proximity sensor for actions (f.e. screen wake up) on T6E based multi-functional control panels running on Android (NSPanel Pro and Tuya)

_Shopping links in this article are affiliate links and I earn a small commission when you buy through them_

{% include T6E.html %}

These panels have a light and proximity sensor in top bezel of the display panel but the proximity sensor is misconfigured and cannot not wake the device from sleep when the default panel app is not active. 

Considering it works as expected using the default panel app I determined to discover a solution. After trying standard wave to wake apps and combing through the driver files I learned that the proximity sensor max range is lower than the display shown when its mounted in the case. After I took the proximity sensor out it worked as it should. The simplest solution was drill a hole in the front. Simplest in this case is not the best solution. I put out a call on twitter for some help from any Android developer but that didn't go far either.

Determined to find the solution I set on the hunt for that one perfect app. The journey was arduous and fraught with failure but in the end my google fu dug up the perfect solution:

## Enter Automagic

[Automagic](https://automagic4android.com/) is an automation app, reminiscent of Tasker, with many triggers for automations or, as they're called in the app, flows. Most importantly it can access proximity and light sensor values which is exactly what is needed

After Google kicked it off the Play store for being too awesome its development has stopped but the author graciously allowed downloads from the official site. You even get the premium version!

## Installation

You need to have the ability to [sideload apps](/android-panel-webview) to your panel via ADB.

Download the newest version of [Automagic](https://automagic4android.com/download_en.html).

Install the apk. Command via ADB:

```sh
adb install Automagic_1_38_0.apk
```

Open the app and read the... I mean: Skip everything cause we don't need no damn tutorial!

You can delete all the included flows and start fresh.

## Proximity Flow

Download the [flow for proximity sensor](/files/flow_Proximity_turn_screen_on.xml) file to your panel

```sh
adb push flow_Proximity_turn_screen_on.xml /sdcard/Download/flow_Proximity_turn_screen_on.xml
```

Import it by clicking the hamburger icon and select "Import Flows/Widgets".

![Import Flow](/assets/images/android-panel-proximity/importflow.png)

Navigate to "Downloads" folder and select the flow file.

![Select Flow](/assets/images/android-panel-proximity/file.png)

This is how the flow looks. You can pinch zoom if its too small for you.

![Flow](/assets/images/android-panel-proximity/flow.png)

Adjust the proximity distance trigger to be above the value shown when nothing is in front of the panel.

![Set distance](/assets/images/android-panel-proximity/distance.png)

Adjust screen wake options if you wish.

![Screen Duration](/assets/images/android-panel-proximity/screen_duration.png)

Turn the flow on. If asked allow permissions required by the app.

If everything is set up correctly the flow with turn red when you trigger the proximity sensor.

![Flow working](/assets/images/android-panel-proximity/flowworking.png)

Go to ***Settings -> Display -> Advanced -> Sleep*** and set the timeout to turn the screen off.

And now you can simply walk up to the panel or do the Jedi wave for the screen to turn on

## Don't Stop There

You can make this basic flow more complex with various conditions to turn it on dimmer during the night and so on.

Also make sure to explore other automation possibilities of this very capable app, such as the sound level trigger!
