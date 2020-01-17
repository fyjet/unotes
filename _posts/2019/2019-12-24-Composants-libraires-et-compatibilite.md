---
layout: post
title: Composants, librairies et compatiblite
categories:
- programming
- arduino
---
# Capteurs

device | type | lib | testé | description
-- | -- | -- | -- | --:
GPS GY NEO-6M | GPS série 9600 Bauds | harwareserial | non | GY-271M Compas et accéléromètre 3 axes
18B20 | sonde température 1Wire | 1wire | bpi, rpi | 
BMP280 | sonde pression et température i2c | | bpi, rpi | 
SCT-013-00 | Pince ampèremétrique | | non | 

# Afficheurs

device | type | lib | testé | description
-- | -- | -- | -- | --:
ILI9341 | Écran TFT I2C ILI9341 240x320 | LiquidCristal_I2C | esp32 | 
2004 I2C | Écran LCD 2004 I2C | liquidcristaldisplay_i2c | esp8266 esp32 |

# Communication radio

device | type | lib | testé | description
-- | -- | -- | -- | --:
FS1000A 433 |Émetteur générique 433 MHz | X10RF | arduino pro mini | 

# Communication série

device | type | lib | testé | description
-- | -- | -- | -- | --:
JY-MCU | Adaptateur usb série | | arduino pro mini | (pas de reset lors de l'upload du sketch, faire un reset manuel lorsque le télépchargement commence, ou utiliser un adaptateur FTDI)
FT232RL | Adaptateur usb série FTDI | | arduino pro mini |

Connections FTDI USB-to-TTL 6pins to Arduino Mini Pro:

-   FTDI USB-to-TTL: GND (marron) -> Arduino Mini Pro GND
-   FTDI USB-to-TTL: CTS (rouge) -> Arduino Mino Pro GND
-   FTDI USB-to-TTL: +5Vcc (orange) -> Arduino Mini Pro VCC
-   FTDI USB-to-TTL: TX (jaune) -> Arduino Mini Pro RX
-   FTDI USB-to-TTL: RX (vert) -> Arduino Mini Pro TX
-   FTDI USB-to-TTL: DTR (bleu) -> Arduino Mini Pro Reset (DTR)
