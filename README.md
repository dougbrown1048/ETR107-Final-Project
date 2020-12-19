## Playing With Trucks
Here's where I'm documenting the project I'm working on for two electronics classes I'm taking at  
Piedmont Virginia Community College in Fall 2020.

### Introduction

**For ETR237**
For ETR237 I created a slot-car truck that regulated its own speed autonomously. The primary factor relating to safe operating speed for a slot car is the distance to the next corner. I used an ultrasonic distance sensor mounted on the truck to measure how far it is to the next curve on the track and to send that information to an Adafruit Feather motor control setup, also mounted on the car. I programmed an algorithm into the Feather that used the sensor distance data as an input to then control the output voltage accordingly.


**For ETR107**
With the same truck
  1. Use a BBC MicroBit microprocessor onboard the truck to collect and stream telemetry via radio link to another MicroBit.  Data collected includes  
    - Acceleration in two dimensions  
    - Maximum and average speed
  2. With the second MicroBit connected to a laptop, view the streamed data in realtime

**Here's a picture of the truck**

![The project truck!](https://github.com/dougbrown1048/ETR107-Final-Project/blob/main/Pictures/IMG_0542.JPG "Racing trucks, who knew?")

