## Playing With Trucks
Here's where I'm documenting the project I'm working on for two electronics classes I'm taking at  
Piedmont Virginia Community College in Fall 2020.

### Introduction

**For ETR237**
For ETR237 I created a slot-car truck that regulated its own speed autonomously. The primary factor relating to safe operating speed for a slot car is the distance to the next corner. I used an ultrasonic distance sensor mounted on the truck to measure how far it is to the next curve on the track and to send that information to an Adafruit Feather motor control setup, also mounted on the car. I programmed an algorithm into the Feather that used the sensor distance data as an input to then control the output voltage accordingly.  **Be aware** if you attempt to repeat this project, the Adafruit motor shield that I used is not robust enough for this project to work at full power.  Slot cars can readily draw more current than the motor shield can handle.  The result is a rather catastrophic failure of the motor driver.


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

![Picture 0530!](https://github.com/dougbrown1048/ETR107-Final-Project/blob/main/Pictures/IMG_0530.jpg) ![Picture 0536!](https://github.com/dougbrown1048/ETR107-Final-Project/blob/main/Pictures/IMG_0536.jpg)

Next I mounted the ultrasonic range sensors on the cab of the truck.  I did this by drilling small holes and using wire to secure the sensor.  I then attached the cab of the truck to the rest of the body and connected the sensor leads to the feather.  This completed the physical assembly of the truck for this portion of the project.

![Picture 0541!](https://github.com/dougbrown1048/ETR107-Final-Project/blob/main/Pictures/IMG_0541.jpg)

**Composing the Power Management Code**

Now that the truck was assembled, I needed to figure out the code that would make it do what I hoped it would.  I'd already shown that I could control motor speed with the Feather using an example program provided with the Arduino file editor.  I searched online and found some Arduino code that could translate signals from an ultrasonic sensor to distance in centimeters.  I ran this code on my Feather and found that it worked and the sensor could accurately measure distance.  Now I merely had to integrate those two applications into one program.

My first attempt was a simple linear relationship between distance and motor power.  That code can be found at my Github repo here:

https://github.com/dougbrown1048/ETR107-Final-Project/blob/main/project01.ino

The critical code can be found at lines 50 and 51.  I loaded this code and hooked the truck up to a 9V battery and it actually worked.  As I moved my hand in front of the sensor, the truck wheels would speed and slow in proportion to the distance.  However, this algorithm was still quite skewed towards larger distances and the speed was slow at distances that I wanted the truck to operate at.  So I worked on version two here:

https://github.com/dougbrown1048/ETR107-Final-Project/blob/main/project02.ino

Again, the important code here is found on lines 49-59.  This includes a set of "if" statements for far (full power) , near (zero power), and in between.  It still has a linear relationship between distance and power for the in between distance range.  This code worked when bench tested, so I took it to the track.  To be careful, I started with a constant 6 V DC power supply.  The truck worked.  The motor speed varied in response to distance.  The ultrasonic distance sensors even seemed to respond to the guard rails on the corners of the track, suggesting that I wouldn't need artificial means to mark the distance.  Unfortunately, when I increased the voltage, the motor driver failed suddenly and catastrophically.

I purchased a new motor driver, and again tested the truck at low voltage.  It works, but the top speed is rather limited and, as a consequence, acceleration is quite modest as well.  To help overcome this, I tweaked the software algorithm one more time.  I think the ideal solution would be some sort of logarithmic relationship.  But rather than experiment with that, I created more distance intervals with linear relationships with a steeper slope in order to simulate a non-linear, logarithmic relationship in a stepwise manner.  That final code can be found here:

https://github.com/dougbrown1048/ETR107-Final-Project/blob/main/project03.ino

That was all the power optimization that I attempted.  With that, the part of this project for ETR237 was essentially complete and I then moved on to the parts of the project for ETR107.

### The Process ETR107

As I noted above, in this part of the project my goal was to send acceleration data from a MicroBit mounted on the truck to a computer where it could be monitored in real time.  My first thought was to use the Bluetooth capability of the MicroBit and have it communicate directly with the laptop.  My instructor persuaded me that it might be difficult to send more than one datastream in this manner.  A better plan might be to use the radio communication features of the MicroBit to have the device on the truck send the data to a second MicroBit that was connected to a laptop via USB.  I regret that I don't have any good pictures of the final physical setup of the MicroBit on the truck, but here's a picture of a mockup, without the wiring hooked up.  At this point in the physical setup I also connected the MicroBit, via the breadboard, to the power output from the Feather.

![Picture 0543!](https://github.com/dougbrown1048/ETR107-Final-Project/blob/main/Pictures/IMG_0543.jpg)

A second goal of this portion of the project was to also transmit the distance data from the ultrasonic sensor that was collected on the Feather mounted on the truck.  The hope was that the distance data could be plotted along with the acceleration.  The first attempt at doing this involved using the serial monitor in the Mu editor that I'd used to program the Feather.  Unfortunately, while the numerical distance would print in the serial monitor, I was unable to get the data to plot in the Mu editor.  This lead to a change in tools and programming language.  I then switched to using Microsoft MakeCode for MicroBit.  The online editor can be found here:

https://makecode.microbit.org/

With this simple tool I was readily able to get the MicroBits to communicate and I could plot the acceleration data directly in the MakeCode editor.  Here's a screenshot of that.

![Screenshot 2!](https://github.com/dougbrown1048/ETR107-Final-Project/blob/main/Pictures/Screenshot%20(2).png)

Below is the code that I wrote that enabled the two microbits to transfer that information.  On the left is the code for the MicroBit that was mounted on the truck, what I referred to as the "sender".  On the right is the code for the MicroBit that was connected to the laptop, what I referred to as the "receiver". 

![Screenshot 7!](https://github.com/dougbrown1048/ETR107-Final-Project/blob/main/Pictures/Screenshot%20(7).png)  ![Screenshot 8!](https://github.com/dougbrown1048/ETR107-Final-Project/blob/main/Pictures/Screenshot%20(8).png)

This worked great and transmitted acceleration data in real time.  As I noted above, the secondary goal of transmitting distance data from the Feather wasn't entirely successful.  I was able to transmit a text string.  To do that I used the Rx and Tx pins on the Feather to make a serial connection with Tx and Rx pins that I specified on the "sender" MicroBit.  I only had to modify the Arduino code on the Feather slightly at lines 19 and 67, where "serial.begin" and "serial.print" were changed to "serial1.begin" and "serial1.print".  This re-directed the serial output from the USB to the Rx and Tx pins.  The modified code for the sender MicroBits are shown below.  Again the "sender" is on the right and the "receiver" is on the left.

![Screenshot 15!](https://github.com/dougbrown1048/ETR107-Final-Project/blob/main/Pictures/Screenshot%20(15).png)  ![Screenshot 15!](https://github.com/dougbrown1048/ETR107-Final-Project/blob/main/Pictures/Screenshot%20(15).png)

That's the best that I could do.  MakeCode, so far as I can tell, does not have the capability to program the MicroBit to receive data on the serial monitor, only text.  If I want to make this happen, I'll need to try to code the MicroBits in another language like Circuit Python.  But I'm content with my results and so I'll conclude here.



