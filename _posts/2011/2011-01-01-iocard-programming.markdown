---
layout: "post"
title: "IOCard programming"
date: "2011-01-01 15:07"
tags: [iocp,fsx]
categories:
- programming
---
IOcard is directly connected to PC parallel port, using a standard DB25 cable. IOcard consist on a network of multiplexed addressable latches. Programming is done by controlling parallel port signals.
PPORT_BASE is parallel port base address (0×378 for parallel port #1).

Parallel port has three registries:

- PPORT_BASE, DATA registry (I/O, but used as Output only by IOcard)
- PPORT_BASE+1, STATUS registry (Input or Ouput depending on pin, IO card only use Input)
- PPORT_BASE+2, CONTROL registry, like STATUS for I/O

# Programming output

Output number is defined by PIN identification on J2 connector (it is an arbitrary choice). We then calculate the real value presented on parallel port using this formula:
```
x = (((output/8)+1)<<3 + (output%8);
if (state) x += 64;
```
where output is the J2 pin number, state if true or false (ON or OFF) and x is the value presented on parallel port.

This value is presented to addressable latch, and then memorised:
```
    Out32(PPORT_BASE,x);
    Out32(PPORT_BASE, x + 128);
    Out32(PPORT_BASE,7);
```
# Reading input

Input consist of groups of 9 input lines. Each group is scanned by programming a latch to select appropriate group.

Info: see bug #000001 about line#5 initialisation.
OnePanel programming

Detailed electronic schema can be found in the OnePanel Manual.

The "high level" library to program directly OnePanel is handled by npanel.h header. Each OnePanel specific function is implemented. For example, a function controls the Landing Gear indicator, another controls the flaps angle indicator.
Reading Selector input

The selector has 12 positions, and is directly connected to a 9 input group. Then, 3 positions are leaved in "hiZ" state. The software aims to manage changes on input lines (also in values given by FSUIPC). So, each change is managed like an event. The driver knows what was the previous selector position, guess what is the actual position and returns it.
Reading Rotary knob input

The rotary knob returns an offset, with value +1 or -1. The rotary knob is based on a 12 positions rotary switch (the same as selector switch), but inputs are connected 4 by 4, so it creates 3 groups, called A,B and C. If we change from A to B, or B to C, or C to A, the offset is +1. Offset is -1 else. See source code for implementation details.
