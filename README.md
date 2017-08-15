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

![demo](/images/pic01.jpg)

### Initialization

To initialize the library, just add in your *setup()* the following:

```arduino
void setup() {
  Serial.begin(9600);
  SPI.begin();
}
```
Note: This project prints the tag id to the serial port where the Arduino is connected. If you want to use a different port or different baudrate, change the port and baudrate in the previous code.

### Main 

In the *loop()*, we have

```arduino
void loop() {
if (!rfid.PICC_IsNewCardPresent() || !rfid.PICC_ReadCardSerial())
    return;
	...
}
```
This code keeps waiting until a smarttag is read by the RFID scanner. When this happens, it continues with the rest of the code:

```arduino
void loop() {
  if (!rfid.PICC_IsNewCardPresent() || !rfid.PICC_ReadCardSerial())
    return;

  MFRC522::PICC_Type piccType = rfid.PICC_GetType(rfid.uid.sak);

  // Check is the PICC of Classic MIFARE type
  if (piccType != MFRC522::PICC_TYPE_MIFARE_MINI &&
    piccType != MFRC522::PICC_TYPE_MIFARE_1K &&
    piccType != MFRC522::PICC_TYPE_MIFARE_4K) {
    Serial.println(F("Your tag is not of type MIFARE Classic."));
    return;
  }

  String strID = "";
  for (byte i = 0; i < 4; i++) {
    strID +=
    (rfid.uid.uidByte[i] < 0x10 ? "0" : "") +
    String(rfid.uid.uidByte[i], HEX) +
    (i!=3 ? ":" : "");
  }
  strID.toUpperCase();

  Serial.print("Tap card key: ");
  Serial.println(strID);

  rfid.PICC_HaltA();
  rfid.PCD_StopCrypto1();

}
```
Here, the id is print to serial port in 
```arduino
Serial.print("Tap card key: ");
Serial.println(strID);
```
so if you want to print to a different port, change *Serial* for your new port and remember to initialize it in the *setup()*.

## Execution

After implementing this code and connecting the RFID scanner to the Arduino Mega, we can read the tag ids of different smartcards in serial monitor.