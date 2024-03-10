#define sensorPin A0  // Analog pin connected to LM35 sensor
#define ledPin 13     // Onboard LED pin

volatile unsigned long lastBlinkTime = 0;  // Stores the last blink timestamp
volatile int blinkInterval = 500;         // Blink interval in milliseconds (default 500ms)

void setup() {
  pinMode(ledPin, OUTPUT);  // Set LED pin as output
  // No additional setup needed for LM35 sensor
}

void loop() {
  // Read analog value from sensor (0-1023)
  int sensorReading = analogRead(sensorPin);

  // Convert analog reading to voltage (0-5V)
  float voltage = sensorReading * (5.0 / 1023.0);

  // Convert voltage to temperature in Celsius (assuming LM35 calibration)
  float temperature = voltage * 100;

  // Update blink interval based on temperature
  if (temperature < 30) {
    blinkInterval = 250;
  } else {
    blinkInterval = 500;
  }

  // Check for elapsed time since last blink
  unsigned long currentTime = micros();
  if (currentTime - lastBlinkTime >= blinkInterval * 1000) {
    digitalWrite(ledPin, !digitalRead(ledPin));  // Toggle LED state
    lastBlinkTime = currentTime;
  }
}
