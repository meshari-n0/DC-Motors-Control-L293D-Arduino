# 🚗 DC Motors Control using L293D & Arduino

## 📌 Overview
This project controls two DC motors using the L293D motor driver with Arduino UNO.  
The system performs multiple movements (Forward, Backward, Left, Right), displays the current movement on an LCD screen, and prints the status in the Serial Monitor.

---

## ⚙️ Components
- Arduino UNO
- L293D Motor Driver
- 2 × DC Motors
- LCD 16x2 (I2C - PCF8574)
- Breadboard
- Jumper wires
- 9V Battery (optional)

---

## 🔌 Connections

### 🔹 Motor Control
| L293D Pin | Arduino |
|----------|--------|
| IN1 (Pin 2) | 8 |
| IN2 (Pin 7) | 9 |
| IN3 (Pin 10) | 10 |
| IN4 (Pin 15) | 11 |

---

### 🔹 Enable Pins
| L293D Pin | Connection |
|----------|-----------|
| Pin 1 (Enable 1,2) | 5V |
| Pin 9 (Enable 3,4) | 5V |

---

### 🔹 Power
- Pin 16 → 5V  
- Pin 8 → 9V battery 
- GND → Arduino GND  

---

### 🔹 LCD (I2C)
| LCD | Arduino |
|-----|--------|
| VCC | 5V |
| GND | GND |
| SDA | A4 |
| SCL | A5 |

---

## 🚀 Features
- Move Forward (30 seconds)
- Move Backward (60 seconds)
- Turn Left & Right (alternating)
- LCD displays movement
- Serial Monitor shows movement

---

## 🖥️ Serial Monitor Output
- Moving Forward
- Moving Backward
- Turning Left
- Turning Right
- Stopped

---

## 📷 Project Preview
<img width="811" height="499" alt="image" src="https://github.com/user-attachments/assets/51cbde6d-c38d-465d-8335-591886383116" />

---

## 💻 Arduino Code

```cpp
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd(0x20, 16, 2);

// Motor pins
int IN1 = 8;
int IN2 = 9;
int IN3 = 10;
int IN4 = 11;

void setup() {
  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);

  Serial.begin(9600);

  lcd.init();
  lcd.backlight();
}

// ===== Functions =====

void forward() {
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Forward");

  Serial.println("Moving Forward");

  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
}

void backward() {
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Backward");

  Serial.println("Moving Backward");

  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
}

void left() {
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Left");

  Serial.println("Turning Left");

  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
}

void right() {
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Right");

  Serial.println("Turning Right");

  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, LOW);
}

void stopMotors() {
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Stop");

  Serial.println("Stopped");

  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, LOW);
}

// ===== Main Loop =====

void loop() {

  // 1) Forward 30 sec
  forward();
  delay(30000);

  // 2) Backward 60 sec
  backward();
  delay(60000);

  // 3) Left & Right alternating (60 sec total)
  for (int i = 0; i < 6; i++) {

    left();
    delay(5000);

    right();
    delay(5000);
  }

  stopMotors();
  delay(5000);
}
