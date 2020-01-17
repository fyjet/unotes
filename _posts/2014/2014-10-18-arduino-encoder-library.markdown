---
layout: "post"
title: "Arduino encoder library"
date: "2014-10-18 14:53"
tags: [encoder]
categories:
- programming
- arduino
---
Encoder library is published by [pjrc](http://www.pjrc.com/teensy/td_libs_Encoder.html)
# The code
```c++
#include "Encoder.h"

 Encoder encoder(2, 4);

 void setup() {
 attachInterrupt(0, doEncoder, CHANGE);
 Serial.begin (115200);
 Serial.println("start");
 }

 void loop(){
 // do some stuff here - the joy of interrupts is that they take care of themselves
 }

 void doEncoder(){
 encoder.update();
 Serial.println( encoder.getPosition() );
 }
 ```
