# RFID lock by OBO HANDS

## Target
https://www.aliexpress.com/item/32809023198.html
```
Rfid Metal Access Control Keypad With Waterproof Cover Contactless Door Controller Electric Security Lock+20pcs 125KHz Keychains
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

![logo]

[logo]: https://github.com/Zokol/Zokol.github.io/blob/master/_posts/RFID_lock_psu.jpg "PSU+keypad+lock circuit"


## System operation

1. PSU is powered up
2. PSU powers keypad
3. PSU sets relays to closed-state
4. Keypad outputs LOW-state to PULL-input if access is granted
5. PSU sets relays to open-state for T seconds
6. PSU sets relays to closed-state


## Keypad functions

* Access granted either on valid tag, valid private pin or valid public pin
* Private pin is disabled by default
* New tags can be programmed in configuration-mode (#881122)
* Public pin is 1234
* Keypad reads four digit sequences. Every input feeds into buffer, which is cleared after four digits. Incorrect pin results in audible signal, but keypad is still reading the next input. DeBruijn sequence cannot be used, but there is no delay between two inputs.
* By default, tamper switch is disabled. Keypad case can be opened and "open door"-input can be jumped to ground.
* By default, door alarm is disabled. This allows door to be opened without keypad granting access.


### Configuration mode

0. Set configuration pin: [0] + [new 6 digit code] + [repeat new 6 digit code] + [#]
1. Set access mode:
   * [1] + [0]: Access requires card OR pin
   * [1] + [1]: Access requires card AND pin
   * [1] + [2]: Disable private pins
   * [1] + [3]: Enable private pins
2. Set door open delay: [2] + [time in two digits (seconds)] + [#]
3. Set public pin (If public pin is set to 0000 and access-mode is card OR pin, public pin is disabled): [3] + [new 4 digit code] + [#]
4. Set tamper detection: 
   * [4] + [0]: Disable tamper detection (default)
   * [4] + [1]: Enable tamper detection
5. Enroll new card: [5] + [3 digit memory ID (001-999)], present cards to reader and finally press [#]
6. Set door contact sensor: 
   * [6] + [0]: Disable door sensor (default)
   * [6] + [1]: Enable door sensor
7. Delete card: [7] + [3 digit memory ID (001-999)] OR [7] + present card to reader and finally press [#]
8. Set door alarm (gives out alarm if door sensor opens without keypad granting access): 
   * [8] + [0]: Disable door alarm (default)
   * [8] + [1]: Enable door alarm
9. UNUSED


## Attack vectors

### Keypad
* Default public key: [1234] -> door is open
* Default maintenance key (#881122) and set public key: [3] + [0000] + [#], [0000] -> door is open.
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
