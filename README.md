# ESP32S3-Mini-U1-DevBoard
##Description: <br>
A simple ESP32S3MiniU1 Devboard designed by JLCEDA std.<br>
Repository URL: https://github.com/ClemenzC/ESP32S3-Mini-U1

## Features:<br>
- Multiple Burning Modes Supported
- JTAG Debug Supported
- Optional External High-Precision Clock
- USB Electrostatic Discharge
- Wide Input Voltage Ranges 2.2-5V
- CH340N LED Indicators

## Document:<br>
- References:<br>
  See *[techmanual](https://item.szlcsc.com/datasheet/ESP32-S3-MINI-1U-N8/3401551.html)* for ESP32S3MiniU1.

## Debug:<br>
### Boot:<br>
*Note: the status of GPIO0 will be locked after reset or power-up then it could be used as normal IO.*
- Lauch(Normal)<br>
  GPIO0(BOOT) has to be held high(internally pulled up by default; floating means high).
- Download Mode(waiting for burning):<br>
  GPIO0 must be pulled low during reset. Note that because it has an internal weak pull-up of about 45 kΩ, an external   button to ground needs a strong enough pull-down (e.g., a 10 kΩ resistor or a direct short) to reliably pull it low.
  
### Reset:<br>
- Pull EN low and hold it for at least 50 μs, then release it; the chip will reboot.
- Power-up sequence:
  After power is applied, wait for the supply to stabilize (approximately 50 μs). EN must be pulled high to activate the chip.
  
### Built-in & External JTAG:<br>
*Note: Built-in JTAG vs. external JTAG.*

It should be noted that the ESP32-S3 has an integrated JTAG circuit. By default, the chip's JTAG interface is connected to the built-in USB_SERIAL_JTAG peripheral, so you only need to connect GPIO19 (D-) and GPIO20 (D+) via a USB cable to perform debugging; the aforementioned GPIO39–42 are not required.
Only when using an external JTAG debugger (such as ESP-Prog) do you need to connect the debugger's TMS/TDO/TDI/TCK to the chip's GPIO42/40/41/39 respectively. In addition, to enable this group of GPIOs as the JTAG interface, you usually also need to disable the built-in USB JTAG function by configuring eFuse (for example, burning DIS_USB_JTAG).

# Appendix

## ESP32 Reset & Boot Mode Table<br>

|RST (EN) Status|BOOT (GPIO0) Level at Reset Release|Chip Behavior|Typical Use|
|---------------|-----------------------------------|-------------|-----------|
|Low (held ≥50μs)|Any|Chip is in reset/off state, not running|Force shutdown, wait for power stabilization|
|Low → High (reset released)|High (floating/pulled up)|Normal boot: loads and runs existing firmware from Flash|Daily operation, power-on boot|
|Low → High (reset released)	|Low (pulled down/button to GND)|Download mode: enters ROM serial/USB download, waiting to be flashed|Flashing firmware, manually entering download mode|
|High (during normal operation)|High → Low (pulled low while running)|No effect: GPIO0 is already latched, used only as a normal IO|Using GPIO0 as a regular input/output|
|High (during normal operation)|Low → High (released while running)|No effect: no boot mode switch is triggered|Normal IO operation|
|High (during normal operation)|Any|Only pulling RST low during operation triggers a reset; changing BOOT alone has no effect|N/A|
|Low → High (reset released)|Low, and GPIO46 is High|May fail to enter download mode (S3 requires GPIO46 to be Low or floating)|Troubleshooting download failures|Boot failure, may repeatedly reboot or enter ROM to wait
|Low → High (reset released)|High, but Flash has no valid firmware|Boot failure, may repeatedly reboot or enter ROM to wait|N/A|

## JTAG Pin Definition<br>

|JTAG Signal|ESP32-S3 Pins|Definition|
|-----------|-------------|----------|
|TMS|GPIO42(MTMS)|Test Mode Select|
|TDO|GPIO40(MTDO)|Test Data Output|
|TDI|GPIO41(MTDI)|Test Data Input|
|TCK|GPIO39(MTCK)|Test Clock|


