# Dynamic Location Updates
![alt text](https://github.com/taylor-snow33/home-assistant/blob/main/Images/Location_Example.png?raw=true)
Home Assistant uses it location for the following uses:
* Local weather data
* Setting a Home Location
* Building Home and Away modes

## Getting a Dynamic Location
Ideally I would have a dedicated GPS sensor on the RV, but time does not allow this. Using the devices and services already out there we can leverage pre-built Home Assistant integrations. 

### Life 360
[Life 360](https://www.home-assistant.io/integrations/life360/)This service has a decent free offering that we run on our phone. It updates location and provides more data far more frequently than the Home Assistant Mobile Apps. Follow [the Life360 integration guide](https://www.home-assistant.io/integrations/life360/) to get your phones location into Home Assistant. This can be tied to the User and Person object within Home Assistant.

### Home Assistant Mobile App
Home Asssitant builds their own [iOS, MacOS and Android apps]https://www.home-assistant.io/integrations/mobile_app/ to access your installation. Among many data points it provides, we can access the GPS latitude and longitude of the device. 

## Set Location Script
Now that we have a GPS in the RV, we need to control when we update the location of Home Assistant. I started by building a script that is useful to create a manual update button, as well as automations. This is helpful so that your not constantly telling the system that its moving when your phone is moving, you want to be able to tell it where it is, only at certian times. 
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

