





# 3D Scanner Project

## Project overview

For this project, Matthew Ma, Blair Wen, and I built a scanner that can measure how far away an object is from different angles. We used a motor to rotate the scanner while the distance sensor checked the distance to objects. Moving the scanner lets it collect readings in more than one direction, which is an important part of making a 3D scan.

## How the motor helps the scanner

The stepper motor moves the scanner a small amount at a time. As it turns, the distance sensor can take a reading, then the motor can move again to scan the next direction. By repeating this process, the scanner can measure objects around it instead of only looking straight ahead.

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

<video controls width="100%"><source src="https://github.com/user-attachments/assets/4a0e6a04-1768-4b9a-98af-e8e8328486bb" type="video/mp4">Your browser does not support the video tag.</video>
