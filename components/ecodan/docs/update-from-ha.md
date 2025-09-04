# Using Home Assistant
The esphome component will be auto detected in Home Assistant. In Home Assistant you need to enable actions. See https://esphome.io/components/api.html#actions
Perform the following steps:

```
  1. Find your device in the device list
  2. Click the “configure” button next to it
  3. Check the “Allow the device to perform Home Assistant actions” box
  4. Then click “submit”.
```

Depending on how many remote thermostats you have configured the following actions will become available: `set_climate_temperature_room_0`, `set_climate_temperature_room_1`, .. `set_climate_temperature_room_7` 

Use these actions to link your thermostat to a temp sensor or any other thermostat. Update this value when the source changes. 
You can manually test (if everything is correct) by performing the following steps in Home Assistant:
```
  1. Click Developer tools and select the actions tab
  2. Select action set_climate_temperature_room_0
  3. Fill the temperature field and click perform action
```

Your thermostat is then updated with the new temperature. 

# example: link a climate to the remote thermostat
In this example the climate.kantoor `current_temperature` attribute is linked as remote thermostat `0`. The `current_temperature` attribute is synced to the remote thermostat on value change only.

```yaml
- id: SyncTemperatureToRemoteThermostat
  alias: Sync temp to remote thermostat
  trigger:
    - trigger: state
      entity_id:
        - climate.kantoor
  conditions:
    - condition: template
      value_template: "{{ trigger.from_state.attributes.current_temperature != trigger.to_state.attributes.current_temperature }}"
  actions:
    - action: esphome.ecodan_thermostat_set_climate_temperature_room_0
      data:
        temperature: "{{ state_attr('climate.kantoor' , 'current_temperature') }}"
```

Paste this into a file `automations.yaml` in the `config` folder of home assistant. Include the `automations.yaml` in home assistant by adding the line below in `configuration.yaml`.
```yaml
automation: !include automations.yaml
```
In the 'Developers tools/Check and restart' tab click 'check configuration' and if all is okay click restart (quick reload is fine).

To link another remote thermostat, just duplicate the yaml above in the `automations.yaml` file and give it a unique `id`. Don't forget to adjust the room number in `esphome.ecodan_thermostat_set_climate_temperature_room_x` to match your remote thermostat number.
