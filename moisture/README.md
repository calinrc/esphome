# Moisture Sensor

This module is an ESPHome configuration for a moisture sensor. It is designed to run on ESP32 devices and integrates with Home Assistant to provide real-time mnoisture data. 


## Features
- Collects moisture data from connected sensors 
- Push sensor data to Home Assistant via ESPHome using MQTT protocol
- Customizable YAML configuration for different sensor types
- Supports OTA updates and Wi-Fi connectivity
- Low power consumption

## Usage
1. Flash the ESPHome firmware to your ESP device using the provided YAML configuration.
2. Connect the required sensors to the ESP device as described in the configuration.
3. Integrate the device with Home Assistant to monitor weather data.

## Requirements
- ESP32 device
- Supported weather sensors - DHT11
- ESPHome installed
- Home Assistant ( for integration)

## Installation


Create a new ESpDevice and upload proper yaml file
On Settings -> Device & services -> New device -> Add


### Pinout