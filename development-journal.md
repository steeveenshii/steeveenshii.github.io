# Arduino Timer Alarm — Innovator Journal

## Project idea

For my Unit 1 project, I made an Arduino timer alarm. While the button is pressed, the LED turns on and the Arduino starts timing. After five seconds, the piezo buzzer begins to beep. The buzzer keeps cycling while the button is held; when I release the button, the LED and buzzer turn off and the timer resets.

I chose this project because I wanted to get better at **analog output** and understand how outputs can work together in a real circuit. My final LED is on digital pin 13, so it is currently used as a digital output. In a future version, I would move it to a PWM pin and use analogWrite() to fade the LED; that would be my next step for developing analog output skills. This project also helped me learn more about timing with millis(), buzzer output, and reading a button.

## New component and research

The new component I chose was a **piezo buzzer**. It is an output device that makes a tone when the Arduino sends it a frequency. I researched how to use the tone() and noTone() functions in Arduino's Tone Melody example: <https://docs.arduino.cc/built-in-examples/digital/toneMelody/>. I also used Arduino's Fade example to learn how PWM output works with analogWrite(): <https://docs.arduino.cc/built-in-examples/basics/Fade/>.

## Materials

| Component | What it does |
| --- | --- |
| Arduino Uno | Runs the program and controls the circuit |
| Piezo buzzer | Makes the alarm sound |
| LED and 220 Ω resistor | Shows that the timer is running |
| Push button | Starts and resets the timer |
| Breadboard and jumper wires | Connect the circuit |

## Circuit plan

| Component | Arduino connection |
| --- | --- |
| Push button | Pin 2; pressing it reads LOW |
| Piezo buzzer (+) | Pin 8 |
| Piezo buzzer (−) | GND |
| LED anode through a 220 Ω resistor | Pin 13 |
| LED cathode | GND |

## Design process and testing

### 1. Planning

I planned the project as a sequence: read the button, turn on the light, count five seconds, then start the buzzer. I used millis() instead of one long delay so the Arduino could keep checking the button and printing useful messages to the Serial Monitor.

### 2. First prototype — what did not work

My first version did not work correctly because the button logic had the HIGH and LOW values flipped. I was checking for the wrong state, so the Arduino responded at the wrong time. I changed the code to use LOW as the pressed state and tested it again.

This also taught me why debouncing matters. A real button can produce more than one quick signal when it is pressed. My current program checks the button state directly, so adding debounce code would be an important future improvement if the button began triggering more than once.

### 3. Peer support

Matthew Ma helped me wire my alarm and helped my code work. He looked at my button code and found that the LOW and HIGH values were flipped. He explained which state my button should read when it was pressed. After that, I changed my button condition and tested it again. This helped me understand both the wiring and why I need to check how the input is set up instead of assuming every button works the same way.

### 4. Final test

I tested the final circuit in this order:

1. I held the button down; the LED turned on and the Serial Monitor printed LIGHT ON.
2. I waited five seconds; the buzzer started to beep every half second.
3. I checked the Serial Monitor to see the timer and the message Timer is done!
4. I released the button; the LED and buzzer stopped, and the system reset.
5. I repeated the test to make sure it worked again.

## Final Arduino code

~~~cpp
const int buttonPin = 2;
const int ledPin = 13;
const int buzzerPin = 8;

unsigned long lightStartTime = 0;
unsigned long lastPrintTime = 0;
unsigned long lastBeepTime = 0;
unsigned long buzzerStartTime = 0;

bool lightOn = false;
bool buzzerOn = false;

void setup() {
  pinMode(buttonPin, INPUT);
  pinMode(ledPin, OUTPUT);
  pinMode(buzzerPin, OUTPUT);

  Serial.begin(9600);
  Serial.println("SYSTEM READY");
}

void loop() {
  int buttonState = digitalRead(buttonPin);

  if (buttonState == LOW) {
    digitalWrite(ledPin, HIGH);

    if (!lightOn) {
      lightOn = true;
      lightStartTime = millis();
      lastPrintTime = millis();

      Serial.println("LIGHT ON");
    }

    unsigned long currentTime = millis();
    unsigned long lightTime = currentTime - lightStartTime;

    // Print timer every 0.5 seconds
    if (currentTime - lastPrintTime >= 500) {
      lastPrintTime = currentTime;

      Serial.print("LIGHT ON FOR ");
      Serial.print(lightTime / 1000.0, 1);
      Serial.println(" SECONDS");
    }

    // Start buzzer after 5 seconds
    if (lightTime >= 5000 && !buzzerOn) {
      buzzerOn = true;
      buzzerStartTime = currentTime;
      lastBeepTime = currentTime;

      Serial.println("BUZZER STARTED");
    }

    if (buzzerOn) {
      unsigned long buzzerTime = currentTime - buzzerStartTime;

      // Stop after 3 seconds
      if (buzzerTime >= 3000) {
        noTone(buzzerPin);
        buzzerOn = false;

        Serial.println("BUZZER STOPPED");
      }

      // Beep every 0.5 seconds
      else if (currentTime - lastBeepTime >= 500) {
        lastBeepTime = currentTime;
        tone(buzzerPin, 1000, 200);

        Serial.println("Timer is done!");
      }
    }
  } else {
    digitalWrite(ledPin, LOW);
    noTone(buzzerPin);

    if (lightOn) {
      Serial.println("LIGHT OFF");
    }

    lightOn = false;
    buzzerOn = false;
  }
}
~~~

## Evidence of the finished circuit

**Add a photo or short video here before submitting.** It should show the Arduino, button, LED, buzzer, and breadboard wiring while the alarm is active. A short video can show the button press, five-second wait, the light and buzzer turning on, and the button release resetting the circuit.

## Reflection

This kind of alarm could be useful for someone who needs a simple reminder, such as a student remembering to take a break or a person who needs a timed alert while doing a task. To make this project useful in a real situation, I would add a case, a battery, a way to choose the timer length, and a display that shows the countdown. I would also move the LED to a PWM pin and fade it as the alarm gets closer.

The skill I would rely on most is wiring and debugging. My first code did not work because the button's LOW and HIGH values were flipped. Finding and fixing that problem showed me that I need to understand how every wire and input setting affects the code. Next time, I would test each part separately before putting the whole alarm together.
