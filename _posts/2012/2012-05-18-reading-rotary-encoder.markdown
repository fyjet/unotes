---
layout: "post"
title: "Reading Rotary Encoder"
date: "2012-05-18 14:55"
tags: [encoder]
categories:
- programming
- arduino
---
The XOR logical table can help with reading and computing rotary encoder output.

Rotary encoder has 2 outputs, that returns a state, using “grey” code. When encoder turns un Clockwise(CW), outputs are 01,00,10,11 and Counter Clockwise(CCW) outputs are 01,11,10,00.
Advantage of grey code is that value between two states only change from 1 bit (it differs from binary code).
# Detecting position change

If encoder position change (rotating), only 1 bit change on output code. This ca be detected by operating a XOR on actual value and previous value. If it stay “0″, encoder does not change, if “1″, it has moved.

# Detecting direction

It can be done by operating a simple WOR between OLD B value, and new A value. If result is “1″, direction is CW, if CCW, direction is CCW.

More reading:

- [http://fr.wikipedia.org/wiki/Fonction_OU_exclusif](http://fr.wikipedia.org/wiki/Fonction_OU_exclusif)
- See PIC application notes on RotaryEncoder
- [Arduino library example to read 3 encoders](http://code.google.com/p/phi-prompt-user-interface-library/downloads/detail?name=phi_interfaces_V100.zip&can=2&q=#makechanges)
- [Other simple Arduino example](http://www.circuitsathome.com/mcu/programming/reading-rotary-encoder-on-arduino)
