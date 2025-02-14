![image](https://github.com/user-attachments/assets/b592ca95-89e9-470c-b302-c741f8b392d4)---
title: "2025-02-14 interim solution"
date: 2025-02-14
layout: post
---

happy valentines day. never stop gambling. <br />

![20250214123715_1](https://github.com/user-attachments/assets/fc48f025-650b-49b5-84be-8bfa77af86a8)

but it's not about balatro. or any other balala joker poker. it's about revision b of the grip, the bridge between rev-a and rev-c.  <br />

### the interim solution

![interim-solution-001-bold](https://github.com/user-attachments/assets/472b61da-6b4b-4112-b158-5b05753e62c5)

i'm on holiday this week, my work holiday. so i was working on the design, and i'd like to keep this brief.  <br />

the electronics and code are very similar to rev-a. but better. almost everything here is almost the same as before, but better. let's address the elephant in the room - i've worked on the with some help from chatgpt, but the final product is mine only. <br />

```c
#include <iostream>
#include <stdio.h>
#include <Wire.h>

int grip_address = 0x20; // Address for the I2C expander
int status;
const int buttons_qty = 14; // >>Number<< of buttons, lower than table size due to loop behaviour

void setup() {
  // I2C
  Wire.begin();
  Serial.begin(9600);
  Wire.beginTransmission(grip_address);
  Wire.write(0);
  Wire.write(0);
  Wire.endTransmission();

  // Joystick update state
  Joystick.useManualSend(true);
}

void loop() {
  int previous_button_status[16] = {0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0}; // Button status table
  Joystick.X(analogRead(8)); // Pitch axis
  Joystick.Y(analogRead(7)); // Roll axis
  Serial.println(analogRead(7));
  Serial.println(analogRead(8));
  //Joystick.Z(analogRead(9));

  Joystick.Zrotate(analogRead(11)); // Broken yaw axis
  Joystick.sliderLeft(analogRead(12)); // Throttle

  Wire.requestFrom(grip_address, 1); // Requisting one byte of information from the I2C expander
  while(Wire.available()){
    status = Wire.read(); // Transfering that byte into variable
  }
  
  // The >>funny<< part
  int current_button_state = 0; // Status of button that is currently, in the loop, being tested
  // Checking every button of the array
  for(int i = 0; i <= buttons_qty; i++){
    // If the button is pressed, the status is true
    // The byte can be interpreted as binary - 00001000, but also as a regular number - 8
    // By comparing the power of 2 values with status we can check if e. g. third button is pressed, by comparing it with status value
    if(status == pow(2, i)){
      current_button_state = 1;
      Serial.println(status);
    }
    // If not we can proceed forward, button is not pressed
    else if(status != pow(2, i)){
      current_button_state = 0;
    }

    // It would be possible to optimize this code by moving this loop inside the first loop
    // But here, if the current status of the button is different that the previous one
    // So the button changed it's position from pressed to released
    // Or the other way around
    // We can update the status of the button, and after that change it's status once again

    // So if the current status is released, but the previous was pressed
    // We update the button, and change it's status to the current, correct one
    if(current_button_state != previous_button_status[i])
    {
      Joystick.button(i + 1, 1);
      previous_button_status[i] = current_button_state;
      status = 0;
    }
  }

  Joystick.send_now(); // Updating the button state and clearing the status of whole set in the next loop
  for(int j = 0; j <= buttons_qty; j++){
    Joystick.button(j, 0);
  }
}
```
buttons are running on pcf8575, so we have access to sixteen of them but currently using only eight. you can go briefly through the code attached above, it's jury rigged and working, so i'm proud of it. layout is the same as before - weapon release, tracking, weapon cycle, guns and countermeasures. eight buttons in total.  <br />

biggest changes are in the mechanical design section. whereas before it was one monolithic print this time i decided to split it for easier assembly.

![interim-solution-007](https://github.com/user-attachments/assets/bc7f7459-ff8f-4652-9c2a-36e4f17500ec)

grip section is splitted along the longer edge - we get front facing and back facing plates. both with mounting holes for pcbs and space for cable routing. adapter attaches to 2020 aluminum extrusion (as before) or print with similar size.
