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