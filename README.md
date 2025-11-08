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

Note: You need a wired connection for first boot and  first startup migh take up to 20 minutes until the environment is fully functional

- Create My Smart Home, Setup user and password, region etc
- (Optional) Make Home Assistant run on wireless network `Settings` -> `System` -> `Network` -> `Configure network interfaces` -> `wlan0`

- Install MQTT Server `Settings` -> `Add-ons` -> `Add-on Store` -> `Mosquitto broker` -> `Install`
    - Config: `Settings` -> `Add-ons` -> `Mosquitto broker` -> `Configuration` -> `Options Login` -> `Add` (Add a login and password for Mosquitto Service)
    - Start Service: `Settings` -> `Add-ons` -> `Mosquitto broker` -> Start

    Note: From Linux you can explore MQTT Using MQTT Explorer (Use the above username and password to connect to your instance)
- Install ESP Home Support: `Settings` -> `Add-ons` -> `Add-on Store` -> `ESPHome Device Builder` -> `Install` -> `Start` -> `Open WebUI`

### ESPHome

#### [Weather](./weather/README.md)
#### [Moisture](./moisture/README.md)

