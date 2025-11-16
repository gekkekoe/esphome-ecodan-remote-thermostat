# Using Home Assistant

Depending on how many remote thermostats you have configured the following number input will become available: `room_0`, `room_1`, .. `room_7` 

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
    - action: number.set_value
      target:
        entity_id: >-
          number.ecodan_thermostat_room_0
      data:
        temperature: "{{ state_attr('climate.kantoor' , 'current_temperature') }}"
```

Paste this into a file `automations.yaml` in the `config` folder of home assistant. Include the `automations.yaml` in home assistant by adding the line below in `configuration.yaml`.
```yaml
automation: !include automations.yaml
```
In the 'Developers tools/Check and restart' tab click 'check configuration' and if all is okay click restart (quick reload is fine).

To link another remote thermostat, just duplicate the yaml above in the `automations.yaml` file and give it a unique `id`. Don't forget to adjust the room number in `number.ecodan_thermostat_room_x` to match your remote thermostat number.
