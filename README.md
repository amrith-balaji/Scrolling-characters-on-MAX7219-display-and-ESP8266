# Scrolling-characters-on-MAX7219-display-and-ESP8266

A program to show the scrolling text capabilities of the MAX7219 display using the MD_MAX72xx library in arduino.

The library (MD_MAX72xx) can be installed using the boards manager in the Arduino IDE or can be installed from the github link (provided below) :
``` https://github.com/MajicDesigns/MD_MAX72XX ```

## Pinout:
| MAX7219  | ESP8266 |
| ------------- | ------------- |
| VIN -->  | 3V  |
| GND --> | GND  |
| DIN --> | D7 |
| CS --> | D8 |
| CLK --> | D5 |

## Code:
```
#include <MD_MAX72xx.h>
#include <SPI.h>

#define  DEBUG  1
#if  DEBUG
#define	PRINT(s, x)	{ Serial.print(F(s)); Serial.print(x); }
#define	PRINTS(x)	Serial.print(F(x))
#define	PRINTD(x)	Serial.println(x, DEC)
#else
#define	PRINT(s, x)
#define PRINTS(x)
#define PRINTD(x)
#endif

#define	MAX_DEVICES	8

#define CLK_PIN   14 //D5
#define DATA_PIN  13 //D7
#define CS_PIN    15 //D8

MD_MAX72XX mx = MD_MAX72XX(CS_PIN, MAX_DEVICES);

#define  DELAYTIME  100

void scrollText(char *p)
{
  uint8_t	charWidth;
  uint8_t	cBuf[8];	// this should be ok for all built-in fonts

  PRINTS("\nScrolling text");
  mx.clear();

  while (*p != '\0')
  {
    charWidth = mx.getChar(*p++, sizeof(cBuf)/sizeof(cBuf[0]), cBuf);

    for (uint8_t i=0; i<charWidth + 1; i++)	// allow space between characters
    {
      mx.transform(MD_MAX72XX::TSL);
      if (i < charWidth)
        mx.setColumn(0, cBuf[i]);
      delay(DELAYTIME);
    }
  }
}

void setup() {
  mx.begin();
}

void loop() {
  scrollText("Hello world");
  scroleText("Amrith");
}
