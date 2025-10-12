# esphome

## ESP32 Home assistant automations


This project is a simple ESPhome implementation over various small projects

The implementation includes:
- Low Power consumption Weather station (see wehather folder)
- Flower Moisture Monitor (see moisture folder)


## Prerequisites

### Home Assistant

#### Instalation

My deployment includes a RaspberryPI 4 (4GB Ram) provisioned with Home Assistant default image and several ESP32 devices provisoned with proper ESPHome images

Steps:
- Use RaspberryPI Imager and write Home assistant on a clean MicroSD card
- Start RPI and connect to HomeAssistant using http://homeassistant.local:8123
- Setup user and password
- Install MQTT protocol
