#Keeping Track of Travel Days

##Custom Counter
Home Assistant now has something called Helpers within the settings panel. One of the options is to have a custom counter. 
* Login to Home Assistant
* Navigate to Configuration in the left hand menu
* Select Helpers
* Select Counter

* Give your counter a name, I used "Days On Road" 
* If you are already on the road, insert the number of days you have been on the road in the Intial Value
* Make sure that Step Size is 1


##Automation for Counting Days
In the example below, im updating a few counters at the same time. The first one is our days on the road counter, along with some water based tracking. 
```
alias: Day Counter
description: ''
trigger:
  - platform: sun
    event: sunrise
condition: []
action:
  - service: counter.increment
    data: {}
    entity_id: counter.road
  - service: counter.increment
    data: {}
    entity_id: counter.filter_hose
  - service: counter.increment
    data: {}
    entity_id: counter.house_filter
  - service: counter.increment
    data: {}
    entity_id: counter.black_tank_days
mode: single


```
