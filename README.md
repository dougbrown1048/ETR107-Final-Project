## Playing With Trucks
Here's where I'm documenting the project I'm working on for two electronics classes I'm taking at  
Piedmont Virginia Community College in Fall 2020.

### Introduction

**For ETR237**
For ETR237 I created a slot-car truck that regulated its own speed autonomously. The primary factor relating to safe operating speed for a slot car is the distance to the next corner. I used an ultrasonic distance sensor mounted on the truck to measure how far it is to the next curve on the track and to send that information to an Adafruit Feather motor control setup, also mounted on the car. I programmed an algorithm into the Feather that used the sensor distance data as an input to then control the output voltage accordingly.  Be aware if you attempt to repeat this project that the Adafruit motor shield that I used is not robust enough for this project to work at full power.  Slot cars can readily draw more current than the motor shield can handle.  The result is a rather catastrophic failure of the motor driver.


**For ETR107**
For ETR107, I used a BBC MicroBit mounted on the truck as a telemetry data logger.  The microbit contains integrated accelerometers in three dimensions.  I linked the mounted MicroBit via radio with another microbit that was connected to my laptop.  I then programmed the mounted MicroBit to collect acceleration data in two dimensions and send that data via the radio link to the other microbit.  I programmed the second microbit to receive the data and stream it via serial interface to my laptop where it could be monitored in real time. 

### The Process ETR237

**Feasibility Test**

First thing I did was proof of concept by using the DCMotorTest program provided with the Arduino file editor.  I connected the Feather with the motor control unit to the truck motor.  With a 9v power supply and no load on the motor, the motor control worked and it appears that it didn't exceed the current capacity of the motor driver.  Here's a picture of that test.

![Picture 03!](https://github.com/dougbrown1048/ETR107-Final-Project/blob/main/Pictures/Truck%2003.jpg)

**Construction of the Truck**

Next came the fun part, the physical configuration of all the parts.  First, in order to reduce mass and save space, I cut a breadboard to the minimum size needed to mount the Feather.  I then mounted the Feather and wired the breadboard to the body of the truck.

![Picture 0527!](https://github.com/dougbrown1048/ETR107-Final-Project/blob/main/Pictures/IMG_0527.jpg)

Then I spliced into the power supply wires for the slot-car in order to route power through the motor control unit.

![Picture 0530!](https://github.com/dougbrown1048/ETR107-Final-Project/blob/main/Pictures/IMG_0530.jpg)  ![Picture 0536!](https://github.com/dougbrown1048/ETR107-Final-Project/blob/main/Pictures/IMG_0536.jpg)

Next I mounted the ultrasonic range sensors on the cab of the truck.  I did this by drilling small holes and using wire to secure the sensor.  I then attached the cab of the truck to the rest of the body and connected the sensor leads to the feather.  This completed the physical assembly of the truck for this portion of the project.

![Picture 0541!](https://github.com/dougbrown1048/ETR107-Final-Project/blob/main/Pictures/IMG_0541.jpg)


**Here's a picture of the truck**

![The project truck!](https://github.com/dougbrown1048/ETR107-Final-Project/blob/main/Pictures/IMG_0542.JPG "Racing trucks, who knew?")

