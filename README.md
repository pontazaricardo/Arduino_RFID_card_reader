# Arduino_RFID_card_reader

This project shows how to use a RFID reader with Arduino, by showing how to read the embedded id inside some magnetic smartcards and smarttags.

![demo](/images/hands.gif?raw=true)

## Library

The **MFRC522** library is used. To include it in your project, just add
```arduino
#include "SPI.h"
#include "MFRC522.h"
```
to the header of your project.

## Hardware

The following hardware was used:
1. A **MF522-AN** RFID scanner.
2. An Arduino Mega 2560.
3. A LCD 1602 keypad shield. 

## Code

There are some important sections for the MFRC library that need to be addressed.

### Pin connection

At the top of your code, the following pin mapping has to be added:
```arduino
#define SS_PIN 53
#define RST_PIN 49
#define SP_PIN 50

MFRC522 rfid(SS_PIN, RST_PIN);
```
And the following pins need to be connected from the Arduino Mega to the RFID scanner:
1. MISO	- Pin 50 - SP_PIN.
2. SCK 	- Pin 52.
3. SS	- Pin 53 - SS_PIN.
4. MOSI	- Pin 51.
5. GND	- GND.
6. 3.3V	- 3.3V or 5V if the 3.3V is in use.
7. RST	- Pin 49 - RST_PIN.

![demo](/images/pic01.jpg?raw=true){:height="700px" width="400px"}
