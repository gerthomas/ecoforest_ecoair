[![SWUbanner](https://raw.githubusercontent.com/vshymanskyy/StandWithUkraine/main/banner-direct-single.svg)](https://stand-with-ukraine.pp.ua/)

Ecoforest EcoGeo heat pump integration PoC (proof of concept, thus may lack features or stability)

To install this integration, add this GitHub Repo to the HACS Custom Repositories or use the button below

[![Open your Home Assistant instance and open a repository inside the Home Assistant Community Store.](https://my.home-assistant.io/badges/hacs_repository.svg)](https://my.home-assistant.io/redirect/hacs_repository/?owner=bytestorm&repository=ecoforest_ecogeo&category=integration)

After installing the integration in HACS
- Restart Home Assistant 
- Go to "Settings" -> "Devices & Services" -> "Add integration"
- Look for "Ecoforest Ecogeo" integration to start setting it up
- For "Ecoforest Easynet URL" use the web interface address of your heat pump (e.g. http://192.168.1.200/)
- Username is your heat pump serial number (same as its web interface)
- Password is the first 8 characters of the original wireless network password (same as its web interface)
- Optionally, you can specify an alias that will be used instead of the model name to generate entity prefixes


------------

Custom visualization:

![flow](https://github.com/bytestorm/ecoforest_ecogeo/blob/master/flow.png?raw=true)

<details>
  <summary>Details...</summary>
  
  This card is built using [power-flow-card-plus](https://github.com/flixlix/power-flow-card-plus). You can install it with HACS.<br />
  Just replace entity prefixes (ebfhbb in my case) with the model name or the alias of your heat pump.


```yaml
type: custom:power-flow-card-plus
entities:
  grid:
    entity: sensor.ebfhbb_power_electric
    display_state: one_way_no_zero
    name: Consumption
    color:
      consumption:
        - 78
        - 122
        - 39
  home:
    entity: sensor.ebfhbb_power_output
    icon: mdi:hvac
    subtract_individual: false
    override_state: true
    secondary_info:
      entity: sensor.ebfhbb_t_outdoor
      unit_of_measurement: °C
      decimals: 1
  individual:
    - entity: sensor.ebfhbb_power_cooling
      display_zero_state: false
      name: Cooling
      secondary_info:
        entity: sensor.ebfhbb_t_cooling
        unit_of_measurement: °C
        display_zero: false
        decimals: 1
      icon: mdi:snowflake
      display_zero: true
      color:
        - 0
        - 213
        - 255
    - entity: sensor.ebfhbb_power_heating
      display_zero: true
      name: Heating
      icon: mdi:heating-coil
      secondary_info:
        entity: sensor.ebfhbb_t_heating
        unit_of_measurement: °C
      color:
        - 203
        - 37
        - 37
      decimals: 1
      display_zero_state: false
    - name: DHW
      icon: mdi:water-boiler
      color:
        - 212
        - 154
        - 28
      decimals: 1
      display_zero: true
      secondary_info:
        entity: sensor.ebfhbb_t_dhw
        unit_of_measurement: °C
        decimals: 1
      display_zero_state: false
      entity: '0'
clickable_entities: true
display_zero_lines:
  mode: show
  transparency: 50
  grey_color:
    - 189
    - 189
    - 189
use_new_flow_rate_model: true
w_decimals: 0
kw_decimals: 1
min_flow_rate: 5
max_flow_rate: 5
max_expected_power: 2000
min_expected_power: 0.01
watt_threshold: 1000
transparency_zero_lines: 0
disable_dots: true
```
</details>
