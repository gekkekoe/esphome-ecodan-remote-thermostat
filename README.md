# ESPHome Ecodan Remote Thermostat
Mimics a remote thermostat over CNRF. You can use any thermostat/temp sensor as a remote thermostat. This projects supports up to 8 remote thermostats. Use any temperature sensor or thermostat as data source for the Ecodan heatpump. The required hardware is the same as used by https://github.com/gekkekoe/esphome-ecodan-hp

# available languages
English (default), Dutch, Italian, French, Spanish. Select the language in `ecodan-remote-thermostat-esphome.yaml` file. 
If you want to contribute with a translation: copy the file `ecodan-labels-en.yaml` to `ecodan-labels-xx.yaml`, fill in all the labels and submit a pull request.

# recommended hardware
If you don't want to solder, use one of the tested boards. [More boards that should be working](https://github.com/SwiCago/HeatPump/issues/13#issuecomment-457897457). It also should work for airco units with cn105 connectors. 

Cable
* Get one of the grove female cable and a ST PAP-05V-S connector. Remove one end of the grove connector and replace it with a ST PAP-05V-S connector. Here's an example:
![image](https://github.com/gekkekoe/esphome-ecodan-hp/blob/main/img/m5stack_cn105.jpg?raw=true)

Pin mapping (from left to right)
| grove | cn105 |
|:---|:---|
|pin 1 - black (gnd) | pin 4 - black (gnd) |
|pin 2 - red (5v) | pin 3 - red (5v) |
|pin 3 - white (GPIO 2) | pin 2 - white (Tx) |
|pin 4 - yellow (GPIO 1) | pin 1 - yellow (Rx) |

*Note: pin 5 (12v) on the cn105 is not used.*

# where to buy
atom s3 lite
https://www.digikey.nl/en/products/detail/m5stack-technology-co-ltd/C124/18070571

grove cable (multiple options/lengths available)
https://www.digikey.nl/en/products/detail/seeed-technology-co-ltd/110990036/5482563

JST PAP-05V-S connector
https://www.digikey.nl/en/products/detail/jst-sales-america-inc/PAP-05V-S/759977

# easy install: download the latest firmware
## Get the factory.bin firmware for initial flash
* Go to https://github.com/gekkekoe/esphome-ecodan-remote-thermostat/releases/latest and download one of the factory.bin files. 
```
Firmware for Atom S3 (Lite): room-1-version.factory.bin (single zone remote thermostat)
Firmware for Atom S3 (Lite): room-2-version.factory.bin (zone1 and zone2 remote thermostat)
```
* Go to https://web.esphome.io/?dashboard_install (Only works in Chrome or Edge), connect the atom via usb-c and click connect. Chrome will let you select the detected esp. Finally you click install and select the factory.bin firmware (downloaded in previous step) and flash.

* Power the esp via usb-c. The led indicator will turn blue indicating that its in access point mode. Connect to the wifi network `ecodan-heatpump` with password `configesp`. Fill in your wifi credentials and click save. The esp will reboot and the led will become green indicating that the wifi is connected correctly. Disconnect the unit from the computer and connect it to the heatpump via the CNRF connector (don't forget to power down the unit via circuit breaker!).

* You will get notifications if an update is available and its updateable in Home Assistant itself. 

# build esphome-ecodan-remote-thermostat firmware
### Build via cmd line:
* Install ESPHome https://esphome.io/guides/getting_started_command_line.html
    ```console
    python3 -m venv venv
    source venv/bin/activate
    pip3 install wheel
    pip3 install esphome
    ```
* Fill in `secrets.yaml` and copy the `ecodan-remote-thermostat-esphome.yaml` to your esphome folder and edit the values (*check GPO pins (uart: section), you might need to swap the pins in the config*)
The secrets.yaml should at least contain the following entries:
```
wifi_ssid: "wifi network id"
wifi_password: "wifi password"
```
* Edit the following section in `ecodan-remote-thermostat-esphome.yaml` to match your configuration. ESPhome (currently) does not support remote yaml file templating. You probably need to download the `thermostat-room.yaml` file in a `conf` subfolder of the folder where your `ecodan-remote-thermostat-esphome.yaml` is stored. Then configure up to 8 remote thermostats. Use the commented line as template (don't forget to replace `thermostat-room-1` and `room_identifier: 1`):
```yaml
thermostat-room-1: !include { file: confs/thermostat-room.yaml, vars: { room_identifier: 1, room_name: "Room 2" } }
```

```yaml
packages:
# up to 8 thermostats can be defined. 0-7.
  thermostat-room-0: !include { file: confs/thermostat-room.yaml, vars: { room_identifier: 0, room_name: "Room 1" } }
#  thermostat-room-1: !include { file: confs/thermostat-room.yaml, vars: { room_identifier: 1, room_name: "Room 2" } }
  remote_package:
    url: https://github.com/gekkekoe/esphome-ecodan-remote-thermostat
    ref: main
    refresh: always
    files: [ 
            confs/esp32s3.yaml,
            ## enable label language file
            confs/ecodan-labels-en.yaml,
            #confs/ecodan-labels-nl.yaml,
            #confs/ecodan-labels-it.yaml,
            #confs/ecodan-labels-fr.yaml,
            #confs/debug.yaml,
            confs/wifi.yaml,
           ]

```

* Build
```console
esphome compile ecodan-remote-thermostat-esphome.yaml
```
* To find the tty* where the esp32 is connected at, use `sudo dmesg | grep tty`. On my machine it was `ttyACM0` for usb-c, and `ttyUSB0` for usb-a. On a Mac its `tty.usbmodemxxx`
* Connect your esp32 via usb and flash
```console 
esphome upload --device=/dev/ttyACM0 ecodan-remote-thermostat-esphome.yaml
```
* You can update the firmware via the web interface of the esp after the initial flash, or use the following command to flash over the network
```console 
esphome upload --device ip_address ecodan-remote-thermostat-esphome.yaml
```

# Installation
Power down your unit (use circuit breaker!) and plug the flashed unit into the CNRF port. It's near the regular CN105 port. Turn `SW1-8` to on to enable the remote thermostat. Restore the power and select the remote thermostat as thermostat. If you are using `IN1` port, you probably need to disable it via `SW2-1`

# Using direct REST API
The esphome component will expose a REST API for the thermostats. You can use GET/POST to access/modify the current temperature. The entities will be named `room_0`, `room_1` and so on.

### Get data 
```bash
curl -X GET "http://<esp_id>/number/room_0"
# returns
{"id":"number-room_0","value":"21.5","state":"21.5"}
```

### Post data
```bash
curl -X POST "http://<esp_ip>/number/room_0/set?value=22.5" -d ""
```

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


[!["Buy Me A Coffee"](https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png)](https://www.buymeacoffee.com/gekkekoe)
