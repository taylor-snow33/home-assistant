# Dynamic Location Updates
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