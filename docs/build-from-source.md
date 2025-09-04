# build esphome-ecodan-remote-thermostat firmware
### Build via cmd line
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
