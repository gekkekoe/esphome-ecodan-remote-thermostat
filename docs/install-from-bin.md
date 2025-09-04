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