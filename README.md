# Intergrating a Neopower all in one heatpump 315L

Add Tuya Local via HACS and setup using the Local key & IP Address.
Protocol version should be set to 3.4


# neopower_heatpump_tuya
Home Assistant control of Neopower 315L all in one heatpump

List of DP ID's below.
Extracted from Tuya Developer Platform

```
{
  "result": {
    "properties": [
      {
        "code": "switch",
        "custom_name": "",
        "dp_id": 1,
        "time": 1759086706216,
        "type": "bool",
        "value": true
      },
      {
        "code": "mode",
        "custom_name": "",
        "dp_id": 2,
        "time": 1759104032658,
        "type": "enum",
        "value": "HYB1"
      },
      {
        "code": "temp_set",
        "custom_name": "",
        "dp_id": 4,
        "time": 1759104031740,
        "type": "value",
        "value": 70
      },
      {
        "code": "defrost",
        "custom_name": "",
        "dp_id": 7,
        "time": 1759098970949,
        "type": "bool",
        "value": false
      },
      {
        "code": "water_set",
        "custom_name": "",
        "dp_id": 10,
        "time": 1758449977981,
        "type": "value",
        "value": 0
      },
      {
        "code": "countdown_left",
        "custom_name": "",
        "dp_id": 14,
        "time": 1759104830455,
        "type": "value",
        "value": 93
      },
      {
        "code": "fault",
        "custom_name": "",
        "dp_id": 15,
        "time": 1758449977981,
        "type": "bitmap",
        "value": 0
      },
      {
        "code": "temp_current",
        "custom_name": "",
        "dp_id": 16,
        "time": 1759104816311,
        "type": "value",
        "value": 58
      },
      {
        "code": "power_consumption",
        "custom_name": "",
        "dp_id": 18,
        "time": 1758449977981,
        "type": "value",
        "value": 56
      },
      {
        "code": "compressor_strength",
        "custom_name": "",
        "dp_id": 20,
        "time": 1759104785084,
        "type": "value",
        "value": 4
      },
      {
        "code": "temp_top",
        "custom_name": "",
        "dp_id": 21,
        "time": 1759104816536,
        "type": "value",
        "value": 58
      },
      {
        "code": "temp_bottom",
        "custom_name": "",
        "dp_id": 22,
        "time": 1759104679308,
        "type": "value",
        "value": 42
      },
      {
        "code": "coiler_temp",
        "custom_name": "",
        "dp_id": 23,
        "time": 1759103378199,
        "type": "value",
        "value": 4
      },
      {
        "code": "venting_temp",
        "custom_name": "",
        "dp_id": 24,
        "time": 1759104780393,
        "type": "value",
        "value": 79
      },
      {
        "code": "around_temp",
        "custom_name": "",
        "dp_id": 26,
        "time": 1759103922487,
        "type": "value",
        "value": 17
      },
      {
        "code": "compressor_state",
        "custom_name": "",
        "dp_id": 27,
        "time": 1759098972453,
        "type": "bool",
        "value": true
      },
      {
        "code": "four_valve_state",
        "custom_name": "",
        "dp_id": 28,
        "time": 1759098942623,
        "type": "bool",
        "value": false
      },
      {
        "code": "draught_fan_state",
        "custom_name": "",
        "dp_id": 29,
        "time": 1759098912029,
        "type": "bool",
        "value": true
      },
      {
        "code": "pump_state",
        "custom_name": "",
        "dp_id": 30,
        "time": 1758449983134,
        "type": "bool",
        "value": false
      },
      {
        "code": "backwater",
        "custom_name": "",
        "dp_id": 31,
        "time": 1759096497157,
        "type": "bool",
        "value": false
      },
      {
        "code": "ele_heating_state",
        "custom_name": "",
        "dp_id": 32,
        "time": 1759029356224,
        "type": "bool",
        "value": false
      },
      {
        "code": "defrost_state",
        "custom_name": "",
        "dp_id": 33,
        "time": 1758449986917,
        "type": "bool",
        "value": false
      },
      {
        "code": "cur_current",
        "custom_name": "",
        "dp_id": 101,
        "time": 1758449980329,
        "type": "value",
        "value": 0
      },
      {
        "code": "cur_voltage",
        "custom_name": "",
        "dp_id": 102,
        "time": 1758449981055,
        "type": "value",
        "value": 0
      },
      {
        "code": "cur_power",
        "custom_name": "",
        "dp_id": 103,
        "time": 1758449981535,
        "type": "value",
        "value": 0
      },
      {
        "code": "electric_total",
        "custom_name": "",
        "dp_id": 104,
        "time": 1758449982296,
        "type": "value",
        "value": 0
      }
    ]
  },
  "success": true,
  "t": REDACTED,
  "tid": "REDACTED"
}
```



		
# modify the yaml file
My Tuya Local detected my device as an aquatech x6 water heater. Once setup go to
/homeassistant/custom_components/tuya_local/devices/aquatech_x6_water_heater.yaml
and edit this file


```
name: Aquatech RAPID/X6
entities:
  - entity: water_heater
    dps:
      - id: 1
        type: boolean
        name: operation_mode
        mapping:
          - dps_val: false
            value: "off"
          - dps_val: true
            constraint: work_mode
            conditions:
              - dps_val: ECO
                value: eco
              - dps_val: Stand
                value: heat_pump
#              - dps_val: HYB
#                value: high_demand
              - dps_val: HYB1
                value: performance
              - dps_val: ELE
                value: electric
      - id: 2
        type: string
        name: work_mode
        hidden: false
      - id: 4
        type: integer
        name: temperature
        unit: C
        range:
          min: 15
          max: 75
        readonly: false
      - id: 7
        type: boolean
        name: defrosting
      - id: 16
        type: integer
        name: current_temperature
      - id: 16
        type: integer
        name: current_temperature        
  - entity: binary_sensor
    class: problem
    category: diagnostic
    dps:
      - id: 15
        type: bitfield
        name: sensor
        mapping:
          - dps_val: 0
            value: false
          - value: true
      - id: 15
        type: bitfield
        name: fault_code
      - id: 32
        type: boolean
        name: ele_heating_state
        mapping:
          - dps_val: false
            value: "off"
          - dps_val: true
            value: "on"            
      - id: 27
        type: boolean
        name: compressor_state
        mapping:
          - dps_val: false
            value: "off"
          - dps_val: true
            value: "on"                 
      - id: 7
        type: boolean
        name: defrost_state
        mapping:
          - dps_val: false
            value: "off"
          - dps_val: true
            value: "on"             
      - id: 24
        type: integer
        name: venting_temp
      - id: 18
        type: integer
        name: power_consumption
      - id: 21
        type: integer
        name: temp_top     
      - id: 22
        type: integer
        name: temp_bottom           
      - id: 28
        type: boolean
        name: four_valve_state
        mapping:
          - dps_val: false
            value: "off"
          - dps_val: true
            value: "on"
'''


The additional entities like defrost state and compressor state appear under the diagnostics tab in the integration
