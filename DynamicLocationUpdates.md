# Dynamic Location Updates
![alt text](https://github.com/taylor-snow33/home-assistant/blob/main/Images/Location_Example.png?raw=true)
Home Assistant uses it location for the following uses:
* Local weather data
* Setting a Home Location
* Building Home and Away modes

## Set Location Script
```
alias: Set Location
sequence:
  - service: homeassistant.set_location
    data:
      latitude: '{{ states.device_tracker.life360_taylor_snow.attributes.latitude }}'
      longitude: '{{ states.device_tracker.life360_taylor_snow.attributes.longitude }}'
mode: single
```
## Automating location udpates when sleeping
Because we live in our RV full time, we assume that wherever we are sleeping at night, we are automating our location updates every night. The example below is using the built in sunset sensor and an offset. This may not be the best approach for every
```
alias: Set Location
description: ''
trigger:
  - platform: sun
    event: sunset
    offset: '05:30:00'
condition: []
action:
  - service: script.toggle
    data: {}
    entity_id: script.set_location
mode: single
```


## Automating location udpates when traveling
We tow our RV around behind our truck and move fairly often, so updating our location when moving around is handy for automations. 
```
alias: Set Location Travel Mode
description: This sets the location based on my phone activity and SSID
trigger:
  - platform: state
    entity_id: device_tracker.life360_taylor_snow
    from: Driving
    to: '*'
condition:
  - condition: state
    entity_id: sensor.taylors_iphone_ssid
    state: MenTay Escape
    attribute: friendly_name
action:
  - service: script.set_location
    data: {}
mode: parallel
max: 10
```

