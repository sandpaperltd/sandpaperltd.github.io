---
title: "2025-01-06 from concept to prototype in five days (sort of)"
date: 2025-01-06
layout: post
---

i had an idea for this blogpost for since middle of the december 2024. <br />

this was supposed to be a development blog after all, so where's all the development? so far we've done some personal stuff, some history of this and some psychological conversations about project approaches. but today i present grip-rev-a (i need to come up with a different name). <br />

![shtifuck-5-days-to-prototype-407](https://github.com/user-attachments/assets/e7d3e2f6-b7fc-411b-9137-1808ebe5a301)

(it looks soooooo goood). <br />

in the simplest terms it's a 3d printed flightstick grip (stick? joystick? a lot of different ways to call it, the manipulator?) attached to gimbal base of shtfck hotas/hosas. <br />

![5-days-to-prototype-407](https://github.com/user-attachments/assets/d82339e1-7c31-45b2-ac3d-fd56f50b01e7)
![5-days-to-prototype-008](https://github.com/user-attachments/assets/b3882e6e-0ebf-41ed-b465-edf305592e8c)

inspired (that's a bit of a stretch, but hear me out, even if look nothing like it) by f-16 grip has space for 3 universal pcbs (check the blogpost from 30th of december 2024, now it's all coming together) and can be mounted to the base with 2020 aluminum extrusion. insipiration comes from angle of the stick, and placement of buttons on the boards. f-16 grip has front panel with buttons for trimming the aircraft, weapon release. near the base it has additional paddle for autopilot, and of course there's a trigger for shooting guns. <br />

before we go the button functions on this grip let's address the 5 days part. i started sketching this on the december 9th and as you can see there's a note with five days later date indicating the idea (you have to believe me here, listen, i could post someone elses work and you would have no option to check it, so here, again, you need to believe me, i did this prototype in 5 days, more or less). <br />
![2024-12-09](https://github.com/user-attachments/assets/ebb294be-2edd-4d51-937d-41b7aeafeb5d)
![mainsail-log](https://github.com/user-attachments/assets/ffb832e8-972f-437a-87ab-c40e5527ca92)

and 5 days later i had first prints, with boards ready on december 15th. six days then, but five days for prints. so let's stick with that. <br />

it really looks like a development blogpost now, i'm so proud. <br />

anyway, back to the build.  <br />

for the print part was separated in the two planes for testing and checking the tolerances, after that printed and assembled. you can check the dates on the screenshot above (fun fact: both part design, electronic assembly and big part of code were done with mission impossible marathon running in the background). simple petg print and few hours later part was ready. i don't have good background for photos, and i don't want to share them yet as they look messier than on pictures, so once again, you have to believe me. <br />

electronics and code!  <br />
(i didn't hate it that much to be honest). <br />

each board has a plethora (that means a lot) of buttons - six on the front plane, two or three on the back plane (the two pin button stopped working, so i'm not counting it and not including it on the drawings), and additional one on the bottom plate. they're all connected to the pcf8574 i2c expander (one of them, i have three in my inventory and two seem not to work, but i'm not throwing them away yet, maybe they need some additional encouragement). <br />

front plate is responsible for weapon tracking, weapon release and weapon cycle (two buttons on the left hand side, and four on the right hand side). <br />

![5-days-to-prototype-303](https://github.com/user-attachments/assets/6cc5501a-437e-4b60-9aae-76207a7ec12b)

back plate(s) are responsible for shooting guns (one of the two buttons) and countermeasures release (pinky switch near left hand side, bottom). <br />

![5-days-to-prototype-005](https://github.com/user-attachments/assets/6d557923-37f7-44c9-afa4-1614d4c05c80)
![5-days-to-prototype-105](https://github.com/user-attachments/assets/89e122ac-37ab-4322-a374-6c4d8732583c)

as for the code it's primitive but i'll share it nevertheless, for the rev-c (where is rev-b you ask? currently - in progress, acting as a benchmark for most-likely-hopefully-final revision c of the project) layout maybe will stay the same but there are some updates to the code, currently tested with help from chatgpt (don't even @ me here, i'm so on the fence here i don't think how to approach, but it helped a lot in this case. worked like a copilot? as an copilot? i'm not really sure). <br />

```c
// Requires Arduino Joystick Library https://github.com/MHeironimus/ArduinoJoystickLibrary
#include <Joystick.h>
#include <Adafruit_PCF8574.h>
#include <Wire.h>

Adafruit_PCF8574 expander_01;
//Adafruit_PCF8574 expander_02;

Joystick_ Joystick;

int currentButtonState0;
int lastButtonState0;
int currentButtonState1;
int lastButtonState1;
int currentButtonState2;
int lastButtonState2;
int currentButtonState3;
int lastButtonState3;
int currentButtonState4;
int lastButtonState4;
int currentButtonState5;
int lastButtonState5;
int currentButtonState6;
int lastButtonState6;
int currentButtonState7;
int lastButtonState7;

int currentButtonState10;
int lastButtonState10;
int currentButtonState11;
int lastButtonState11;
int currentButtonState12;
int lastButtonState12;

int currentButtonState21;
int lastButtonState21;

const int buttonPin2 = 0;
const int buttonPin5 = 5;

int JoystickX;
int JoystickY;
int JoystickZ;
int Throttle;

void setup() {
  Serial.begin(115200);
  expander_01.begin(0x21);
  //expander_02.begin(0x27);
  expander_01.pinMode(0, INPUT_PULLUP);
  expander_01.pinMode(1, INPUT_PULLUP);
  expander_01.pinMode(2, INPUT_PULLUP);
  expander_01.pinMode(3, INPUT_PULLUP);
  expander_01.pinMode(4, INPUT_PULLUP);
  expander_01.pinMode(5, INPUT_PULLUP);
  expander_01.pinMode(6, INPUT_PULLUP);
  expander_01.pinMode(7, INPUT_PULLUP);

  pinMode(buttonPin2, INPUT);
  pinMode(buttonPin5, INPUT);

  pinMode(A0, INPUT);
  pinMode(A1, INPUT);
  pinMode(A2, INPUT);
  pinMode(A3, INPUT);
  pinMode(A4, INPUT);
  pinMode(A5, INPUT);
  pinMode(A6, INPUT);
  pinMode(A7, INPUT);
  pinMode(A8, INPUT);
  pinMode(A9, INPUT);
  

// Initialize Joystick Library
  Joystick.begin();
  Joystick.setXAxisRange(0, 1024); 
  Joystick.setYAxisRange(0, 1024);
  Joystick.setZAxisRange(0, 1024);
  Joystick.setThrottleRange(0, 1024);
}

void loop() {

// Read Joystick
  JoystickX = analogRead(A1); // Hall effect sensor connects to this analog pin
  JoystickY = analogRead(A0); // Hall effect sensor connects to this analog pin
  JoystickZ = analogRead(A8);
  Throttle  = analogRead(A9); // Hall effect sensor connects to this analog pin

  int currentButtonState0 = expander_01.digitalRead(0); // Button 0
    if (currentButtonState0 != lastButtonState0)
    {
    Joystick.setButton(0, currentButtonState0);
    lastButtonState0 = currentButtonState0;
  }

  int currentButtonState1 = expander_01.digitalRead(1); // Button 1
    if (currentButtonState1 != lastButtonState1)
    {
    Joystick.setButton(1, currentButtonState1);
    lastButtonState1 = currentButtonState1;
  }

  int currentButtonState2 = expander_01.digitalRead(2); // Button 2
    if (currentButtonState2 != lastButtonState2)
    {
    Joystick.setButton(2, currentButtonState2);
    lastButtonState2 = currentButtonState2;
  }

  int currentButtonState3 = expander_01.digitalRead(3); // Button 3
    if (currentButtonState3 != lastButtonState3)
    {
    Joystick.setButton(3, currentButtonState3);
    lastButtonState3 = currentButtonState3;
  }

  int currentButtonState4 = expander_01.digitalRead(4); // Button 4
    if (currentButtonState4 != lastButtonState4)
    {
    Joystick.setButton(4, currentButtonState4);
    lastButtonState4 = currentButtonState4;
  }

  int currentButtonState5 = expander_01.digitalRead(5); // Button 5
    if (currentButtonState5 != lastButtonState5)
    {
    Joystick.setButton(5, currentButtonState5);
    lastButtonState5 = currentButtonState5;
  }

  int currentButtonState6 = expander_01.digitalRead(6); // Button 6
    if (currentButtonState6 != lastButtonState6)
    {
    Joystick.setButton(6, currentButtonState6);
    lastButtonState6 = currentButtonState6;
  }
  /*
  int currentButtonState7 = expander_01.digitalRead(7); // Button 7
    if (currentButtonState7 != lastButtonState7)
    {
    Joystick.setButton(7, currentButtonState7);
    lastButtonState7 = currentButtonState7;
  }
  */
  int currentButtonState11 = digitalRead(buttonPin5); // Button 11
    if (currentButtonState11 != lastButtonState11)
    {
    Joystick.setButton(11, currentButtonState11);
    lastButtonState11 = currentButtonState11;
  }
  /*
  int currentButtonState21 = digitalRead(buttonPin2); // Button 21
    if (currentButtonState21 != lastButtonState21)
    {
    Joystick.setButton(21, currentButtonState21);
    lastButtonState21 = currentButtonState21;
  }
  */
  Joystick.setXAxis(JoystickX);
  Joystick.setYAxis(JoystickY);
  Joystick.setZAxis(JoystickZ);
  Joystick.setThrottle(Throttle);
  Joystick.sendState();

}
```
the code goes through button states and updates them accordingly in the least optimal fashion (is this sentence gramatically correct? frankly, i don't know nor i care). but it works. <br />

rev-a done, rev-b in progress, rev-c on the horizon (in the backlog).  <br />

enjoy some additional cad screenshots.  <br />

![shtifuck-5-days-to-prototype-407](https://github.com/user-attachments/assets/80551ea0-4c73-462d-b6ee-df942ac3720d)
![shtifuck-5-days-to-prototype-406](https://github.com/user-attachments/assets/08f47cd7-b80d-4776-bdc4-1d349e801cc8)
![shtifuck-5-days-to-prototype-405](https://github.com/user-attachments/assets/faa73baa-215d-426d-b0b4-7f3921ba707e)
![shtifuck-5-days-to-prototype-404](https://github.com/user-attachments/assets/a19c70ea-9ec3-4c2b-89b6-088edcab344b)
![shtifuck-5-days-to-prototype-403](https://github.com/user-attachments/assets/213f4485-a811-4193-a89d-cbe0ded7d369)
![shtifuck-5-days-to-prototype-402](https://github.com/user-attachments/assets/62cc728e-9425-473b-802a-017ae476cf2a)
![shtifuck-5-days-to-prototype-401](https://github.com/user-attachments/assets/f25958a8-f19b-44b2-afee-ca0dafbe0041)

![5-days-to-prototype-004](https://github.com/user-attachments/assets/0ca84280-daf8-4cf3-8987-672069f6806d)
![5-days-to-prototype-104](https://github.com/user-attachments/assets/13fa7672-3cd0-4704-a336-a3fcf006826c)
![5-days-to-prototype-204](https://github.com/user-attachments/assets/4afe344e-8835-4e10-b1be-277655d941eb)
![5-days-to-prototype-304](https://github.com/user-attachments/assets/f374d762-8fcd-48d0-882c-453c7b38e58f)
![5-days-to-prototype-404](https://github.com/user-attachments/assets/0007ea38-e9a4-491f-9f56-19abeb0bf246)

![5-days-to-prototype-002](https://github.com/user-attachments/assets/05de6910-97f7-419d-8090-6dc67b0d9bfe)
![5-days-to-prototype-102](https://github.com/user-attachments/assets/07144b7c-06d1-4289-a38a-c79d30e2e2b3)
![5-days-to-prototype-202](https://github.com/user-attachments/assets/f6f18270-5b13-4edd-a5a6-965053f0b0b2)
![5-days-to-prototype-302](https://github.com/user-attachments/assets/87df2830-c3f8-47e6-92e7-ac87243e180b)
![5-days-to-prototype-402](https://github.com/user-attachments/assets/2ff36abd-483c-4ded-a648-6c1bbd10e872)







