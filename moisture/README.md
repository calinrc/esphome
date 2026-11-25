# Moisture Sensor

This module is an ESPHome configuration for a soil moisture sensor. It runs on an ESP32 board (`esp32dev` in ESPHome) and reads an HW-390 resistive soil moisture probe through the ESP32's analog-to-digital converter (ADC). The readings are converted to a percentage and published to Home Assistant, both through the ESPHome native API and over MQTT.

## Features
- Reads soil moisture from an HW-390 analog output every 60 seconds
- Converts the raw voltage to a 0–100 % value with a linear calibration (dry and wet points)
- Clamps the result so it never goes below 0 % or above 100 %
- Publishes to Home Assistant through the ESPHome API and MQTT (topic prefix `sensors`)
- Supports OTA updates and Wi-Fi, with a captive portal fallback hotspot
- Sensor is exposed with `device_class: moisture`, so Home Assistant treats it as a moisture entity

## Hardware
- ESP32 development board (`esp32dev`)
- HW-390 soil moisture sensor module (analog output)
- 4 jumper wires (VCC, GND, AO, and optionally DO)
- Home Assistant with an MQTT broker (the config points to `homeassistant.lan`)

## Pinout and wiring

Connect the HW-390 module to the ESP32 as shown below. Only the analog output (AO) is used by this configuration. The digital output (DO) is optional and not read by the YAML.

| HW-390 pin | ESP32 pin | Notes |
|------------|-----------|-------|
| VCC        | 3V3       | Power from 3.3 V. Do not use 5 V, because the analog output can rise above the ESP32 ADC maximum of about 3.3 V and damage GPIO34. |
| GND        | GND       | Common ground with the ESP32. |
| AO         | GPIO34    | Analog input (ADC1, channel 6). GPIO34 is input-only, which is fine for this sensor. |
| DO         | Not connected | Optional digital threshold output. Leave unconnected for this configuration. |

Pin labels can vary slightly between HW-390 boards. Check the silkscreen on your module before wiring.

## Calibration

The sensor reports a voltage that is mapped to a percentage in `moisture.yaml`:

```yaml
calibrate_linear:
  - 2.65 -> 0.0     # dry soil / air voltage
  - 1.25 -> 100.0   # fully saturated soil voltage
```

These values are examples from one test setup. To calibrate your own sensor:

1. Flash the firmware and open the device logs or Home Assistant.
2. Hold the probe in open air (or dry soil) and note the raw voltage. Use this as the 0 % value.
3. Submerge the probe tip in a glass of water (up to the same mark as in the soil) and note the voltage. Use this as the 100 % value.
4. Replace the two numbers in `calibrate_linear` and reflash.

## Usage
1. Create a `secrets.yaml` file next to `moisture.yaml` with the keys the config uses: `wifi_ssid`, `wifi_password`, `mqtt_user`, and `mqtt_password`.
2. Flash the ESPHome firmware to the ESP32 using `moisture.yaml`.
3. Wire the sensor as described in the Pinout section.
4. Calibrate the dry and wet voltages for your probe.
5. In Home Assistant, check that the **Soil Moisture** entity appears and reports values between 0 and 100 %.

## Requirements
- ESP32 board
- HW-390 soil moisture sensor (analog output)
- ESPHome installed (CLI or the ESPHome Dashboard add-on)
- Home Assistant with the MQTT integration and a broker (for example, the Mosquitto add-on)

## Installation

1. Install ESPHome, or open the ESPHome Dashboard in Home Assistant.
2. Create the `secrets.yaml` file described in Usage.
3. Run `esphome run moisture.yaml` to compile and flash the board over USB for the first time.
4. After the first flash, use OTA updates from the dashboard or `esphome run moisture.yaml --device <ip-or-host>`.
5. In Home Assistant, go to **Settings → Devices & services** and accept the discovered ESPHome device. If it does not appear, add it manually with **Add integration → ESPHome** and the device IP address.

## Notes and recommendations
- **Corrosion:** Resistive probes corrode when powered all the time. For long-term use, power the sensor from a GPIO pin, switch it on before each reading, and switch it off afterwards. This is not included in the current YAML.
- **Secrets:** The current `moisture.yaml` contains the API encryption key and the fallback hotspot password in plain text. Move them into `secrets.yaml` before sharing or committing this repository.
- **Filtering:** Soil moisture changes slowly. A moving-average filter can smooth out noise if the readings jump around.
- **ESP32 ADC accuracy:** The ESP32 ADC is non-linear, especially near the top of its range. Keep the sensor voltage well within the calibrated range for best results.
