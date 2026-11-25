# Moisture Sensor

This module is a low-power ESPHome configuration for a soil moisture sensor. It runs on an ESP32 board (`esp32dev` in ESPHome) and reads an HW-390 resistive soil moisture probe through the ESP32's analog-to-digital converter (ADC). Each cycle the device wakes up, powers the sensor, publishes one reading to Home Assistant over MQTT, and goes back to deep sleep.

## Features
- Deep sleep between measurements (default: one reading every 30 minutes)
- Sensor is powered from a GPIO pin only while it is being read
- MQTT only: no native API, no OTA updates, no captive portal or fallback access point
- Converts the raw voltage to a 0–100 % value with a linear calibration (dry and wet points)
- Clamps the result so it never goes below 0 % or above 100 %
- Sensor is exposed with `device_class: moisture`, so Home Assistant treats it as a moisture entity

## Hardware
- ESP32 development board (`esp32dev`)
- HW-390 soil moisture sensor module (analog output)
- 4 jumper wires (VCC, GND, AO, and optionally DO)
- Home Assistant with an MQTT broker (the config points to `homeassistant.lan`)

## Pinout and wiring

The sensor is powered from GPIO27 instead of a fixed 3.3 V rail, so the probe is only energised during a reading. This is the main power saving in this configuration.

| HW-390 pin | ESP32 pin     | Notes |
|------------|---------------|-------|
| VCC        | GPIO27        | Switched 3.3 V supply, driven by the `sensor_power` output. Do not connect to 5 V: the analog output can rise above the ESP32 ADC maximum of about 3.3 V and damage GPIO34. |
| GND        | GND           | Common ground with the ESP32. |
| AO         | GPIO34        | Analog input (ADC1, channel 6). GPIO34 is input-only, which is fine for this sensor. |
| DO         | Not connected | Optional digital threshold output. Leave unconnected for this configuration. |

Pin labels can vary slightly between HW-390 boards. Check the silkscreen on your module before wiring.

## Configuration

The main settings are at the top of `moisture.yaml`:

| Substitution       | Default     | Purpose |
|--------------------|-------------|---------|
| `sleep_duration`   | `30min`     | Time between measurements |
| `sensor_power_pin` | `GPIO27`    | Pin that switches sensor VCC |
| `sensor_pin`       | `GPIO34`    | ADC pin that reads the AO output |

`deep_sleep.run_duration` is set to 45 seconds. If Wi-Fi or MQTT is slow or fails, the device still goes back to sleep after this time.

## Calibration

The sensor reports a voltage that is mapped to a percentage in `moisture.yaml`:

```yaml
calibrate_linear:
  - 2.65 -> 0.0     # dry soil / air voltage
  - 1.25 -> 100.0   # fully saturated soil voltage
```

These values are examples from one test setup. Because the device sleeps between readings, you need to read the raw voltage from Home Assistant after a cycle, or from the device logs during a manual test. To calibrate your own sensor:

1. Hold the probe in open air (or dry soil) and note the raw voltage. Use this as the 0 % value.
2. Submerge the probe tip in a glass of water (up to the same mark as in the soil) and note the voltage. Use this as the 100 % value.
3. Replace the two numbers in `calibrate_linear` and flash the device again over USB.

## Usage
1. Create a `secrets.yaml` file next to `moisture.yaml` with the keys the config uses: `wifi_ssid`, `wifi_password`, `mqtt_user`, and `mqtt_password`.
2. Flash the ESPHome firmware to the ESP32 using `moisture.yaml` (see Installation).
3. Wire the sensor as described in the Pinout section.
4. Calibrate the dry and wet voltages for your probe.
5. In Home Assistant, check that the **Soil Moisture** entity appears and updates once per sleep interval, with values between 0 and 100 %.

## Requirements
- ESP32 board
- HW-390 soil moisture sensor (analog output)
- ESPHome installed (CLI or the ESPHome Dashboard add-on)
- Home Assistant with the MQTT integration and a broker (for example, the Mosquitto add-on)

## Installation

1. Install ESPHome, or open the ESPHome Dashboard in Home Assistant.
2. Create the `secrets.yaml` file described in Usage.
3. Connect the ESP32 by USB and run `esphome run moisture.yaml` to compile and flash it.
4. Updates after this first flash must also be done over USB, because OTA is disabled.
5. In Home Assistant, check that the MQTT device appears under **Settings → Devices & services → MQTT**. The sensor is published with MQTT discovery and topic prefix `sensors`.

## Notes and recommendations
- **Corrosion:** The probe is powered only during a reading, which reduces corrosion compared with continuous power.
- **Updating firmware:** With OTA and the API removed, the device can only be reflashed by USB. Keep the ESP32 accessible, or temporarily re-enable OTA in `moisture.yaml` if you need remote updates.
- **Debugging:** Logging is set to `WARN` to save time awake. Raise `logger: level` to `DEBUG` while calibrating or troubleshooting.
- **Secrets:** Keep `wifi_ssid`, `wifi_password`, `mqtt_user`, and `mqtt_password` in `secrets.yaml`. Do not commit that file.
- **Filtering:** Soil moisture changes slowly, so a moving-average filter can be added if readings are noisy. Each cycle takes only one reading, so averaging would need several readings per cycle.
- **ESP32 ADC accuracy:** The ESP32 ADC is non-linear, especially near the top of its range. Keep the sensor voltage well within the calibrated range for best results.
