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

