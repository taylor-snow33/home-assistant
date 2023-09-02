# Display Live Sensors Example
![alt text](https://github.com/taylor-snow33/home-assistant/blob/main/Images/Live%20Sensors.png?raw=true)

## Vertical Stack Card Code
Required front-end repositories
* [Apex Charts](https://github.com/RomRider/apexcharts-card)
* [Bar Card](https://github.com/custom-cards/bar-card)
* [Card Mod](https://github.com/thomasloven/lovelace-card-mod)
* [Apex Charts](https://github.com/RomRider/apexcharts-card)

```
type: vertical-stack
cards:
  - type: horizontal-stack
    cards:
      - type: custom:apexcharts-card
        apex_config:
          plotOptions:
            radialBar:
              dataLabels:
                name:
                  offsetY: 22px
                value:
                  fontSize: 36px
                  fontWeight: '800'
                  fontFamily: Quicksand
                  offsetY: '-10px'
              stroke:
                lineCap: round
                colors:
                  - '#52C5F5'
                  - '#1A86C8'
          legend:
            show: false
        header:
          show: false
        update_interval: 1sec
        chart_type: radialBar
        series:
          - entity: sensor.energy_percentage
            name: Inverter
            color: '#52C5F5'
            min: 0
            max: 100
            show:
              legend_value: false
              datalabels: false
        card_mod:
          style: |
            :host{
              font-family: 'Quicksand';
            }
            ha-card {
              background-color: transparent;
              font-weight: 800;
              font-family: 'Quicksand';
              box-shadow:none;
              margin-top: 0px;
              margin-left: -55px;
            }
      - type: vertical-stack
        cards:
          - type: custom:mushroom-template-card
            primary: Renogy Inverter {{ states.switch.inverter_2.state }}
            secondary: >-
              {% set last_changed = states.switch.inverter_2.last_changed %} {{
              last_changed | relative_time }}
            icon: ''
            entity: switch.inverter_2
            layout: horizontal
            card_mod:
              style: |
                ha-card {
                  --card-primary-font-size: 20px;
                  --card-primary-font-color: #52C5F5;
                  --card-secondary-font-size: 15px;
                  background-color: transparent;
                  box-shadow: none;
                  margin-top: 30px;
                  color: #52C5F5;
                  font-weight: 600;
                  font-family: 'Quicksand';
                  font-size: 18px;
                  margin-left: -69px;
                }
                .state-badge {
                  display: none !important;
                }
          - type: entities
            entities:
              - entity: sensor.home_energy_meter_gen5_electric_consumption_a
                name: Amp Output
                icon: none
                secondary_info: none
            show_header_toggle: false
            card_mod:
              style: |
                ha-card {
                  color: #4dc7e5;
                  font-weight: 600;
                  font-family: 'Quicksand';
                  font-size: 18px;
                  margin-top: -30px;
                  box-shadow:none;
                  background-color: transparent;
                  margin-left: -130px;
                }
                #states{
                  padding: 16px 16px 0px 16px;
                }
          - type: custom:bar-card
            entities:
              - entity: sensor.energy_percentage
                positions:
                  icon: 'off'
                  name: 'off'
                  indicator: 'off'
                  value: 'off'
                height: 15px
                width: 100%
                color: '#52C5F5'
                decimal: '0'
                min: '0'
                unit_of_measurement: ' '
                max: '100'
            card_mod:
              style: |
                #states{
                  padding: 0px 16px 16px 16px !important;
                }
                ha-card {
                  background-color: transparent;
                  box-shadow:none;
                  margin-top: -10px;
                  margin-left: -75px;
                }
                .state-badge{
                  display: none !important;
                }
  - type: markdown
    content: ' '
    card_mod:
      style: |
        ha-card {
          background-color: transparent;
          border-radius: 0px; 
          box-shadow: none;
          margin-top: -35px;
          margin-left: 20px;
          margin-right: 20px;
          border-bottom: 1px solid #404657;
        }
  - type: horizontal-stack
    cards:
      - type: custom:apexcharts-card
        apex_config:
          plotOptions:
            radialBar:
              dataLabels:
                name:
                  offsetY: 22px
                value:
                  fontSize: 36px
                  fontWeight: '800'
                  fontFamily: Quicksand
                  offsetY: '-10px'
              stroke:
                lineCap: round
                colors:
                  - '#52C5F5'
                  - '#1A86C8'
          legend:
            show: false
        header:
          show: false
        update_interval: 1sec
        chart_type: radialBar
        series:
          - entity: sensor.mentay_escape_battery
            name: Battery
            color: '#53F6E6'
            min: 0
            max: 100
            show:
              legend_value: false
              datalabels: false
        card_mod:
          style: |
            :host{
              font-family: 'Quicksand';
            }
            ha-card {
              background-color: transparent;
              font-weight: 800;
              font-family: 'Quicksand';
              box-shadow:none;
              margin-top: -40px;
              margin-left: -55px;
            }
      - type: vertical-stack
        cards:
          - type: conditional
            conditions:
              - entity: binary_sensor.charging
                state: 'off'
            card:
              type: custom:mushroom-template-card
              primary: |-
                Battery   {% if is_state('binary_sensor.charging', 'on') %}
                    Charging
                  {% else %}
                    Discharging
                  {% endif %}
              secondary: ''
              icon: ''
              entity: switch.inverter
              layout: horizontal
              card_mod:
                style: |
                  ha-card {
                    --card-primary-font-size: 20px;
                    background-color: transparent;
                    box-shadow: none;
                    margin-top: -20px;
                    color: #52C5F5;
                    font-weight: 600;
                    font-family: 'Quicksand';
                    font-size: 18px;
                    margin-left: -69px;
                  }
                  .state-badge {
                    display: none !important;
                  }
          - type: conditional
            conditions:
              - entity: binary_sensor.charging
                state: 'on'
            card:
              type: custom:mushroom-template-card
              primary: |-
                Battery   {% if is_state('binary_sensor.charging', 'on') %}
                    Charging
                  {% else %}
                    Discharging
                  {% endif %}
              secondary: >-
                {% set battery_capacity = 276 %}

                {% set battery_level = states('sensor.mentay_escape_battery') |
                float %}

                {% set charge_current = states('sensor.mentay_escape_current') |
                float %}

                {% set remaining_capacity = (100 - battery_level) / 100 *
                battery_capacity %}

                {% if charge_current > 0 %}
                  {% set estimated_time_minutes = remaining_capacity / charge_current * 60 %}
                  {% set estimated_time_hours = estimated_time_minutes // 60 %}
                  {% set estimated_time_minutes_remainder = estimated_time_minutes % 60 %}
                  {{ estimated_time_hours | int }} hours {% if estimated_time_minutes_remainder > 0 %}{{ estimated_time_minutes_remainder | int }}min{% endif %} until fully charged
                {% else %}
                  Fully Charged
                {% endif %}
              icon: ''
              entity: switch.inverter
              layout: horizontal
              card_mod:
                style: |
                  ha-card {
                    --card-primary-font-size: 20px;
                    --card-secondary-font-size: 15px;
                    background-color: transparent;
                    box-shadow: none;
                    margin-top: -20px;
                    color: #52C5F5;
                    font-weight: 600;
                    font-family: 'Quicksand';
                    font-size: 18px;
                    margin-left: -69px;
                  }
                  .state-badge {
                    display: none !important;
                  }
          - type: entities
            entities:
              - entity: sensor.mentay_escape_voltage
                name: Voltage
                icon: none
                secondary_info: none
            show_header_toggle: false
            card_mod:
              style: |
                ha-card {
                  color: #53F6E6;
                  font-weight: 600;
                  font-family: 'Quicksand';
                  font-size: 18px;
                  margin-top: -40px;
                  box-shadow:none;
                  background-color: transparent;
                  margin-left: -130px;
                }
                #states{
                  padding: 16px 16px 0px 16px;
                }
          - type: conditional
            conditions:
              - entity: binary_sensor.charging
                state: 'on'
            card:
              type: entities
              entities:
                - entity: sensor.mentay_escape_current
                  name: Current
                  icon: none
                  secondary_info: none
              show_header_toggle: false
              card_mod:
                style: |
                  ha-card {
                    color: #53F6E6;
                    font-weight: 600;
                    font-family: 'Quicksand';
                    font-size: 18px;
                    margin-top: -40px;
                    box-shadow:none;
                    background-color: transparent;
                    margin-left: -130px;
                  }
                  #states{
                    padding: 16px 16px 0px 16px;
                  }
          - type: conditional
            conditions:
              - entity: binary_sensor.charging
                state: 'off'
            card:
              type: entities
              entities:
                - entity: sensor.mentay_escape_time_remaining
                  name: Time
                  icon: none
                  secondary_info: none
              show_header_toggle: false
              card_mod:
                style: |
                  ha-card {
                    color: #53F6E6;
                    font-weight: 600;
                    font-family: 'Quicksand';
                    font-size: 18px;
                    margin-top: -40px;
                    box-shadow:none;
                    background-color: transparent;
                    margin-left: -130px;
                  }
                  #states{
                    padding: 16px 16px 0px 16px;
                  }
          - type: conditional
            conditions:
              - entity: binary_sensor.charging
                state: 'off'
            card:
              type: entities
              entities:
                - entity: sensor.mentay_escape_current
                  name: Current
                  icon: none
                  secondary_info: none
              show_header_toggle: false
              card_mod:
                style: |
                  ha-card {
                    color: #F4938C;
                    font-weight: 600;
                    font-family: 'Quicksand';
                    font-size: 18px;
                    margin-top: -40px;
                    box-shadow:none;
                    background-color: transparent;
                    margin-left: -130px;
                  }
                  #states{
                    padding: 16px 16px 0px 16px;
                  }
          - type: conditional
            conditions:
              - entity: binary_sensor.charging
                state: 'on'
            card:
              type: custom:bar-card
              entities:
                - entity: sensor.mentay_escape_current
                  positions:
                    icon: 'off'
                    name: 'off'
                    indicator: 'off'
                    value: 'off'
                  height: 15px
                  width: 100%
                  color: '#53F6E6'
                  decimal: '1'
                  unit_of_measurement: ' '
                  max: '60'
                  min: '0.1'
              card_mod:
                style: |
                  #states{
                    padding: 0px 16px 16px 16px !important;
                  }
                  ha-card {
                    background-color: transparent;
                    box-shadow:none;
                    margin-top: -10px;
                    margin-left: -75px;
                  }
                  .state-badge{
                    display: none !important;
                  }
          - type: conditional
            conditions:
              - entity: binary_sensor.charging
                state: 'off'
            card:
              type: custom:bar-card
              entities:
                - entity: sensor.mentay_escape_current
                  positions:
                    icon: 'off'
                    name: 'off'
                    indicator: 'off'
                    value: 'off'
                  height: 15px
                  width: 100%
                  color: '#F4938C'
                  decimal: '1'
                  unit_of_measurement: ' '
                  max: '-180'
                  min: '0'
                  value: '{{ -180 - states("sensor.mentay_escape_current") | float }}'
              card_mod:
                style: |
                  #states{
                    padding: 0px 16px 16px 16px !important;
                  }
                  ha-card {
                    background-color: transparent;
                    box-shadow:none;
                    margin-top: -10px;
                    margin-left: -75px;
                  }
                  .state-badge{
                    display: none !important;
                  }
  - type: markdown
    content: ' '
    card_mod:
      style: |
        ha-card {
          background-color: transparent;
          border-radius: 0px; 
          box-shadow: none;
          margin-top: -35px;
          margin-left: 20px;
          margin-right: 20px;
          border-bottom: 1px solid #404657;
        }
  - type: vertical-stack
    cards:
      - type: entities
        entities:
          - entity: sensor.modem_2_lte_signal
            icon: mdi:signal-4g
            secondary_info: none
            name: Internet Signal
        show_header_toggle: false
        card_mod:
          style: |
            ha-card {
              color: #fff;
              font-weight: 600;
              font-family: 'Quicksand';
              font-size: 18px;
              margin-top: -30px;
              box-shadow:none;
              margin-bottom: 30px;
            }
            #states{
              padding: 16px 16px 0px 16px;
            }
            ha-card {
              background-color: transparent;
              box-shadow:none;
            }
      - type: custom:bar-card
        entities:
          - entity: sensor.modem_2_lte_signal
            positions:
              icon: 'off'
              name: 'off'
              indicator: 'off'
              value: 'off'
            height: 15px
            width: 100%
            color: '#fff'
            decimal: '0'
            min: '0'
            unit_of_measurement: ' '
            max: '100'
        card_mod:
          style: |
            #states{
              padding: 0px 16px 16px 16px !important;
            }
            ha-card {
              background-color: transparent;
              box-shadow:none;
              margin-top: -30px;
              margin-bottom: 30px;
            }
            .state-badge{
              display: none !important;
            }
  - type: horizontal-stack
    cards:
      - type: vertical-stack
        cards:
          - type: custom:bar-card
            direction: up
            height: 195px
            stack: horizontal
            style:
              text-align: center
            positions:
              icon: inside
              indicator: 'off'
              minmax: 'off'
              title: outside
              value: inside
            entities:
              - entity: sensor.multiplied_tank_level
                min: '0'
                max: '100'
                icon: mdi:water
                name: Fresh
                decimal: '0'
              - entity: input_number.tank_1
                name: Grey
                icon: mdi:shower
                color: '#3F96BB'
                decimal: '0'
            card_mod:
              style: |
                :host{
                  font-family: 'Quicksand';
                }
                ha-icon{
                  margin-top: 10px;
                }
                ha-card {
                  background-color: transparent;
                  margin-top: -25px;
                  box-shadow:none;
                }
                bar-card-name{
                  font-size: 14px;
                  font-weight: bold;
                  text-shadow: 1px 1px #0005;
                }
                bar-card-value {
                  font-size: 16px;
                  font-weight: bold;
                  text-shadow: 1px 1px #0005;
                  margin-bottom: 15px;
                }
                bar-card-currentbar{
                  border-radius: 15px;
                }
                bar-card-backgroundbar{
                  border-radius: 15px;
                }
      - type: vertical-stack
        cards:
          - type: custom:bar-card
            direction: up
            height: 195px
            stack: horizontal
            bar-card-border-radius: 20px
            positions:
              icon: inside
              indicator: 'off'
              minmax: 'off'
              value: inside
            entities:
              - name: Passenger
                icon: mdi:propane-tank
                color: '#7752F5'
                decimal: '0'
                entity: sensor.passenger_level
              - entity: sensor.driver_level
                name: Driver
                icon: mdi:propane-tank
                color: '#7752F5'
                decimal: '0'
            card_mod:
              style: |
                :host{
                  font-family: 'Quicksand';
                }
                ha-icon{
                  margin-top: 10px;
                }
                ha-card {
                  background-color: transparent;
                  margin-top: -25px;
                  box-shadow:none;
                }
                bar-card-name{
                  font-size: 14px;
                  font-weight: bold;
                  text-shadow: 1px 1px #0005;
                }
                bar-card-value {
                  font-size: 16px;
                  font-weight: bold;
                  text-shadow: 1px 1px #0005;
                  margin-bottom: 15px;
                }
                bar-card-currentbar{
                  border-radius: 15px;
                }
                bar-card-backgroundbar{
                  border-radius: 15px;
                }


```

