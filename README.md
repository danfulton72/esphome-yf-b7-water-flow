# ESPHome YF-B7 Water Flow + Temperature Sensor

ESPHome configuration and wiring notes for integrating a **YF-B7 G1/2 DN15 water-flow sensor with temperature sensor** into Home Assistant using an **ESP32-WROOM-32D** development board.

## Files

- [`water-meter.yaml`](./water-meter.yaml) — ESPHome configuration
- [`WIRING.md`](./WIRING.md) — detailed resistor placement and wiring
- `wiring-diagram.png` — visual wiring diagram

## What it exposes to Home Assistant

- Water Flow Rate (L/min)
- Water Total (L)
- Water Total (m³)
- Water Usage Today (L)
- Water Temperature (°C)

## Hardware

- ESP32-WROOM-32D / ESP32 DevKit
- YF-B7 water flow sensor
- 10 kΩ resistor
- 20 kΩ resistor
- 47 kΩ resistor

## Important electrical note

The YF-B7 flow signal is level-shifted with a **10 kΩ / 20 kΩ voltage divider** before GPIO27. Do not feed a 5 V pulse directly into an ESP32 GPIO.

The NTC temperature input uses a **47 kΩ fixed resistor** with the sensor thermistor on GPIO34.

See [`WIRING.md`](./WIRING.md) for the exact connections.

## Calibration

The YAML starts with approximately **660 pulses per litre** (`0.00151515` L per pulse). For best accuracy, run a known volume through the sensor and adjust the multiplier if necessary.
