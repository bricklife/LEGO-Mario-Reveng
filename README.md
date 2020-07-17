# LEGO Mario Reverse Engineering

## Overview

### LEGO Mario

LEGO Mario is a BLE device which supports [LEGO Wireless Protocol](https://lego.github.io/lego-ble-wireless-protocol-docs/). It has five input devices as follows:
- Port 0: `0x47` Accelerometer, Gesture?
- Port 1: `0x49` Color Barcode, RGB Color
- Port 2: `0x4a` Pants
- Port 3: `0x46` Events, Debug, and something
- Port 6: `0x14` Voltage

### Color Barcode

Each color barcode is drawn with five out of [nine (eight?) colors](Images/barcode-colors.png). The first two lines are always green and red as header part. The other three lines are code part. The value can be `0x01` - `0xd2` = `7 * 6 * 5`.
- e.g. <img src="Images/barcode-02.jpg" width="100" /> is `0x02`. <img src="Images/barcode-b8.jpg" width="100" /> is `0xb8`.

### Pants

Each pants has a unique 6 bit code. [The pants sensor](Images/pants-code.jpg) at the bottom of LEGO Mario can detect it physically.
- e.g. The default pants is `100001` = `0x22`.

## LEGO Wireless Protocol

### Manufacturer Data

```
97 03 00 43 0a 00 41 00
```
- System Type and Device Number: `0x43` LEGO System - Mario?
- Device Capabilities: `0x0a` Supports Peripheral Role, Act as a Remote Controller (R/C)

### Port Information [0x43]

| Port ID | IO Type | Capabilities | Input Modes | Output Modes | Possible Mode Combination |
| --- | --- | --- | --- | --- | --- |
| 0 | 0x47 | Logical Combinable, Input | 0, 1 | - | [0, 1] |
| 1 | 0x49 | Logical Combinable, Input | 0, 1 | - | [0, 1] |
| 2 | 0x4a | Input | 0 | - | - |
| 3 | 0x46 | Input | 0, 1, 2, 3 | - | - |

### Port Mode Information [0x44]

| Port ID | Mode | Name | RAW Range | PCT Range | SI Range | Symbol | Mapping | Value Format |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 0 | 0 | RAW | 0 - 100 | 0 - 100 | 0 - 100 | cnt | Supports NULL, Discrete | 3 * 8 bit |
|  | 1 | GEST | 0 - 10 | 0 - 100 | 0 - 10 | raw | Supports NULL, Discrete | 2 * 16 bit |
| 1 | 0 | TAG | 0 - 10 | 0 - 100 | 0 - 10 | idx | Supports NULL, Discrete | 2 * 16 bit |
|  | 1 | RGB | 0 - 10 | 0 - 100 | 0 - 10 | raw | Supports NULL, Discrete | 3 * 8 bit |
| 2 | 0 | PANT | 0 - 63 | 0 - 100 | 0 - 63 | idx | Supports NULL, Discrete | 1 * 8 bit |
| 3 | 0 | CHAL | 0 - 65535 | 0 - 100 | 0 - 65535 | na | Supports NULL, Discrete | 2 * 16 bit |
|  | 1 | VERS | 0 - 255 | 0 - 100 | 0 - 255 | na | Supports NULL, Discrete | 4 * 32 bit |
|  | 2 | EVENTS | 0 - 65535 | 0 - 100 | 0 - 65535 | na | Supports NULL, Discrete | 2 * 16 bit |
|  | 3 | DEBUG | 0 - 65535 | 0 - 100 | 0 - 65535 | na | Supports NULL, Discrete | 4 * 32 bit |
