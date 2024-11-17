# HASS-Costs-Dashboard

##You can copy/paste this into a new "raw conf" dashboard in Home Assistant##


views:
  - title: Costs
    type: sections
    sections:
      - type: grid
        columns: 2
        cards:
          - type: heading
            heading: Daily Costs Overview
          - type: custom:mini-graph-card
            name: Accumulative Cost Overview
            hours_to_show: 24
            points_per_hour: 4
            line_width: 2
            entities:
              - entity: >-
                  sensor.octopus_energy_electricity_20l3345714_1415699912006_current_accumulative_cost
                name: Electricity Cost
                color: '#FFA726'
              - entity: >-
                  sensor.octopus_energy_gas_e6s24887631961_4160990601_current_accumulative_cost
                name: Gas Cost
                color: '#EF5350'
          - type: heading
            heading: Energy Savings
          - type: custom:mini-graph-card
            name: Dispatch Monitor
            entities:
              - entity: >-
                  binary_sensor.octopus_energy_a_6150b1bf_intelligent_dispatching
            hours_to_show: 48
            points_per_hour: 2
            line_width: 2
            state_map:
              - value: 'off'
                label: Not Dispatching
                color: '#FF0000'
              - value: 'on'
                label: Dispatching
                color: '#00FF00'
            show:
              labels: true
          - type: custom:mushroom-entity-card
            entity: sensor.battery_charge_cost
            name: Battery Charge Cost
            icon: mdi:battery-charging
            fill_container: true
          - type: custom:mushroom-entity-card
            entity: sensor.battery_discharge_savings
            name: Battery Discharge Savings
            icon: mdi:battery-minus
            fill_container: true
          - type: custom:mushroom-entity-card
            entity: sensor.daily_savings
            name: Total Daily Savings
            icon: mdi:piggy-bank
            fill_container: true
          - type: custom:mushroom-entity-card
            entity: sensor.octopus_energy_a_6150b1bf_octoplus_points
            name: Octopus Points
            icon: mdi:star-circle
            fill_container: true
      - type: grid
        columns: 2
        cards:
          - type: heading
            heading: Electricity Costs
          - type: custom:mushroom-template-card
            primary: Today's Electric Cost
            secondary: >-
              {{
              states('sensor.octopus_energy_electricity_20l3345714_1415699912006_current_accumulative_cost')
              }}
            icon: mdi:currency-gbp
            fill_container: true
            icon_color: >-
              {% set cost =
              states('sensor.octopus_energy_electricity_20l3345714_1415699912006_current_accumulative_cost')
              | float %} {% if cost < 10 %}
                green
              {% elif cost < 50 %}
                yellow
              {% else %}
                red
              {% endif %}
          - type: custom:mushroom-template-card
            primary: Yesterday's Electric Cost
            secondary: >-
              {{
              states('sensor.octopus_energy_electricity_20l3345714_1415699912006_previous_accumulative_cost')
              }}
            icon: mdi:currency-gbp-off
            fill_container: true
            icon_color: >-
              {% set cost =
              states('sensor.octopus_energy_electricity_20l3345714_1415699912006_previous_accumulative_cost')
              | float %} {% if cost < 10 %}
                green
              {% elif cost < 50 %}
                yellow
              {% else %}
                red
              {% endif %}
          - type: custom:mushroom-entity-card
            entity: >-
              sensor.octopus_energy_electricity_20l3345714_1415699912006_current_rate
            name: Current Electricity Rate
            icon: mdi:transmission-tower
            fill_container: true
            grid_options:
              columns: full
          - type: custom:mini-graph-card
            name: Electricity Rate History
            entities:
              - entity: >-
                  sensor.octopus_energy_electricity_20l3345714_1415699912006_current_rate
            hours_to_show: 24
            points_per_hour: 2
            line_width: 2
            show:
              labels: true
              points: true
          - type: heading
            heading: Dynamic Costs
          - type: custom:mushroom-template-card
            primary: EV Dynamic Cost
            secondary: £{{ states('sensor.ev_dynamic_cost') }}
            icon: mdi:car-electric
            fill_container: true
            icon_color: >-
              {% set cost = states('sensor.ev_dynamic_cost') | float %} {% if
              cost < 0.5 %}
                green
              {% elif cost < 2 %}
                yellow
              {% else %}
                red
              {% endif %}
          - type: custom:mushroom-template-card
            primary: Quooker Dynamic Cost
            secondary: '{{ states(''sensor.quooker_dynamic_cost'') }}p'
            icon: mdi:water-boiler
            fill_container: true
            icon_color: >-
              {% set cost = states('sensor.quooker_dynamic_cost') | float %} {%
              if cost < 10 %}
                green
              {% elif cost < 25 %}
                yellow
              {% else %}
                red
              {% endif %}
          - type: custom:mushroom-entity-card
            entity: sensor.charging_dock_smartplug_energy
            name: Charging Dock Usage
            icon: mdi:battery-charging-wireless
            fill_container: true
          - type: heading
            icon: mdi:fridge
            heading: Appliances
            heading_style: title
          - type: tile
            entity: sensor.dryer_smartplug_energy
          - type: tile
            entity: sensor.washer_smartplug_energy
          - type: vertical-stack
            cards:
              - type: custom:mushroom-template-card
                primary: Washing Machine
                secondary: Energy Usage Overview
                icon: mdi:washing-machine
                tap_action:
                  action: more-info
              - type: grid
                columns: 3
                square: false
                cards:
                  - type: custom:mushroom-template-card
                    primary: Daily
                    secondary: '{{ states(''sensor.washer_daily_energy'') }} kWh'
                    icon: mdi:calendar-today
                    icon_color: >-
                      {% set energy = states('sensor.washer_daily_energy') |
                      float %} {% if energy < 0.5 %}
                        green
                      {% elif energy < 1 %}
                        yellow
                      {% else %}
                        red
                      {% endif %}
                  - type: custom:mushroom-template-card
                    primary: Weekly
                    secondary: '{{ states(''sensor.washer_weekly_energy'') }} kWh'
                    icon: mdi:calendar-week
                    icon_color: >-
                      {% set energy = states('sensor.washer_weekly_energy') |
                      float %} {% if energy < 2 %}
                        green
                      {% elif energy < 4 %}
                        yellow
                      {% else %}
                        red
                      {% endif %}
                  - type: custom:mushroom-template-card
                    primary: Monthly
                    secondary: '{{ states(''sensor.washer_monthly_energy'') }} kWh'
                    icon: mdi:calendar-month
                    icon_color: >-
                      {% set energy = states('sensor.washer_monthly_energy') |
                      float %} {% if energy < 8 %}
                        green
                      {% elif energy < 15 %}
                        yellow
                      {% else %}
                        red
                      {% endif %}
                  - type: custom:mushroom-template-card
                    primary: Power
                    secondary: '{{ states(''sensor.washer_smartplug_power'') }} W'
                    icon: mdi:lightning-bolt
                    icon_color: >-
                      {% set power = states('sensor.washer_smartplug_power') |
                      float %} {% if power < 100 %}
                        green
                      {% elif power < 500 %}
                        yellow
                      {% else %}
                        red
                      {% endif %}
                  - type: custom:mushroom-template-card
                    primary: Status
                    secondary: >-
                      {% if states('sensor.washer_smartplug_power') | float > 5
                      %}
                        Running
                      {% else %}
                        Idle
                      {% endif %}
                    icon: mdi:state-machine
                    icon_color: >-
                      {% if states('sensor.washer_smartplug_power') | float > 5
                      %}
                        blue
                      {% else %}
                        gray
                      {% endif %}
                  - type: custom:mushroom-template-card
                    primary: Voltage
                    secondary: '{{ states(''sensor.washer_smartplug_voltage'') }} V'
                    icon: mdi:voltage-ac
                    icon_color: blue
      - type: grid
        columns: 2
        cards:
          - type: heading
            heading: Gas Costs
          - type: custom:mushroom-template-card
            primary: Today's Gas Cost
            secondary: >-
              {{
              states('sensor.octopus_energy_gas_e6s24887631961_4160990601_current_accumulative_cost')
              }}
            icon: mdi:fire
            fill_container: true
            icon_color: >-
              {% set cost =
              states('sensor.octopus_energy_gas_e6s24887631961_4160990601_current_accumulative_cost')
              | float %} {% if cost < 5 %}
                green
              {% elif cost < 25 %}
                yellow
              {% else %}
                red
              {% endif %}
          - type: custom:mushroom-template-card
            primary: Yesterday's Gas Cost
            secondary: >-
              {{
              states('sensor.octopus_energy_gas_e6s24887631961_4160990601_previous_accumulative_cost')
              }}
            icon: mdi:fire-off
            fill_container: true
            icon_color: >-
              {% set cost =
              states('sensor.octopus_energy_gas_e6s24887631961_4160990601_previous_accumulative_cost')
              | float %} {% if cost < 5 %}
                green
              {% elif cost < 25 %}
                yellow
              {% else %}
                red
              {% endif %}
          - type: custom:mushroom-entity-card
            entity: sensor.octopus_energy_gas_e6s24887631961_4160990601_current_rate
            name: Current Gas Rate
            icon: mdi:meter-gas
            fill_container: true
            grid_options:
              columns: full
          - type: custom:mini-graph-card
            name: Gas Rate History
            entities:
              - entity: >-
                  sensor.octopus_energy_gas_e6s24887631961_4160990601_current_rate
            hours_to_show: 24
            points_per_hour: 2
            line_width: 2
            show:
              labels: true
              points: true
          - type: custom:mini-graph-card
            name: Power Usage Comparison
            entities:
              - entity: sensor.grid_power_kw
                name: Grid Power
              - entity: sensor.modbus_battery_discharge
                name: Battery Power
              - entity: sensor.modbus_pv1_power
                name: Solar
            hours_to_show: 24
            points_per_hour: 2
            line_width: 2
            show:
              labels: true
              points: true
              legend: true
              fill: true
            aggregate: none
          - type: vertical-stack
            cards:
              - type: custom:mushroom-template-card
                primary: Tumble Dryer
                secondary: Energy Usage Overview
                icon: mdi:tumble-dryer
              - type: grid
                columns: 3
                square: false
                cards:
                  - type: custom:mushroom-template-card
                    primary: Daily
                    secondary: '{{ states(''sensor.dryer_daily_energy'') }} kWh'
                    icon: mdi:calendar-today
                    icon_color: >-
                      {% set energy = states('sensor.dryer_daily_energy') |
                      float %} {% if energy < 1 %}
                        green
                      {% elif energy < 2 %}
                        yellow
                      {% else %}
                        red
                      {% endif %}
                    tap_action:
                      action: more-info
                  - type: custom:mushroom-template-card
                    primary: Weekly
                    secondary: '{{ states(''sensor.dryer_weekly_energy'') }} kWh'
                    icon: mdi:calendar-week
                    icon_color: >-
                      {% set energy = states('sensor.dryer_weekly_energy') |
                      float %} {% if energy < 4 %}
                        green
                      {% elif energy < 8 %}
                        yellow
                      {% else %}
                        red
                      {% endif %}
                  - type: custom:mushroom-template-card
                    primary: Monthly
                    secondary: '{{ states(''sensor.dryer_monthly_energy'') }} kWh'
                    icon: mdi:calendar-month
                    icon_color: >-
                      {% set energy = states('sensor.dryer_monthly_energy') |
                      float %} {% if energy < 15 %}
                        green
                      {% elif energy < 30 %}
                        yellow
                      {% else %}
                        red
                      {% endif %}
                  - type: custom:mushroom-template-card
                    primary: Power
                    secondary: '{{ states(''sensor.dryer_smartplug_power'') }} W'
                    icon: mdi:lightning-bolt
                    icon_color: >-
                      {% set power = states('sensor.dryer_smartplug_power') |
                      float %} {% if power < 200 %}
                        green
                      {% elif power < 1000 %}
                        yellow
                      {% else %}
                        red
                      {% endif %}
                  - type: custom:mushroom-template-card
                    primary: Status
                    secondary: >-
                      {% if states('sensor.dryer_smartplug_power') | float > 5
                      %}
                        Running
                      {% else %}
                        Idle
                      {% endif %}
                    icon: mdi:state-machine
                    icon_color: >-
                      {% if states('sensor.dryer_smartplug_power') | float > 5
                      %}
                        blue
                      {% else %}
                        gray
                      {% endif %}
                  - type: custom:mushroom-template-card
                    primary: Voltage
                    secondary: '{{ states(''sensor.dryer_smartplug_voltage'') }} V'
                    icon: mdi:voltage-ac
                    icon_color: blue
    max_columns: 4
    cards: []
