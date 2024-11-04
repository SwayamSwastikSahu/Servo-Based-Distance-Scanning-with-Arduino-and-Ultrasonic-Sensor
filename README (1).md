
# Servo-Based Distance Scanning with Arduino and Ultrasonic Sensor

This Arduino project uses a servo motor and ultrasonic sensor to measure distances and scan the environment. The servo rotates the sensor, capturing distance data at various angles, which is then output to the Serial Monitor. This setup is ideal for proximity detection and basic environmental mapping applications.

## Components Required
- Arduino (e.g., Arduino Uno)
- Ultrasonic Sensor (HC-SR04)
- Servo Motor
- Jumper wires
- Breadboard

## Circuit Diagram
Connect the components as follows:
- **Trig Pin** of Ultrasonic Sensor to **Digital Pin 11** on Arduino
- **Echo Pin** of Ultrasonic Sensor to **Digital Pin 12** on Arduino
- **Servo Motor Signal Pin** to **Digital Pin 13** on Arduino
- Connect GND and VCC pins as needed

## Code
The code rotates the servo from 15° to 165° and back, capturing distance measurements at each position. The `calculateDistance()` function calculates the distance based on the ultrasonic sensor's feedback.

```cpp
#include <Servo.h>

const int trigPin = 11;
const int echoPin = 12;
long duration;
int distance;
Servo myServo;

void setup() {
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  Serial.begin(9600);
  myServo.attach(13);
}

void loop() {
  for (int i = 15; i <= 165; i++) {  
    myServo.write(i);
    delay(25);
    distance = calculateDistance();
    Serial.print(i);
    Serial.print(",");
    Serial.print(distance);
    Serial.print(".");
  }
  for (int i = 165; i > 15; i--) {  
    myServo.write(i);
    delay(25);
    distance = calculateDistance();
    Serial.print(i);
    Serial.print(",");
    Serial.print(distance);
    Serial.print(".");
  }
}

int calculateDistance() {
  digitalWrite(trigPin, LOW); 
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  duration = pulseIn(echoPin, HIGH);
  distance = duration * 0.034 / 2;
  return distance;
}
```

## How It Works
1. The servo motor rotates from 15° to 165° and then back.
2. At each angle, the ultrasonic sensor measures the distance to the nearest object.
3. Distance and angle data are sent to the Serial Monitor in a comma-separated format.

## Applications
- Obstacle detection
- Robotic navigation
- Environmental mapping

## License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
