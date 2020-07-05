# RFID lock by OBO HANDS

https://www.aliexpress.com/item/32809023198.html
```
Rfid Metal Access Control Keypad 
With Waterproof Cover Contactless Door Controller 
Electric Security Lock+20pcs 125KHz Keychains
```

## Specs

```
• Security Mode: Fail Safe
• Working Voltage: DC 12V
• Working temperature: 0~65 °C
• Storage capacity: 1000 users
• Frequency: 125KHz
• Read range: 0-15 cm
• Material: Metal
```

## Circuit

* PSU / Control unit
* Keypad / lock
* Relay / latch / bolt

*Insert picture of PSU+keypad+lock circuit*


## Attack vectors

### Keypad
* Default entrance key (1234)
* Default maintenance key (#881122) and register your own tag
* **Failed** Dump firmware with keys, not possible with STC microcontrollers, as bootloader does not support reading of the non-volatile memory: https://github.com/grigorig/stcgal/issues/7
* Clone existing key using T5577 tag
* **Failed** Bruteforce keys. Emulating key usign Proxmark 3 does not work due to SW issue.

### Keypad internals
* Remove screw and jump "door"-button to ground (tamper detection switch is disabled by default)
* Tamper detection switch can be bypassed with suitable pick (1x1mm hole next to switch, allowing pick to hold switch closed while opening the case)

### PSU
* Connect 9V battery to Control-pins

### Wiring harness
* Cut NO latch wire -> relay loses power and latch opens

### Door
* Use magnet to pull NC latch open
