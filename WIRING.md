# YF-B7 + ESP32 wiring

This project uses an ESP32-WROOM-32D development board with a YF-B7 G1/2 DN15 water-flow sensor and its integrated NTC temperature sensor.

## Flow sensor wiring

The YF-B7 3-wire flow connector is:

- **Red**: +5 V
- **Black**: GND
- **Yellow**: pulse output

Use a resistor divider on the yellow signal before connecting it to GPIO27:

```text
YF-B7 yellow ----[ 10 kΩ ]----+---- GPIO27 / D27
                               |
                             [ 20 kΩ ]
                               |
                              GND

YF-B7 red -------------------------- VIN / 5V
YF-B7 black ------------------------ GND
```

The 10 kΩ resistor is in series with the yellow wire. The 20 kΩ resistor goes from the GPIO27 side of the 10 kΩ resistor to ground.

This divides a nominal 5 V pulse to about 3.33 V for the ESP32 input.

## Temperature sensor wiring

The 2-wire temperature connector is a 50 kΩ NTC thermistor (B≈3950). The two thermistor leads are not polarised.

Use a 47 kΩ fixed resistor as the upper half of the divider:

```text
3.3V ----[ 47 kΩ ]----+---- GPIO34 / D34
                       |
                  YF-B7 NTC
                       |
                      GND
```

In other words:

- ESP32 3V3 -> 47 kΩ resistor
- Other end of 47 kΩ -> GPIO34 and one NTC wire
- Other NTC wire -> GND

## Complete connection summary

```text
ESP32-WROOM-32D                  YF-B7
----------------                  -----
VIN / 5V -----------------------> Red
GND ----------------------------> Black
GPIO27 <---- divider <----------- Yellow

3V3 ---- 47 kΩ ----+---- GPIO34
                    |
                    +---- NTC wire 1
GND --------------------- NTC wire 2
```

## Resistors used

- 10 kΩ: in series with YF-B7 yellow flow signal
- 20 kΩ: GPIO27 signal node to GND
- 47 kΩ: 3.3 V to GPIO34/NTC divider node

## Notes

- Keep all grounds common.
- Do not connect a 5 V flow pulse directly to an ESP32 GPIO.
- The YAML starts with a flow calibration of 660 pulses/L; calibrate against a known volume for best accuracy.
- GPIO34 is input-only, which makes it suitable for the temperature ADC input.
