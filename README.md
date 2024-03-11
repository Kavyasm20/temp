#include <ArduinoSTLWrapper.h>

const int lm35Pin = A0;
const int ledPin = 13;

STLIntervalTimer blinkTimer;

void setup() {
  pinMode(ledPin, OUTPUT);
  blinkTimer.setInterval(250);
}

void loop() {
  int temperature = analogRead(lm35Pin) * 0.48828125; // Convert analog reading to temperature in degrees Celsius

  if (temperature < 30) {
    blinkTimer.update();
    if (blinkTimer.checkInterval()) {
      digitalWrite(ledPin, !digitalRead(ledPin));
    }
  } else {
    blinkTimer.setInterval(500);
    blinkTimer.update();
    if (blinkTimer.checkInterval()) {
      digitalWrite(ledPin, !digitalRead(ledPin));
    }
  }
}
