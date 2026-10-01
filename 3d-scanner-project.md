# 3D Scanner Project

## Project overview

For this project, Matthew Ma, Blair Wen, and I built a scanner that can measure how far away an object is from different angles. We used a motor to rotate the scanner while the distance sensor checked the distance to objects. Moving the scanner lets it collect readings in more than one direction, which is an important part of making a 3D scan.

## How the motor helps the scanner

The stepper motor moves the scanner a small amount at a time. As it turns, the distance sensor can take a reading, then the motor can move again to scan the next direction. By repeating this process, the scanner can measure objects around it instead of only looking straight ahead.

## The Steps

At first, I considered using a servo motor. A normal servo is limited to moving from about -90° to +90°, so it cannot keep rotating all the way around the scanner. To make a full scan, we used a NEMA 17 stepper motor instead. The NEMA 17 can turn 360° in small, controlled steps, which lets the sensor measure distance from many directions.

I used AI to help me make and understand the first version of the motor code. Then I tested the code with the circuit and changed it until the motor moved correctly. The code sends a direction signal and repeated step signals so the NEMA 17 turns a small amount at a time.

For the breadboard wiring, we used the power and ground rails to organize the circuit. The Arduino and motor driver shared a common ground. The motor connected to a stepper-motor driver, rather than directly to the Arduino, and the driver connected to the NEMA 17. The Arduino's direction wire went to digital pin 2, and the step wire went to digital pin 3, matching the code. This wiring let the Arduino tell the motor driver when and which way to move the scanner.

## Teamwork

Matthew Ma, Blair Wen, and I worked together on the motor movement and the scanning system. We used the code below to control the motor so the scanner could move while measuring how far away an object was.

## Motor code

~~~arduino
const int DIR_PIN = 2;
const int STEP_PIN = 3;

void setup() {
  pinMode(DIR_PIN, OUTPUT);
  pinMode(STEP_PIN, OUTPUT);

  digitalWrite(DIR_PIN, HIGH);
}

void loop() {
  // Send steps to the motor
  digitalWrite(STEP_PIN, HIGH);
  delayMicroseconds(1500);

  digitalWrite(STEP_PIN, LOW);
  delayMicroseconds(1500);
}
~~~

## Demonstration

<video controls width="100%"><source src="https://steeveenshii.github.io/img-5423_xHzhEUZ8%20%281%29.mp4" type="video/mp4">Your browser does not support the video tag.</video>
