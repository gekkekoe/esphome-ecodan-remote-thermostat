# ESPHome Ecodan Remote Thermostat
Mimics a remote thermostat over CNRF. You can use any thermostat/temp sensor as a remote thermostat. This projects supports up to 8 remote thermostats. Use any temperature sensor or thermostat as data source for the Ecodan heatpump. The required hardware is the same as used by https://github.com/gekkekoe/esphome-ecodan-hp

# available languages
English (default), Dutch, Italian, French, Spanish. Select the language in `ecodan-remote-thermostat-esphome.yaml` file. 
If you want to contribute with a translation: copy the file `ecodan-labels-en.yaml` to `ecodan-labels-xx.yaml`, fill in all the labels and submit a pull request.

# links
* [Recommended hardware](https://github.com/gekkekoe/esphome-ecodan-remote-thermostat/blob/main/docs/hardware.md)
* [Install from prebuilt binaries](https://github.com/gekkekoe/esphome-ecodan-remote-thermostat/blob/main/docs/install-from-bin.md)
* [Build from source](https://github.com/gekkekoe/esphome-ecodan-remote-thermostat/blob/main/docs/build-from-source.md)
* [Supply temperature using REST API](https://github.com/gekkekoe/esphome-ecodan-remote-thermostat/blob/main/docs/update-from-rest.md)
* [Supply temperature using Home Assistant](https://github.com/gekkekoe/esphome-ecodan-remote-thermostat/blob/main/docs/update-from-rest.md)

# Installation
Power down your unit (use circuit breaker!) and plug the flashed unit into the CNRF port. It's near the regular CN105 port. Turn `SW1-8` to on to enable the remote thermostat. Restore the power and select the remote thermostat as thermostat. If you are using `IN1` port, you probably need to disable it via `SW2-1`. Use the REST API or Home assistant to supply room temperatures.

[!["Buy Me A Coffee"](https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png)](https://www.buymeacoffee.com/gekkekoe)
