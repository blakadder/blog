---
layout: post
title: "Ulanzi Desktop Pixel Clock on Esphome"
author: blak
categories: [ esphome, diy ]
tags: [ ]
image: assets/images/header_ulanzi-esphome.jpg
toc: true
---

Guide on installing Esphome on Ulanzi TC001 pixel clock I [reviewed](ulanzi-pixel-clock) and making it work with Home Assistant.

_**Full disclosure:** This device is purchased with the help of your donations. Links in this article are affiliate links. By purchasing through them I also get a small commission that funds future projects!_

Ulanzi Smart Pixel Clock is available from the following stores:
- [Ulanzi USA](https://www.ulanzi.com/products/ulanzi-pixel-smart-clock-2882?aff=800)
- [Ulanzi Germany](https://www.ulanzi.de/products/ulanzi-pixel-smart-uhr-2882?ref=blakadder)
- [Aliexpress](https://www.aliexpress.com/item/1005005034439849.html?aff_fcid=d5a66d494fcb4a53b1eba0e6d34f656c-1677795402854-08330-_DkwGxYt&tt=CPS_NORMAL&aff_fsk=_DkwGxYt&aff_platform=shareComponent-detail&sk=_DkwGxYt&aff_trace_key=d5a66d494fcb4a53b1eba0e6d34f656c-1677795402854-08330-_DkwGxYt&terminal_id=3f8c776975fd455ba956809c02d71a91&afSmartRedirect=y)
- [Amazon](https://www.amazon.com/dp/B0BS8Q9749?tag=blakaddertemp-20)

In my internet searches for everything Ulanzi and Awtrix pixel clock related I found lubeda's [awtrix python script](https://github.com/lubeda/awtrix_python_script) for Home Assistant. I checked out lubeda's profile, as you usually do as a low key internet stalker, and a repository called [EsphoMaTrix](https://github.com/lubeda/EsphoMaTrix) was on the list. I don't know if it was the camel case of the title or the description "A simple DIY status display with an 8x32 RGB LED panel implemented with esphome.io" that drew me in. Well, I know some Esphome and I like tight Home Assistant integrations so I put it on the "to try out" list. Meanwhile I also got a [Twitter ping](https://twitter.com/tompower31/status/1612856531632164864?s=20) mentioning the same project. 

Almost two months later I got to try it out and, lo and behold, there was even support of Ulanzi TC001, albeit in an unfinished stage. Sometimes you unintentionally wait and others do the bulk of the work for you.

This will be a great solution for a Home Assistant or standalone user, if you're not on of those you might be better off trying out [PixelIt_Ulanzi](https://github.com/aptonline/PixelIt_Ulanzi) which was also mentioned to me on Twitter a couple of times.

## Installation

First you need to have [Esphome](https://esphome.io/). If you don't, install it using the appropriate method for your situation. For me it was a Docker container on my small Celeron J3455 home server.

Open Esphome dashboard and start a new project. Paste the yaml.

{% raw %}
```yaml
substitutions:
  devicename: ulanzi
  matrix_pin: GPIO32 
  buzzer_pin: GPIO15
  battery_pin: GPIO34 
  ldr_pin: GPIO35 
  left_button_pin: GPIO26 
  mid_button_pin: GPIO27 
  right_button_pin: GPIO14 
  scl_pin: GPIO22 
  sda_pin: GPIO21 

external_components:
  - source:
      type: git
      url: https://github.com/lubeda/EsphoMaTrix
    refresh: 60s 
    components: [ ehmtx ]   

esphome:
  comment: "Ulanzi TC001"
  name: $devicename 
  on_boot:
    then:
      - ds1307.read_time:
  
esp32:
  board: esp32dev

ota:

wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password

web_server:

font: 
  # adapt the filename to your local settings
  - file: Calcium.ttf
    id: ehmtx_font
    size: 16
    glyphs:  |
      !?"%‰()+*=,-_.:°µ²³0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ abcdefghijklmnÖÄÜöäüopqrstuvwxyz€$@<>/

ehmtx:
  id: rgb8x32
  display8x32: ehmtx_display
  html: true
  show_clock: 5           # duration to display the clock after this time the date is display until next "show_screen"
  clock_interval: 60      # show the clock at least each x seconds
  show_screen: 6          # duration to display a screen or a clock/date sequence
  duration: 5             # lifetime of a screen in minutes
  date_format: "%d.%m"    # defaults "%d.%m." (use "%m.%d." for the US)
  time_format: "%H:%M"    # defaults "%H:%M" (use "%I:%M%p" for the US)
  dayofweek: false        # draw the day indicator on the bottom of the screen, defaults to true
  show_date: true         # show the date for show_screen - show_clock seconds otherwise only shows the clock for show_screen seconds, defaults to true
  week_start_monday: true # default monday is first day of week, false = sunday
  yoffset: 8              # the text is aligned BASELINE_LEFT, the baseline defaults to 6
  xoffset: 1              # the text is aligned BASELINE_LEFT, the left defaults to 1
  scroll_intervall: 80    # the interval in ms to scroll the text (default=80), should be a multiple of the update_interval from the display (default: 16ms)
  anim_intervall: 192     # the interval in ms to display the next anim frame (default=192), should be a multiple of the update_interval from the display (default: 16ms)
  time: ehmtx_time
  font_id: ehmtx_font
  icons: 
    - id: ha
      lameid: 7956
    - id: tempc
      lameid: 2422
    - id: plug
      lameid: 403
    - id: humidity
      lameid: 51764
    - id: co2
      lameid: 30662
      

binary_sensor:
  - platform: status
    name: "$devicename Status"
  - platform: gpio
    pin:
      number: $left_button_pin
      inverted: true
    name: "$devicename left button"
    on_press:
      then:
        - number.decrement: screen_brightness
  - platform: gpio
    pin: 
      inverted: true
      number: $mid_button_pin
      mode: INPUT_PULLUP
    name: "$devicename middle button"
    on_press:
      then:
        - switch.toggle: displaycontrol
  - platform: gpio
    pin: 
      number: $right_button_pin
      inverted: true
    name: "$devicename right button"
    on_press:
      then:
        - number.increment: screen_brightness
# example to switch to next screen
#        lambda: |-
#          id(rgb8x32)->skip_screen();
logger:
  level: INFO

# Enable Home Assistant API
api:
  services:
    - service: gauge
      variables:
        val: int
      then:
        lambda: |-
          id(rgb8x32).set_gauge_value(val);
    - service: alarm
      variables:
        icon_name: string
        text: string
      then:
        lambda: |-
          id(rgb8x32)->add_screen(icon_name,text,7,true);
          id(rgb8x32)->force_screen(icon_name);
    - service: screen
      variables:
        icon_name: string
        text: string
      then:
        - ehmtx.add.screen:
            id: rgb8x32
            text: !lambda return text;
            icon_name: !lambda return icon_name;
            alarm: false
    - service: brightness
      variables:
        brightness: int
      then:
        lambda: |-
          id(rgb8x32)->set_brightness(brightness);
    - service: status
      then:
        lambda: |-
          id(rgb8x32)->get_status();
    - service: del_screen
      variables:
        icon_name: string
      then:
        - ehmtx.delete.screen:
            id: rgb8x32
            icon_name: !lambda return icon_name;
    - service: indicator_on
      variables:
        r: int
        g: int
        b: int
      then:
        - ehmtx.indicator.on:
            id: rgb8x32
            red: !lambda return r;
            green: !lambda return g;
            blue: !lambda return b;
    - service: force_screen
      variables:
        icon_name: string
      then:
        - ehmtx.force.screen:
            id: rgb8x32
            icon_name: !lambda return icon_name;
    - service: text_color
      variables:
        r: int
        g: int
        b: int
      then:
        lambda: |-
          id(rgb8x32)->set_text_color(r,g,b);
    - service: alarm_color
      variables:
        r: int
        g: int
        b: int
      then:
        lambda: |-
          id(rgb8x32)->set_alarm_color(r,g,b);
    - service: indicator_off
      then:
        - ehmtx.indicator.off:
            id: rgb8x32
    # - service: display_on
    #   then:
    #     - ehmtx.display.on:
    #         id: rgb8x32
    # - service: display_off
    #   then:
    #     - ehmtx.display.off:
    #         id: rgb8x32
    - service: icons
      then:
        lambda: |-
          id(rgb8x32)->show_all_icons();
    - service: skip_screen
      then:
        lambda: |-
          id(rgb8x32)->skip_screen();
    - service: tune
      variables:
        tune: string
      then:
        - rtttl.play:
            rtttl: !lambda 'return tune;'
number:
  - platform: template
    name: "$devicename brightness"
    id: screen_brightness
    min_value: 0
    max_value: 255
    update_interval: 1s
    step: 25
    lambda: |-
      return id(rgb8x32)->get_brightness();
    set_action:
      lambda: |-
        id(rgb8x32)->set_brightness(x);

switch:
  - platform: template
    name: "$devicename Display"
    id: displaycontrol
    icon: "mdi:power"
    restore_mode: ALWAYS_ON
    lambda: |-
      return id(rgb8x32)->show_display;
    turn_on_action:
      lambda: |-
        id(rgb8x32)->set_display_on();
    turn_off_action:
      lambda: |-
        id(rgb8x32)->set_display_off();

sensor:
  - platform: sht3xd
    temperature:
      name: "$devicename Temperature"
    humidity:
      name: "$devicename Humidity"
    update_interval: 60s
  - platform: adc
    pin: $battery_pin
    name: "$devicename Battery"
    id: battery_voltage
    update_interval: 10s
    device_class: battery
    accuracy_decimals: 0
    attenuation: auto
    filters:
      - sliding_window_moving_average:
          window_size: 15
          send_every: 15
          send_first_at: 1
      - multiply: 1.6
      - lambda: |-
          return ((x - 3) / 0.69 * 100.00);
    unit_of_measurement: '%'
  - platform: adc
    id: light_sensor
    name: "$devicename Illuminance"
    pin: $ldr_pin
    update_interval: 10s
    attenuation: auto
    unit_of_measurement: lx
    accuracy_decimals: 0
    filters:
      - lambda: |-
          return (x / 10000.0) * 2000000.0 - 15 ;
    
output:
  - platform: ledc
    pin: $buzzer_pin
    id: rtttl_out

rtttl:
  output: rtttl_out

i2c:
  sda: 21
  scl: 22
  scan: true
  id: bus_a

light:
  - platform: neopixelbus
    id: ehmtx_light
    type: GRB
    variant: WS2812
    pin: $matrix_pin
    num_leds: 256
    color_correct: [30%, 30%, 30%]
    name: "$devicename Light"
    restore_mode: ALWAYS_OFF
    on_turn_on:
      lambda: |-
         id(ehmtx_display)->set_enabled(false);
    on_turn_off:
       lambda: |-
         id(ehmtx_display)->set_enabled(true);

time:
  - platform: homeassistant
    on_time_sync:
      then:
        ds1307.write_time:
  - platform: ds1307
    update_interval: never
    id: ehmtx_time
  
display:
  - platform: addressable_light
    id: ehmtx_display
    addressable_light_id: ehmtx_light
    width: 32
    height: 8
    pixel_mapper: |-
      if (y % 2 == 0) {
        return (y * 32) + x;
      }
      return (y * 32) + (31 - x);
    rotation: 0°
    update_interval: 16ms
    auto_clear_enabled: true
    lambda: |-
      id(rgb8x32)->tick();
      id(rgb8x32)->draw();{% endraw %}
```

Before installing download the [Calcium font](https://www.pentacom.jp/pentacom/bitfontmaker2/gallery/?id=13577) and place it in the same location as the configuration yaml. Connect the Ulanzi via the USB-C port to the machine with Esphome. Click install and choose "Plug into.." and wait for it to complete.

If that is not possible you can choose "Manual download", grab the built binary and flash it manually with esptool.py or [ESPHome-Flasher](https://github.com/esphome/esphome-flasher).

![esphome?](/assets/images/ulanzi-tc001/esphome.gif)

This is just the skeleton configuration, you need to add (animated) icons you're going to use. Or do a deep dive and adapt it how you want it, it's just Esphome.

## Buttons and Sensors

Buttons are configured to change brightness using left and right buttons and turn the display on or off with the middle button.

![Sensors](/assets/images/ulanzi-tc001/sensors.jpg)

Initial configuration had raw ADC sensors for battery and voltage. I've set them up as a lux and battery percentage sensors to the best of my abilities. Can't say they're accurate since the "calibration" was a trial and error job but they look to be in the ballpark.

## Icons

You can add icons and animated gifs via a file or url. Esphome will try to downscale the static images to 8x8 but the icons don't turn out great most of the time. The easier way is to use the Lametric Developer website and poach from their library of thousands of appropriately sized icons, many of them animated.

Create an account, go to Create new app and click Select icon under Look and Feel and find the ones you want to use.

There's a somewhat hidden feature in EsphoMaTrix to pull the icons directly just by its LameTric numeric id:

```yaml
  icons: 
    - id: ha
      lameid: 7956
```

I don't know for how long will this be possible and I strongly recommend to download the icons you intend to use and save them locally. Add the icon number to the url https://developer.lametric.com/content/apps/icon_thumbs/ and download it to the same folder as the configuration yaml file. Use with:


```yaml
  icons: 
    - id: batman
      file: batman.gif
```

## Fonts

To have good readable fonts you should search for pixel fonts. I've found a great resource called Bitfontmaker2 with a large selection of fonts with open licenses like public domain or Creative Commons. Bitfontmaker2 also lets you create your own font or edit the existing one to add characters you might be missing.

[![Font1](/assets/images/ulanzi-tc001/font1.jpg){: width="50%"}](/assets/images/ulanzi-tc001/font1.jpg)[![Font2](/assets/images/ulanzi-tc001/font2.jpg){: width="50%"}](/assets/images/ulanzi-tc001/font2.jpg)[![Font3](/assets/images/ulanzi-tc001/font3.jpg){: width="50%"}](/assets/images/ulanzi-tc001/font3.jpg)[![Font4](/assets/images/ulanzi-tc001/font4.jpg){: width="50%"}](/assets/images/ulanzi-tc001/font4.jpg)

Not all fonts will work but there's a higher chance to find a suitable one here than on the random free font repositories. 

Download the font and save it to the same folder as the configuration yaml file and change the filename in the yaml. 

```yaml
font: 
  - file: fonts/BetterPixels.ttf
    id: ehmtx_font
    size: 16
    glyphs:  |
      !?"%()+*=,-_.:°0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ abcdefghijklmnÖÄÜöäüopqrstuvwxyz€$@<>/
```

Some fonts will require different sizes, most likely 8 or 12 and many fonts will just be too big to display properly. Some fonts will fit only with uppercase letters.

Depending on the font size and shape you might need to raise or lower `yoffset:` to make the entire font visible. 

The `glyphs:` key just tells Esphome which characters will be saved to flash memory and used by the display. If a character is missing from the font with a defined glyph it will be displayed as a white outlined square. If a glyph isn't defined that charcter it will be displayed as a white block. Adjust according to your use and font's capabilities. Fonts on Bitfontmaker2 have a GlyphMap link to display all available characters.

I settled on Calcium because it looks nice and has a lot of included glyphs. Other contenders were [Awesome](https://www.pentacom.jp/pentacom/bitfontmaker2/gallery/?id=13808) as a large font or, appropriately named, [smallfonts with symbols](https://www.pentacom.jp/pentacom/bitfontmaker2/gallery/?id=608) or [DMD-Small](https://www.pentacom.jp/pentacom/bitfontmaker2/gallery/?id=5993) as small font options. Small fonts are useful if you have day_of_week indicator enabled, make sure to set `yoffset: 6`. Honorable mention goes to [Fallout], font used in my favourite games Fallout 1 and 2


## Services

Many of the basic EsphoMaTrix services are in the configuration. Read more about [Services](https://github.com/lubeda/EsphoMaTrix/blob/main/README.md#services).

There's a new service, particular to the Ulanzi TC001: `tune` service which plays RTTTL tunes on the built in buzzer. 

RTTTL or Ring Tone Text Transfer Language was used to transfer ringtones to Nokia cellphones. Some of you will remember the ubiquitous Nokia ringtone in the late 90's and nowadays you can play it directly from Home Assistant as a notification on your pixel clock.

[![Open your Home Assistant instance and show your service developer tools with a specific service selected.](https://my.home-assistant.io/badges/developer_call_service.svg)](https://my.home-assistant.io/redirect/developer_call_service/?service=ulanzi_tuneplay)

find service ESPHome devicename_tuneplay and enter

```yaml
service: esphome.ulanzi_tuneplay
data: 
  tune: 'NokiaTun:d=4,o=5,b=225:8e6,8d6,f#,g#,8c#6,8b,d,e,8b,8a,c#,e,2a'
```
![Tuneplay](/assets/images/ulanzi-tc001/tuneplay.jpg)

As with any other service you can use it in an automation.

There's more than ten thousand melodies to choose from downloadable from [Picaxe](https://picaxe.com/rtttl-ringtones-for-tune-command/).

## Example Automation

I've just gone ahead and used the ehmtk [README.md] example because it's simple and effective.

```yaml
{% raw %}alias: Ulanzi Display Stuff
description: "Create screens on Ulanzi TC001"
trigger:
  - platform: state
    entity_id: sensor.temptest_pms5003_pm2_5
    id: pm25
  - platform: state
    entity_id: sensor.temptest_2_sht4x_temperature
    id: tempc
  - platform: state
    entity_id: sensor.temptest_mhz19b_carbondioxide
    id: co2
condition: []
action:
  - service: esphome.ulanzi_screen
    data:
      icon_name: "{{trigger.id}}"
      text: >-
        {{ trigger.to_state.state }}{{ trigger.to_state.attributes.unit_of_measurement }}
mode: queued
max: 10{% endraw %}
```

To make this work you have to configure icons in yaml with id's identical to the trigger id used. 

What happens is this:

First you need to configure an icon with `icon_name: pm25`. Then, when the trigger with the id "pm25" happens, Home Assistant calls the `_screen` service with `icon_name: pm25` (identical to the trigger id). This will create a screen `pm25` with the icon configured and then insert the value and unit of measurement of that sensor.

![Final product](/assets/images/ulanzi-tc001/finalproduct.gif)

## Possibilities

This were just basic examples. Read more on all the services you can call from Home Assistant and different displays EsphoMaTrix has in their [README](https://github.com/lubeda/EsphoMaTrix/blob/main/README.md#use-in-home-assistant-automations).

Ulanzi Smart Pixel Clock is available from the following stores:
- [Ulanzi USA](https://www.ulanzi.com/products/ulanzi-pixel-smart-clock-2882?aff=800)
- [Ulanzi Germany](https://www.ulanzi.de/products/ulanzi-pixel-smart-uhr-2882?ref=blakadder)
- [Aliexpress](https://www.aliexpress.com/item/1005005034439849.html?aff_fcid=d5a66d494fcb4a53b1eba0e6d34f656c-1677795402854-08330-_DkwGxYt&tt=CPS_NORMAL&aff_fsk=_DkwGxYt&aff_platform=shareComponent-detail&sk=_DkwGxYt&aff_trace_key=d5a66d494fcb4a53b1eba0e6d34f656c-1677795402854-08330-_DkwGxYt&terminal_id=3f8c776975fd455ba956809c02d71a91&afSmartRedirect=y)
- [Amazon](https://www.amazon.com/dp/B0BS8Q9749?tag=blakaddertemp-20)

I'm looking forward to see what you've accomplished with this powerful component for Esphome. Share your screens and tag me on [Twitter](https://twitter.com/blakadder_) or [Instagram](https://instagram.com/blak_adder).
