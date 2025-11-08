# Weather Sensor

This module is an ESPHome configuration for a weather station. It is designed to run on ESP32 devices and integrates with Home Assistant to provide real-time weather data such as temperature, humidity, pressure, and more. 

## Features
- Collects weather data from connected sensors (e.g., temperature, humidity)
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


|   DTH 11   |  ESP32 |
|--------|------------|
| Pin 1  | VCC (3.3V) |
| Pin 2  |  D26       |
| Pin 3  |  ---       |
| Pin 4  |  GND       |

Note: between VCC and data ping (Pin 1 and Pin 2) put a 10K resistor

 