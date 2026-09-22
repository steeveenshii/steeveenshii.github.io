# Arduino Snooze Alarm — Innovator Journal

## Project idea

For my Unit 1 project, I made a small snooze-style Arduino alarm. When I press the button, the Arduino starts a five-second countdown. After five seconds, the LED turns on and the piezo buzzer makes a sound continuously. The alarm does not stop until I press the button again. Pressing the button resets the alarm, like a simple snooze alarm.

I chose this project because I wanted to get better at **analog output**. On an Arduino Uno, a PWM pin can create an analog-like output by turning on and off very quickly. I used PWM to control the brightness of the LED with analogWrite(). I also wanted to extend what I learned about button input, debouncing, and digital output into a device that behaves like a real alarm.

## New component and research

The new component I chose was a **piezo buzzer**. It is an output device that makes a tone when the Arduino sends it a frequency. I researched how to use the tone() and noTone() functions in Arduino's Tone Melody example: <https://docs.arduino.cc/built-in-examples/digital/toneMelody/>. I also used Arduino's Fade example to learn how PWM output works with analogWrite(): <https://docs.arduino.cc/built-in-examples/basics/Fade/>.

## Materials

| Component | What it does |
| --- | --- |
| Arduino Uno | Runs the program and controls the circuit |
| Piezo buzzer | Makes the alarm sound |
| LED and 220 Ω resistor | Shows that the alarm is active |
| Push button | Starts, silences, and resets the alarm |
| Breadboard and jumper wires | Connect the circuit |

## Circuit plan

| Component | Arduino connection |
| --- | --- |
| Push button | Pin 2 and GND, using INPUT_PULLUP |
| Piezo buzzer (+) | Pin 8 |
| Piezo buzzer (−) | GND |
| LED anode through a 220 Ω resistor | PWM pin 9 |
| LED cathode | GND |

The button uses INPUT_PULLUP. This means that its normal reading is HIGH, but it reads LOW when I press it because the button connects the pin to GND. I needed to remember this when I wrote and debugged the code.

## Design process and testing

### 1. Planning

I began by breaking the idea into three parts: read a button, wait five seconds, and trigger outputs. I decided that the same button would do two jobs. The first press starts the countdown. The second press stops the buzzer and resets the alarm so that it is ready to start again.

I used a timer instead of delay(5000) for the main countdown. A timer using millis() lets the Arduino keep checking the button while it waits, which makes the alarm feel more responsive.

### 2. First prototype — what did not work

My first version of the alarm did not work correctly. The code was checking the button with the HIGH and LOW values flipped. Because I used INPUT_PULLUP, I expected the button to read HIGH when I pressed it, but it actually reads LOW when pressed. This meant that the Arduino was responding at the wrong time.

I fixed the code by looking for LOW as the pressed state. I also used a short debounce time so that one press does not accidentally register as several presses.

### 3. Peer support

Matthew Ma helped me wire my alarm and helped my code work. He looked at my button code and found that the LOW and HIGH values were flipped. He explained that with INPUT_PULLUP, the Arduino reads HIGH when the button is not pressed and LOW when it is pressed. After that, I changed my button condition and tested it again. This helped me understand both the wiring and why I need to check which kind of input setup I am using instead of assuming every button works the same way.

### 4. Final test

I tested the final circuit in this order:

1. I pressed the button once; the timer started.
2. I waited five seconds; the LED turned on at full brightness and the buzzer started.
3. I checked that the buzzer kept sounding instead of stopping by itself.
4. I pressed the button again; the LED and buzzer stopped, and the system returned to its ready state.
5. I repeated the test to make sure it reset correctly.

## Final Arduino code

~~~cpp
const int buttonPin = 2;
const int buzzerPin = 8;
const int ledPin = 9;       // PWM pin for analog-style LED output

const unsigned long waitTime = 5000;
const unsigned long debounceTime = 50;

bool waitingForAlarm = false;
bool alarmOn = false;
bool lastButtonReading = HIGH;
bool stableButtonState = HIGH;
unsigned long timerStart = 0;
unsigned long lastDebounceTime = 0;

void setup() {
  pinMode(buttonPin, INPUT_PULLUP);
  pinMode(buzzerPin, OUTPUT);
  pinMode(ledPin, OUTPUT);
}

void loop() {
  bool reading = digitalRead(buttonPin);

  // Debounce the button so that one press counts once.
  if (reading != lastButtonReading) {
    lastDebounceTime = millis();
  }

  if (millis() - lastDebounceTime > debounceTime) {
    if (reading != stableButtonState) {
      stableButtonState = reading;

      // INPUT_PULLUP means LOW is the pressed state.
      if (stableButtonState == LOW) {
        if (alarmOn) {
          // Snooze/reset: silence the alarm.
          alarmOn = false;
          noTone(buzzerPin);
          analogWrite(ledPin, 0);
        } else if (!waitingForAlarm) {
          // First press starts the five-second countdown.
          waitingForAlarm = true;
          timerStart = millis();
        }
      }
    }
  }
  lastButtonReading = reading;

  // Turn on the alarm five seconds after the first button press.
  if (waitingForAlarm && millis() - timerStart >= waitTime) {
    waitingForAlarm = false;
    alarmOn = true;
    analogWrite(ledPin, 255); // brightest PWM value
    tone(buzzerPin, 1000);    // continuous 1000 Hz tone
  }
}
~~~

## Evidence of the finished circuit

**Add a photo or short video here before submitting.** It should show the Arduino, button, LED, buzzer, and breadboard wiring while the alarm is active. A short video can show the button press, five-second wait, the light and buzzer turning on, and the second press silencing it.

## Reflection

This kind of alarm could be useful for someone who needs a simple reminder, such as a student remembering to take a break or a person who needs a timed alert while doing a task. To make this project useful in a real situation, I would add a case, a battery, a better way to set the time, and maybe a display that shows the countdown. I could also make the LED slowly get brighter with more PWM values instead of turning straight to full brightness.

The skill I would rely on most is wiring and debugging. My first code did not work because the button's LOW and HIGH values were flipped. Finding and fixing that problem showed me that I need to understand how every wire and input setting affects the code. Next time, I would test each part separately before putting the whole alarm together.
