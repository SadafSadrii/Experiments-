The Project started with me attempting to articulate western gaze. The initial idea was to make a garment that protects who is wearing it from the pressure of the gaze of who is looking at it. Hence, I decided to use mirrors on my garment which are disguised and only reveal themselves if you look at them. 
I chose mirror as my main material because returning the gaze was my ulmitae goal. Also, I chose Burka as my garment because I assumed that it is among the most misunderstood garments in the west, especifically in the US.

Eventually I ended up using OpenCV to track the gaze and used Arduino to control a servo motor, to which, the scales (my inspiration was scales on the animals and I wanted the mirrors too look like scales adn act as camouflage) are attached.

Here is the code I used for my Processing and OpenCV:
_______________________________________________________________________________________________
/* 
 Actuate a servo motor in Arduino in Firmata 
 by using face detection in OpenCV
 

import gab.opencv.*; // import openCV library
import processing.video.*; // import video library
import java.awt.*; 


import processing.serial.*; // import the serial library
import cc.arduino.*; // import the arduino (firmata) library

Arduino arduino; // create an arduino object

Capture video; 
OpenCV opencv;

int servo = 3; // servo connected to digital pin 3
int servoAngle; // variable to store the servo's angle
int servoPos;

void setup() {
  size(640, 480);
  video = new Capture(this, 640/2, 480/2);
  opencv = new OpenCV(this, 640/2, 480/2);
  opencv.loadCascade(OpenCV.CASCADE_EYE);  

  video.start();

  // Prints out the available serial ports.
  println(Arduino.list());
  // Modify this line, by changing the "0" to the index of the serial
  // port corresponding to your Arduino board (as it appears in the list
  // printed by the line above).
  arduino = new Arduino(this, Arduino.list()[3], 57600);
  arduino.pinMode(servo, Arduino.SERVO); // declare servo connected to digital pin 4 as a servo object
} 

void draw() {
  scale(2);
  opencv.loadImage(video);
  image(video, 0, 0 );

  noFill();
  stroke(0, 255, 0);
  strokeWeight(1);

  Rectangle[] faces = opencv.detect();
  println(faces.length); // print how many faces are detected

  for (int i = 0; i < faces.length; i++) {
    println(faces[i].x + "," + faces[i].y); // print the x,y coordinates of the faces
    ellipseMode(CORNER); // draw a circle from the corner
    ellipse(faces[i].x, faces[i].y, faces[i].width, faces[i].height); // draw a circle over the face's coordinates
    //servoAngle = constrain(faces[i].x, 0, 180); // use the x coordinate of the detected face to control the angle of the servo
    //arduino.servoWrite(servo, servoAngle); // move the servo
  }
  
  if (faces.length > 1) {
    arduino.servoWrite(servo, 180); // tell servo to go to position 180
      delay(30);                       // waits 15ms for the servo to reach the position
    }
  else {
    arduino.servoWrite(servo, 0); // tell servo to go to position 0
      delay(30);                       // waits 15ms for the servo to reach the position
    }
  }


void captureEvent(Capture c) {
  c.read();
}
________________________________________________________________________________________

Here is my Arduino code:
________________________________________________________________________________________
/* Sweep
 by BARRAGAN <http://barraganstudio.com>
 This example code is in the public domain.

 modified 8 Nov 2013
 by Scott Fitzgerald
 http://www.arduino.cc/en/Tutorial/Sweep
*/

#include <Servo.h>

Servo myservo;  // create servo object to control a servo
// twelve servo objects can be created on most boards

int pos = 0;    // variable to store the servo position

void setup() {
  myservo.attach(9);  // attaches the servo on pin 9 to the servo object
}

void loop() {
  for (pos = 0; pos <= 180; pos += 1) { // goes from 0 degrees to 180 degrees
    // in steps of 1 degree
    myservo.write(pos);              // tell servo to go to position in variable 'pos'
    delay(15);                       // waits 15ms for the servo to reach the position
  }
  for (pos = 180; pos >= 0; pos -= 1) { // goes from 180 degrees to 0 degrees
    myservo.write(pos);              // tell servo to go to position in variable 'pos'
    delay(15);                       // waits 15ms for the servo to reach the position
  }
}
_______________________________________________________________________________________
And this video shows what I have achieved so far :https://vimeo.com/558405258

Also you can see the steps I took and check out more pictures of the project in this slide:
https://docs.google.com/presentation/d/1WHLKbFarzVzMUj_7hzvALJ1sIXm0U7A3WDk8gHePg5c/edit?usp=sharing

