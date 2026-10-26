# Heating Oil Monitor

ESPHome configuration for an ESP32-C3 SuperMini Plus and RCWL-9610 ultrasonic module, reporting to Home Assistant.

## Wiring

Power the ESP32 by USB. Sensor VCC connects to 3.3V, GND to GND, Trigger to GPIO4, and Echo to GPIO3. Leave M1 and M2 open for GPIO mode. Do not feed a 5V Echo signal into the ESP32.

## Setup

Copy oil-tank.yaml into ESPHome Device Builder. Copy the entries from secrets.example.yaml into your private secrets.yaml, replacing every placeholder. Generate the API encryption key with ESPHome. Validate, compile, and install from ESPHome; join the device through Home Assistant's ESPHome integration.

## Calibration and estimates

Two connected, equal-height vertical oval tanks, each nominally 275 US gallons. Outside dimensions per tank: 44 in high, 26.75 in wide, 58.5 in long. The model approximates the cross section with semicircular top/bottom and straight sides, normalized to nominal capacity; it is not a manufacturer calibration chart. Both tanks must equalize for doubling to be valid.

An observed 0.978236 m air gap and manual 6.25 in oil depth set an effective sensor-to-bottom offset of 44.76323 in. This is an empirical calibration offset, not a verified physical internal height. Adjust substitutions when calibration improves. Current gallon values are estimates.

## Filtering

Samples every 10 seconds, 2m echo timeout, five-sample median, initial four valid outputs suppressed, then exponential averaging (alpha 0.12). Derived readings update in dependency order. Failed echoes produce NaN after 60 seconds without valid filtered updates. Persistent false echoes can still produce incorrect levels. Averaging smooths noise but does not compensate temperature error.

Wait 5–10 minutes after a delivery for tank equalization and filtering; record settled readings before/after and the metered delivery amount to assess calibration. Software step-response and geometry checks passed; the latest averaging configuration has produced stable readings in device logs. No manufacturer-level accuracy claim is made.

## Installation suitability

This hobby sensor has not been verified as suitable for permanent heating-oil vapor exposure. Use an appropriately rated tank sensor and installation. This monitor is not an overfill prevention or leak detection system.
