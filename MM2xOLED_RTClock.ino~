/*
 * Petey's Clock  v0.1 - simple clock on 20x4 character LCD with i2c backpack
 *                v0.2 - moved to newliquidcrystal library for the LCD
 *                       to help facilitate using larger, custom fonts
 *                v0.3 - Forked / Base clock code kept
 *                       Moved to 2 x SSD1306 128x64 OLED screens,
 *                       split between hours and mintues. 
 *                     - 24/12 hour converstion.
 *                v0.4 - TO DO:
 *                         - add am/pm text
 *                         - how to display seconds? Text or blinky light
 *                           a third screen? need new library MultiOLED.h
 *                         - Possible to smooth out text refresh
 *                         - clean up code, functions
 *                         - Update display1 (Hour) only when hour changes
 *                           to minize the blinking. 
 * https://www.wrightmac.net
MIT License

Copyright (c) 2019, Peter Hein

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.  
 */

#include <Wire.h> 
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>
#include <Fonts/FreeMonoBold12pt7b.h>
#include "RTClib.h"
// #define TIME_24_HOUR  true
// #define DS3231_ADDRESS 0x68
#define OLED_RESET  -1


// Declaration for an SSD1306 display connected to I2C (SDA, SCL pins)
Adafruit_SSD1306 display1(OLED_RESET);  // Create display1
Adafruit_SSD1306 display2(OLED_RESET);  // Create display2

RTC_DS3231 Clock;

int Hour = 0;
int Minute = 0;
int Second = 0;
//int Temp = 0;

void GetTemp(){
      int Temp = 0;
      // Get the temperature
      Temp = Clock.getTemperature();
      // Convert it over to degrees Farenheit
      Temp = ((Temp * 1.8) +32);
      //DEBUG
      Serial.println(Temp);  
}

void setup(){
  
  // Initialize display1 with the I2C address of 0x3C
  display1.begin(SSD1306_SWITCHCAPVCC, 0x3D);  
  // Initialize display2 with the I2C address of 0x3D
  display2.begin(SSD1306_SWITCHCAPVCC, 0x3C);

  Serial.begin(9600);
  Clock.begin();
    
  // Activate LCD module
  display1.clearDisplay();
  display2.clearDisplay();
  
  // Splash screen
  display1.setTextSize(2);
  display1.setTextColor(WHITE);
  display1.setCursor(0, 0);
  // Display static text
  display1.println("Howdy");
  display1.println("Petey Time!");
  display1.display(); 
  delay(2000);
  display1.clearDisplay(); 
  display1.display();
}

void loop()
{     

      // This line sets the RTC with an explicit date & time, for example to set
      // July 29, 2019 at 13:08:00 you would call:
      // rtc.adjust(DateTime(2019, 7, 29, 13, 8, 0));
        
      // Setup time
      DateTime now = Clock.now();
      Second = now.second(); 
      Minute = now.minute();
      Hour = now.hour(); 


      // Clear the displays
      display1.clearDisplay();
      display1.display();  
      display2.clearDisplay();
      display2.display();
      
      //DEBUG 
      Serial.println(Hour);
      if (now.hour() >= 13) 
      {
        Hour = now.hour() - 12;
        display2.setFont();
        display2.setTextSize(1);
        display2.setCursor(30,60);
        display2.print("pm");
        display2.display();
        //DEBUG
        Serial.println("PM");   
      if (Hour == 0) 
          {
          Hour = 12; 
          } 
      }    
      else
        {
        Hour = now.hour();
        }
         
      /*
      // The Header
      display1.setFont();
      display1.setTextSize(2);
      display1.setTextColor(WHITE);
      display1.setCursor(1,15);
      display1.print("Petey");
      display1.display();
      display2.setFont();
      display2.setTextSize(2);
      display2.setTextColor(WHITE);
      display2.setCursor(1,15);
      display2.print("Time");
      display2.display();
  */
      // The Time
      // DEC converts the byte variable to a decimal number
      //    when displayed. 
      display1.setFont(&FreeMonoBold12pt7b);
      display1.setTextSize(2);
      display1.setTextColor(WHITE);
      display1.setCursor(50, 30);   
      display1.print(Hour, DEC); 
      display1.display();
     
      // Minutes and seconds have padded zeros
      display2.setFont(&FreeMonoBold12pt7b);
      display2.setTextSize(2);
      display2.setTextColor(WHITE);
      display2.setCursor(10, 30);
      display2.print(":");
             if(Minute <10)
        {
          display2.print("0");
        }
        display2.display();
      display2.print(Minute, DEC);
      display2.display();
      
      /*
       * This section is for seconds.
       * Due to limited screen space (I need a third screen!) 
       * it is commented out for now; to be added later.
       * 
      display2.setFont(&FreeMonoBold12pt7b);
      display2.setTextSize(2);
      display2.setTextColor(WHITE);
      display2.setCursor(10, 30);
      display2.print(":");
        if (now.second()<10)
        {
          display2.print("0");
        }
        display2.print(now.second());
        display2.display();
      */
      
      delay(2000);
      }
