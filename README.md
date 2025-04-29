# Scrolling-characters-on-MAX7219-display-and-ESP8266

A program to show the scrolling text capabilities of the MAX7219 display using the MD_MAX72xx library in arduino.

The library (MD_MAX72xx) can be installed using the boards manager in the Arduino IDE or can be installed from the github link :
``` https://github.com/MajicDesigns/MD_MAX72XX ```

The version I am using is 2.0.10 which can be selected in the drop down menu in the arduino boards manager.

However there are some changes which need to be made to the header (.h) file of the library so that the code works properly, which i will talk about in the next section.

## Changes to the header file of the library

1. Find the place on your computer where all the arduino libraries are stored. This can be done by pressing CTRL + K in the arduino library which will take you to the folder where the sketch is present.
2. After the folder opens up, navigate back to the 'Arduino' folder where you will see all your programs.
3. From the Arduino folder, navigate to : Arduino -> libraries -> MD_MAX72xx -> src ->  MD_MAX72xx.h
4. Open the header file and find the following #define functions and change both from 0 to 1. (refer image below)
   
<img width="589" alt="image" src="https://github.com/user-attachments/assets/41b99cfe-64a0-469b-a560-f52e39c9464c" />

(change the 0 to a 1 in the '#define	USE_PAROLA_HW'	 function)

![image](https://github.com/user-attachments/assets/51253d36-0a98-4bcb-83d2-f69caa0eb5f6)

(same as above, change te function value from a 0 to a 1)

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
