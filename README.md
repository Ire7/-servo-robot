# Project: 4 Servo Motors Control with Arduino + External Power (Tinkercad)
## Objective
The goal of this project is to control **four servo motors** using an **Arduino UNO**. Each servo should:
Run a **sweep motion** (from 0° to 180° and back) for **2 seconds** one by one.
After all four motors finish their sweep, all motors **hold position at 90°**.

Since Arduino’s built-in 5V output cannot supply enough current for multiple servo motors at once, an **external 9V battery** is used to power the motors through the breadboard.

## Components Used
1x Arduino UNO  
4x Servo Motor (SG90)  
1x Breadboard  
1x 9V Battery  
1x 9V Battery Clip (with male headers or wires)  
Jumper Wires (Male-Male)  

## Power Setup (Step-by-Step)

1. I used a **9V battery** to supply enough current to power the four servo motors.
2. The **positive wire** of the 9V battery is connected to the **positive (red) rail** of the breadboard.
3. The **negative wire** of the 9V battery is connected to the **negative (blue) rail** of the breadboard.
4. A **GND wire** is connected from the **Arduino GND (Power section)** to the **blue (-) rail** on the breadboard.
    **This is essential** to have a **common ground** between the Arduino and the external power supply. Without this, the signal from the Arduino to the servos will not work correctly.

## ⚙Servo Motor Wiring

Each servo motor has 3 wires:
**Brown or Black** → GND (Ground)  
**Red** → VCC (5V)  
**Orange or Yellow** → Signal (PWM)


I connected the servos as follows:
| Servo | Signal Wire | Connected to Arduino Pin |
|-------|-------------|---------------------------|
| S1    | Orange      | D4                        |
| S2    | Orange      | D5                        |
| S3    | Orange      | D6                        |
| S4    | Orange      | D7                        |

All **Red wires (VCC)** from the servos are connected to the **+ rail** on the breadboard.  
All **Brown/Black wires (GND)** from the servos are connected to the **– rail** on the breadboard.

## Summary of Connections

### Servo Power:
9V Battery → Breadboard + rail
9V Battery GND → Breadboard – rail
Arduino GND → Breadboard – rail
Servo Red (VCC) → Breadboard + rail
Servo Brown/Black (GND) → Breadboard – rail

### Servo Signal Pins:
Servo 1 → D4  
Servo 2 → D5  
Servo 3 → D6  
Servo 4 → D7

## Code Description

The Arduino sketch performs the following:

1. Runs a **for loop** to sweep each servo motor (0° to 180° and back) for approximately **2 seconds** per servo.
2. After all four motors are done, it sets all servos to **90°** using `servo.write(90)`.

This is done to simulate the behavior required in the task:  
"Run using the sweep example for 2 seconds after that make all motors hold at 90 degrees."_
 can find the full code in the main `.ino` file in this repo.

## Important Notes

The Arduino UNO alone **cannot power 4 servos at once**. This is why we used an **external 9V battery**.
The **common ground** between the Arduino and external power source is critical. Without it, PWM signals won’t work.
If servos jitter, you may consider adding a **capacitor (100uF-470uF)** between + and – on the breadboard to stabilize voltage.
On Tinkercad simulation, sometimes only 1 servo appears to move — this is due to simulation limitations, not wiring or code.

##  Result

All 4 servos perform a sweep motion one by one.
After sweeping, all motors stop at 90 degrees.
Power is stable and code performs as expected.
External battery successfully powers all servos.

------------------------
code = 
#include <Servo.h>

// Create servo objects
Servo servo1, servo2, servo3, servo4;

void setup() {
  servo1.attach(4);  // Servo 1 to pin 4
  servo2.attach(5);  // Servo 2 to pin 5
  servo3.attach(6);  // Servo 3 to pin 6
  servo4.attach(7);  // Servo 4 to pin 7
}

void loop() {
  // Sweep each servo for ~2 seconds (one by one)

  for (int i = 0; i < 2; i++) {
    for (int pos = 0; pos <= 180; pos += 5) {
      servo1.write(pos);
      delay(10);
    }
    for (int pos = 180; pos >= 0; pos -= 5) {
      servo1.write(pos);
      delay(10);
    }
  }

  for (int i = 0; i < 2; i++) {
    for (int pos = 0; pos <= 180; pos += 5) {
      servo2.write(pos);
      delay(10);
    }
    for (int pos = 180; pos >= 0; pos -= 5) {
      servo2.write(pos);
      delay(10);
    }
  }

  for (int i = 0; i < 2; i++) {
    for (int pos = 0; pos <= 180; pos += 5) {
      servo3.write(pos);
      delay(10);
    }
    for (int pos = 180; pos >= 0; pos -= 5) {
      servo3.write(pos);
      delay(10);
    }
  }

  for (int i = 0; i < 2; i++) {
    for (int pos = 0; pos <= 180; pos += 5) {
      servo4.write(pos);
      delay(10);
    }
    for (int pos = 180; pos >= 0; pos -= 5) {
      servo4.write(pos);
      delay(10);
    }
  }

  // Hold all at 90 degrees
  servo1.write(90);
  servo2.write(90);
  servo3.write(90);
  servo4.write(90);

  while (true);
}
-------------------------------------------------------

## Walking Motion Algorithm (Step-by-Step)

The walking movement of a humanoid robot using 4 servo motors is simulated as follows:

### Assumptions:
The robot has two legs, each with 2 servo motors:
Servo 1: Left Hip
Servo 2: Left Knee
Servo 3: Right Hip
Servo 4: Right Knee
 The robot is standing straight initially (all servos at 90 degrees).

## Walking Algorithm:

1. **Initialize all servos to 90° (standing position).**

2. **Lift Left Leg:**
    Move **Servo 1 (Left Hip)** to 60° → This raises the left leg.
    Move **Servo 2 (Left Knee)** to 120° → Simulates bending the knee.

3. **Swing Left Leg Forward:**
    Slightly move **Servo 1** forward to 80° to push the leg ahead.

4. **Place Left Leg Down:**
    Move **Servo 2** back to 90° (straight knee).
    Move **Servo 1** back to 90° (neutral position).

5. **Shift weight to left leg** (keep all servos stable momentarily).

6. **Lift Right Leg:**
  Move **Servo 3 (Right Hip)** to 120° → Raises right leg.
  Move **Servo 4 (Right Knee)** to 60° → Bends right knee.

7. **Swing Right Leg Forward:**
    Move **Servo 3** to 100° to push forward.

8. **Place Right Leg Down:**
    Move **Servo 4** back to 90°.
    Move **Servo 3** back to 90°.

9. **Repeat steps 2–8 for continuous walking.**

### Note:
This walking logic can be implemented in Arduino code by combining `servo.write()` calls and delays to simulate real movement. It can also be adapted to real robotic joints depending on the number and placement of servo motors.
