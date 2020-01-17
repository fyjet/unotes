---
layout: "post"
title: "UBW32 technical notes"
date: "2011-01-02 15:03"
tags:  ubw32
categories:
- programming
---
Today is a great day ! You just receive your UBW32 board from Sparkfun. Unfortunately, no documentation is provided…
No problem, having a look at [schmalzhaus website](http://www.schmalzhaus.com/UBW32/doc/UBW32Documentation_v1_3.html) will help us to discover this card.

# Steps to play with UBW32

- Plug your UBW32 on USB port and install driver available [here](http://www.schmalzhaus.com/UBW32/FW/UBW32inf.zip).
- Run a terminal emulator (such as putty for example) and set com port to COM5

Try it now !

Type instructions (list is available [here](http://www.schmalzhaus.com/UBW32/doc/UBW32Documentation_v1_3.html))

Type “V” then press enter:
```
V
UBW32 Version 1.4
OK
```
If you make a mistake, don’t hesitate to run the “R” command (that will reset the UBW).

You can individually set pin state using “PO”, or grouping all set with “O”.

To set on the orange led, set port E0 to 0 (let output are inverted). For red let, set port E1 and for white led, set port E2 to 0.
```
PO,E,2,0
PO,E,2,1
```
will light on white led, and then light off
Use Bootloader to load new firmware

- Unplug UBW32 from USB connector
- Keep pressing the “PRG” button and connect USB connector, wait 3 seconds and release “PRG”
- Once white/green leds are blinking, run HIDBootloader (visit Microchip website)
- Load HEX file
- Press RESET button

# StickOS on my UBW32

StickOS is an operating system designed for micro-controller that embed a Basic compiler. It can be downloaded at [StickOs website](http://www.cpustick.com/downloads.htm). Select “StickOS for Microchip PIC32MX4-F512L UBW32 Board Bootloader” and upload to UBW32 using HIDBootloader as described.

The reset the UBW. Download INF file from StickOs [here](http://www.cpustick.com/cpustick.inf) and install if requested (this will download a Virtual Serial port driver from Microsoft website).

Run Putty and talk with StickOS. Documentation is available [here](http://www.cpustick.com/downloads/stickos.v1.80.2p.pdf).
