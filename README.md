# home-assistant

# header H1
## header H2
### header H3
#### header H4
##### header H5
###### header H6

## Technologies
Project is created with:
* Lorem version: 12.3
* Ipsum version: 2.33
* Ament library version: 999


# Dynamic Location Updating
Home Assistant uses it location:


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
Because we live in our RV full time, we assume that wherever we are sleeping at night, we are automating our location updates every night. 
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
