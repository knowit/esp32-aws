 
## Steg 4: Potmeter

I steg 1 skal vi koble til et potmeter.


### Firmware

Legg inn følgene i ```main.cpp```.

```cpp
#include <Arduino.h>
#include <Wire.h>
#include "SSD1306Wire.h"

SSD1306Wire display(0x3c, SDA, SCL);

void setup()
{
  Serial.begin(115200);
  Serial.println("init");
  display.init();
  pinMode(15, INPUT);
  pinMode(18, OUTPUT);
  pinMode(35, INPUT);
  Serial.println("init..done");
}

void loop()
{
  Serial.print("button: ");
  Serial.println(digitalRead(35));
  digitalWrite(18, digitalRead(35));

  display.clear();

  display.setTextAlignment(TEXT_ALIGN_LEFT);
  display.setFont(ArialMT_Plain_24);
  if (digitalRead(35))
  {
    display.drawString(0, 0, "on");
  }
  else
  {
    display.drawString(0, 0, "off");
  }
  display.drawString(0, 24, String(analogRead(15)));
  display.display();

  delay(500);
}
}
```



### Hardware

Koble opp som vist:

![](./step4_bb.jpeg)

![](./step4_cam_3.jpg)
![](./step4_cam_1.jpg)
![](./step4_cam_2.jpg)

Last opp programmet.

Nå du vrir på potmeter, vises det en verdi mellom 0 og 4095 på skjermen
