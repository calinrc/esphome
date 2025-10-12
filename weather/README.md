# Weather Station

This module is an ESPHome configuration for a weather station. It is designed to run on ESP32 devices and integrates with Home Assistant to provide real-time weather data such as temperature, humidity, pressure, and more. 

## Features
- Collects weather data from connected sensors (e.g., temperature, humidity, pressure)
- Sends sensor data to Home Assistant via ESPHome
- Customizable YAML configuration for different sensor types
- Supports OTA updates and Wi-Fi connectivity
- Low power consumption

## Usage
1. Flash the ESPHome firmware to your ESP device using the provided YAML configuration.
2. Connect the required sensors to the ESP device as described in the configuration.
3. Integrate the device with Home Assistant to monitor weather data.

## Requirements
- ESP32 device
- Supported weather sensors - DHT22
- ESPHome installed
- Home Assistant ( for integration)

## Example YAML
```yaml
esphome:
  name: weather_station

sensor:
  - platform: dht
    pin: D2
    temperature:
      name: "Weather Temperature"
    humidity:
      name: "Weather Humidity"
    model: DHT22
    update_interval: 60s
```

