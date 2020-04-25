---
layout: post
title:  "Smarter Coffee"
author: blak
categories: [ Home Assistant ]
tags: [ automation ]
image: assets/images/coffee.jpg
toc: true
---

I like my morning coffee. **I NEED MY MORNING COFFEE!** Let's make it smarter.

Every morning I get up, check if the water kettle is full, turn it on, grind the coffee, put it into the french press, pour the recently boiling water over it then tell Google to set a 3 minute timer to brew the coffee. Yeah, I know, that's way too many activities in the morning... 

## Enter the Kettle

Step 1 towards a more streamlined morning coffee: Get a smart kettle. A very smart kettle! 

Enter ProfiCook PC-WKS 1167G 1.5L purchased from [amazon.de](https://amzn.to/2VFsIKN) and flashed with Tasmota. [See flashing and configuration instructions...](https://templates.blakadder.com/proficook_PC-WKS_1167.html)

![Kettle image](/assets/images/kettle.png)

Once Tasmota is configured and the kettle is integrated into Home Assistant it is time to...

## Automate the Coffee

I used:
- Home Assistant aka HA
- ProfiCook PC-WKS 1167G 1.5L
- Google Home (Nest) Display - `media_player.google_display` in HA
- Light Strip integrated in HA  - `light.coffee_light` in HA

Some requirements:
- kettle integrated in HA using the [configuration](https://templates.blakadder.com/proficook_PC-WKS_1167.html)
- have your HA scripts synced and available in Google Home app
- set up [TTS](https://www.home-assistant.io/integrations/tts/) in HA
- create a coffee brewing timer in configuration.yaml

```yaml
timer:
  brew_coffee:
    name: Brew Coffee
    duration: '00:03:20'
```

Now to create the script which is invoked using a Google Home routine by saying "OK Google coffee". If you don't have a voice assistant you can trigger it with a physical switch or button or even with a wakeup automation if you have one. 

Let's dissect the script first, to explain what it does step by step.

To begin, set the volume of Google Display just in case it was turned down and cast a special view tab created in Lovelace to it then turn on the LED strip above the coffee making area:
```yaml
make_coffee:
  alias: Make Coffee
  sequence:
  - service: media_player.volume_set
    data:
      entity_id: media_player.google_display
      volume_level: 0.9
  - service: cast.show_lovelace_view
    data:
      entity_id: media_player.google_display
      view_path: kettle
  - service: light.turn_on
    data:
      entity_id: light.coffee_light
```
In this part of the script we tell the kettle to set maximum water temperature to 95°C and start heating up the water
```yaml      
  - service: mqtt.publish
    data_template:
      topic: cmnd/kettle/TuyaSend2
      payload: "102,95"
  - service: mqtt.publish
    data_template:
      topic: cmnd/kettle/TuyaSend4
      payload: "101,5"
```
The script will now wait until the kettle water temperature reaches 95°C with a timeout of 10 minutes, which means the script will stop executing if the criteria is not met in that timespan. While the water is heating up I prepare the coffee grounds.
{% raw %}
```yaml
  - wait_template: "{{ is_state('sensor.kettle_temperature', '95') }}"
    timeout: '00:10:00'
    continue_on_timeout: 'false'
```
{% endraw %}
Once the temperature is reached a message is broadcast over Google Display to notify me. The script will then wait until the kettle is removed from its base which is a signal water is poured over the coffee grounds and that starts the coffee brewing timer of 3:20 minutes. Since the TTS message breaks the kettle view cast I need to recast it to Google Display.
{% raw %}
```yaml
  - service: tts.google_translate_say
    data:
      message: '{{ Water is heated }}'
      entity_id: media_player.google_display
  - wait_template: "{{ is_state('sensor.kettle_status', 'Kettle removed') }}"
    timeout: '00:03:00'
    continue_on_timeout: 'false'
  - service: media_player.turn_off
    entity_id: media_player.google_display
  - service: cast.show_lovelace_view
    data:
      entity_id: media_player.google_display
      view_path: kettle
  - service: timer.start
    entity_id: timer.brew_coffee
```
{% endraw %}

Script now waits until the timer runs out and once it is done or 3:30 minutes have passed notifies me that the coffee is ready. After 30 seconds it shuts off the kettle and light and completes.
{% raw %}
```yaml
  - wait_template: "{{ is_state('timer.brew_coffee', 'idle') }}"
    timeout: '00:03:30'
    continue_on_timeout: 'true'
  - service: tts.google_translate_say
    data:
      entity_id: media_player.google_display
      message: '{{ Your coffee is ready }}'    
  - delay: 00:00:30
  - service: mqtt.publish
    data_template:
      topic: cmnd/kettle/TuyaSend4
      payload: "101,6"
  - service: media_player.turn_off
    entity_id: media_player.google_display
  - service: light.turn_off
    data:
      entity_id: light.coffee_light
```
{% endraw %}

## The Script

scripts.yaml
{% raw %}
```yaml
make_coffee:
  alias: Make Coffee
  sequence:
  - service: media_player.volume_set
    data:
      entity_id: media_player.google_display
      volume_level: 0.9
  - service: cast.show_lovelace_view
    data:
      entity_id: media_player.google_display
      view_path: kettle
  - service: light.turn_on
    data:
      entity_id: light.coffee_light
  - service: mqtt.publish
    data_template:
      topic: cmnd/kettle/TuyaSend2
      payload: "102,95"
  - service: mqtt.publish
    data_template:
      topic: cmnd/kettle/TuyaSend4
      payload: "101,5"
  - wait_template: "{{ is_state('sensor.kettle_temperature', '95') }}"
    timeout: '00:10:00'
    continue_on_timeout: 'false'
  - service: tts.google_translate_say
    data:
      message: '{{ Water is heated }}'
      entity_id: media_player.google_display
  - wait_template: "{{ is_state('sensor.kettle_status', 'Kettle removed') }}"
    timeout: '00:03:00'
    continue_on_timeout: 'false'
  - service: media_player.turn_off
    entity_id: media_player.google_display
  - service: cast.show_lovelace_view
    data:
      entity_id: media_player.google_display
      view_path: kettle
  - service: timer.start
    entity_id: timer.brew_coffee
  - wait_template: "{{ is_state('timer.brew_coffee', 'idle') }}"
    timeout: '00:03:30'
    continue_on_timeout: 'true'
  - service: tts.google_translate_say
    data:
      entity_id: media_player.google_display
      message: '{{ Your coffee is ready }}'    
  - delay: 00:00:30
  - service: mqtt.publish
    data_template:
      topic: cmnd/kettle/TuyaSend4
      payload: "101,6"
  - service: media_player.turn_off
    entity_id: media_player.google_display
  - service: light.turn_off
    data:
      entity_id: light.coffee_light
```
{% endraw %}

## Lovelace Configuration
![Kettle Lovelace card](/assets/images/kettle-lovelace.jpg)

This is a card that's inside a view tab with the Url "kettle" and Panel mode enabled.

```yaml
cards:
  - cards:
      - entity: sensor.kettle_temperature
        max: 100
        min: 0
        name: Water
        severity:
          green: 40
          red: 95
          yellow: 85
        theme: default
        type: gauge
      - card:
          entities:
            - entity: timer.brew_coffee
          show_icon: false
          type: glance
        conditions:
          - entity: timer.brew_coffee
            state_not: idle
        type: conditional
      - entity: sensor.kettle_status
        name: Status
        type: sensor
    type: vertical-stack
  - cards:
      - entity: sensor.kettle_time_remaining
        name: Time remaining
        type: sensor
        unit: minutes
      - entities:
          - entity: input_select.kettle_set
            name: Mode
          - entity: input_number.kettle_temp
            name: Temperature
        type: entities
    type: vertical-stack
type: horizontal-stack
```

Here's how it looks when its cast to Google Display:
![Kettle on Google Display](/assets/images/kettle-google.jpg)
