# simple-heater-control-system
WOKWI Link: https://wokwi.com/projects/436889231774615553


## Overview

This project implements a basic heater control system on an Arduino UNO, simulating both real time temperature sensing and manual override using a potentiometer where temperatures can be adjusted using the dial. It tracks five states (Idle, Heating, Stabilizing, Target Reached, Overheat), drives LEDs and a buzzer for visual/audible feedback, and logs state and temperature over Serial Monitor, while obtaining actual BLE features wasn't quite possible, I have simulated it to look like BLE advertising.

This entire coding has been done to fit Arduino IDE and not ESP-IDF, using C++ language.

## Components & Connections

* **Arduino UNO**
* **NTC Thermistor Module**
* **Potentiometer (for override)**
* **Heater Indicator LED** 
* **State Indicator LED**
* **Buzzer**

## The Code:

* **State Machine:** Five states based on `temp` compared against thresholds can be seen on adjusting the dial of the potentiometer:

  * **Idle:** `temp < target - tolerance`
  * **Heating:** `target - tolerance ≤ temp < target`
  * **Stabilizing:** `target ≤ temp < target + tolerance`
  * **TargetReached:** `target + tolerance ≤ temp < overheat`
  * **Overheat:** `temp ≥ overheat` (blinks LED, shrieking buzzer)
* **Thresholds Configurable:** `targetTemp = 25 °C`, `tolerance = 2 °C`, `overheatTemp = 35 °C`.
* **Visual/Audible Feedback:**

  * Heater LED switches ON during Heating.
  * State LED turns OFF/ON/blinking for Idle/Heating/Overheat.
  * Buzzer sets off when overheating with 2 kHz pulse.
* **Serial Output:** Every second prints:
  RealTemp: XX.X C,  UsedTemp: ZZ.Z C
  State: <STATE>
  Action: <ACTION>
  BLE ADV => State=<STATE>, Temp=ZZ.Z C
  ```

## Setting Up

->**Run the Wokwi simulator** and open Serial Monitor at 9600 baud.
->**Observe real NTC readings** on startup (steady value).
->**Turn the potentiometer** (dial) to manually increase `UsedTemp`.
-> **Watch states cycle** as `temp = max(RealTemp, UsedTemp)` moves through:

   * Idle (<23 °C)
   * Heating (23–25 °C)
   * Stabilizing (25–27 °C)
   * TargetReached (27–35 °C)
   * Overheat (≥35 °C)
->**Visual/Audible cues** confirm each state:
   * **Heater LED** lights in Heating.
   * **State LED** blinks in Overheat.
   * **Buzzer** shrieks in Overheat.


