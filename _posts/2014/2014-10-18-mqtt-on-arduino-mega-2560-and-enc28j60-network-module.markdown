---
layout: "post"
title: "MQTT on Arduino Mega 2560 and enc28j60 network module"
date: "2014-10-18 14:48"
tags: [mqtt]
categories:
- programming
- arduino
---
# Cabling

- ENC CS -> Arduino SS (pin 53)
- ENC SI -> Arduino MOSI (pin 51)
- ENC SO -> Arduino MISO (pin 50)
- ENC SCK -> Arduino SCK (pin 52)
- ENC GND -> Arduino GND
- ENC +5V -> Arduino +5V

# MQTT server

Mosquito MQTT is used, it runs on Linux and Windows and is very easy to use, has a very low footprint and is fast.

# Arduino code
## Additionnal libraries

UIPEthernet replace standard Arduino project library. It can be downloaded on GitHub

Arduino client for MQTT is available at knolleary.net

Simply uncompress these two libraries on IDE lib folder

## The code
```c++
#include <UIPEthernet.h>
#include <PubSubClient.h>

int led = 13;

// Update these with values suitable for your network.
byte mac[]    = {  0xDE, 0xED, 0xBA, 0xFE, 0xFE, 0xED };
byte server[] = { 192, 168, 1, 15 };
byte ip[]     = { 192, 168, 1, 17 };

boolean blink = false;

EthernetClient ethClient;
PubSubClient client(server, 1883, callback, ethClient);

// Callback function
void callback(char* topic, byte* payload, unsigned int length) {
 // Copie du payload dans byte* pl pour usage dans les fonctions, car il
 // est vide lors d'un appel a publish
 byte* pl = (byte*)malloc(length);
 memcpy(pl,payload,length);

 // Test du TOPIC
 if (strcmp(topic,"input/set")==0) {
 blink = !blink;
 if(blink) {
 digitalWrite(led, HIGH);
 } else {
 digitalWrite(led, LOW);
 }
 client.publish("output/system/","set");
 }

 // Test du PAYLOAD
 if (memcmp(pl, "comtest",length)==0) {
 blink = !blink;
 if(blink) {
 digitalWrite(led, HIGH);
 } else {
 digitalWrite(led, LOW);
 }
 }
 if (memcmp(pl, "ping",4)==0) {
 client.publish("output/system/","ok");
 }

 free(pl);
}

void setup()
{
 pinMode(led, OUTPUT);
 digitalWrite(led, HIGH);
 Ethernet.begin(mac, ip);
 if (client.connect("arduinoClient")) {
 client.publish("output/system/","ready");
 client.subscribe("input/#");
 }
 digitalWrite(led, LOW);
}

void loop()
{
 client.loop();
}
```
# Testing

Run mosquito server with default configuration
```bash
/usr/sbin/mosquitto -c /etc/mosquitto/mosquitto.conf.example
```
Start arduino

Start watching output/ topic
```bash
mosquitto_sub -t "output/"
```
Send requests to arduino
```bash
mosquitto_pub -t "input/" -m "comtest"
```
# Performances

This simple “echo” program can re-send 400 request in 4 seconds
