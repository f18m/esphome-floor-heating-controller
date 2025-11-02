# floor-heating-controller

ESPHome project to present to HomeAssistant a floor heating controller, i.e. a board
that can turn on or off the flow of warm water in under-floor heating pipes.

## Highlights

* Plain simple actuator board, all the hardware is Commercial Off-The-Shelf (COTS), 
typically available on Aliexpress, and very cheap; easy to replace in future if it fails;
* Connects to your [Home Assistant](https://www.home-assistant.io/) instance via Wi-fi;
* Designed to be capable of running in a dumb, non-smart mode if Wi-fi connection drops
or your HomeAssistant instance has troubles;
* Presents _N_ basic switches plus 2 temperature sensors to Home Assistant; each switch represents an heating under-floor circuit; temperatures are sampled for both the incoming warm water and for room temperature

<img title="Overview" alt="Overview" src="images/overview.drawio.png">


## Bill Of Material

* ESP32 relay board [bought on Aliexpress](https://it.aliexpress.com/item/1005007027676026.html?spm=a2g0o.order_list.order_list_main.31.42f53696cth4st&gatewayAdapt=glo2ita) (17.5€ in Oct 2025)
Main characteristics are:

  * ESP32-WROOM-32E module
  * 5V DC power supply terminal
  * 8 relay channels, both NC and NO contacts available

* 220V to 5V power adapter (5W), bought on Aliexpress (4€ in Oct 2025)
* Temperature sensor, bought on Aliexpress (few € in Oct 2025)


## ESPHome setup

Aliexpress board pinout is:

* GPIO23: led D20
* GPIO13: relay 1
* GPIO12: relay 2
* GPIO14: relay 3
* GPIO27: relay 4
* GPIO26: relay 5
* GPIO25: relay 6
* GPIO33: relay 7
* GPIO32: relay 8

Please note that GPIO12 is a strapping PIN and should only be used for I/O with care.
Attaching external pullup/down resistors to strapping pins can cause unexpected failures.
See https://esphome.io/guides/faq.html#why-am-i-getting-a-warning-about-strapping-pins


## Similar projects

* [Smart Underfloor Heating Controller](https://hackaday.io/project/190828-smart-underfloor-heating-controller)
* [Nicolas Liaudat's Floor Heating Controller](https://github.com/nliaudat/floor-heating-controller)
