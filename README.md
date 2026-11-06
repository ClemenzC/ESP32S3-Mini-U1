# ESP32S3-Mini-U1-DevBoard
##Description: 
A simple ESP32S3MiniU1 Devboard designed by JLCEDA std.

##Features:
- Multiple Burning Modes Supported
- JTAG Debug Supported
- Optional Outside High-Precision Clock
- USB Electrostatic Discharge
- Wide Input Voltage Ranges 2.2-5V
- CH340N LED Indicators

##Document:
-References:
  See *[TechManual](https://item.szlcsc.com/datasheet/ESP32-S3-MINI-1U-N8/3401551.html)* for ESP32S3MiniU1.

##Debug:
Boot:
*!Notice: The status of GPIO0 will be locked after reset or power-up then it could be used as normal GPIO.*
- Lauch(Normal)
  GPIO0(BOOT) has to be held high(internally pulled up by default; floating means high).
- Download Mode(waiting for burning)
  GPIO0 must be pulled low during reset. Note that because it has an internal weak pull-up of about 45 kΩ, an external   button to ground needs a strong enough pull-down (e.g., a 10 kΩ resistor or a direct short) to reliably pull it low.
- Reset:
  Pull EN low and hold it for at least 50 μs, then release it; the chip will reboot.
- Power-up sequence:
  After power is applied, wait for the supply to stabilize (approximately 50 μs). EN must be pulled high to activate the chip.

#ESP32 Reset & Boot Mode Table

|RST (EN) Status|BOOT (GPIO0) Level at Reset Release|Chip Behavior|Typical Use|
|---------------|-----------------------------------|-------------|-----------|
|Low (held ≥50μs)|Any|Chip is in reset/off state, not running|Force shutdown, wait for power stabilization|
|Low → High (reset released)|High (floating/pulled up)|Normal boot: loads and runs existing firmware from Flash|Daily operation, power-on boot|
|Low → High (reset released)	|Low (pulled down/button to GND)|Download mode: enters ROM serial/USB download, waiting to be flashed|Flashing firmware, manually entering download mode|
