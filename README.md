# Ball Balancing Control System with PID Controller

A ball-on-beam balancer built with an Arduino Uno. An HC-SR04 ultrasonic sensor measures where the ball is on the ramp. The Arduino runs a PID loop on that reading and tilts the ramp with a servo motor, which holds the ball at a 15 cm setpoint.

![DIY frame for ball balancing](DIY%20Frame%20for%20Ball%20Balancing.jpg)

---

## How It Works

1. **Measure.** The HC-SR04 at one end of the ramp measures the distance to the ball in centimetres.
2. **Compute.** The Arduino finds the error between the 15 cm setpoint and the measured distance, then calculates the PID output.
3. **Act.** The output is mapped to a servo angle between 0° and 135°. The servo tilts the ramp through a movable arm, and the ball rolls back toward the setpoint.
4. **Repeat.** The loop runs continuously.

```
setpoint (15 cm) ─► error ─► PID ─► servo angle ─► ramp tilt ─► ball position
                      ▲                                              │
                      └──────────── HC-SR04 distance ◄───────────────┘
```

---

## Hardware

| Qty | Component |
|---|---|
| 1 | Arduino Uno |
| 1 | Servo motor |
| 1 | HC-SR04 ultrasonic sensor |
| 1 | Breadboard |

### Wiring

| Arduino pin | Connected to |
|---|---|
| D2 | HC-SR04 Trig |
| D3 | HC-SR04 Echo |
| D5 | Servo signal |

### Frame

The frame is a DIY build made from:

- cork sheet
- a glue gun
- a metal stick
- pen shells
- a foam ball

The parts:

- **Ramp:** an 11-inch hollow prism-shaped bridge made of cork sheet, open on one side.
- **Base:** one large piece of cork sheet.
- **Pivot:** two trapezoidal cork pieces hold the pivot point.
- **Servo and sensor:** the servo drives one end of the ramp through a movable arm, and the ultrasonic sensor sits at the other end.

---

## Code

The full sketch is in [`ball_balancing.ino`](ball_balancing.ino).

| Part | What it does |
|---|---|
| Definitions | Includes `Servo.h`, sets the sensor pins (`trig` = 2, `echo` = 3) and the gains `kp`, `ki` and `kd` |
| `setup()` | Sets the pin modes, attaches the servo to pin 5, starts serial at 9600 baud and moves the servo to 50° |
| `loop()` | Calls `PID()` repeatedly |
| `distance()` | Sends a 10 µs trigger pulse, times the echo with `pulseIn`, and converts it to centimetres |
| `PID()` | Computes the error from the 15 cm setpoint, sums the P, I and D terms, maps the result to a servo angle and limits it to 0–135° |

### PID gains

| Gain | Value |
|---|---|
| `kp` | 100 |
| `ki` | 0 |
| `kd` | 200 |

With `ki` = 0, the controller works as a PD controller.

### Tuning method

1. Set `ki` and `kd` to 0. Increase `kp` until the system oscillates, then lower it until the oscillations die out quickly.
2. Increase `ki` from a small value until overshoot appears, then reduce it to minimize overshoot.
3. Increase `kd` until overshoot and oscillation drop. Back it off if the system becomes unstable.

---

## Running

1. Wire the sensor and servo as shown in the wiring table.
2. Open `ball_balancing.ino` in the Arduino IDE and upload it to the Uno.
3. Place the ball on the ramp. The servo adjusts the tilt to keep it 15 cm from the sensor.
4. Open the Serial Monitor at 9600 baud to watch the PID output.

---

## Future Improvements

- **Longer ramp:** would need a stronger motor and retuned gains.
- **Smoother surface:** less friction makes the ball easier to control.
- **Better position sensing:** to replace the ultrasonic sensor.
- **Different motor type:** in place of the servo.
-
-
**Application and scope** <br>
Ball balancing platform will be used to implement, monitor, and analyze the aforementioned control design approach for the sake of study. The goal is to keep a ball from rolling off a flat surface by compensating for factors like wind and other external forces.
In addition to traditional building materials, three distinct pieces of equipment will be used to bring the edifice to life, generate data, and obtain the appropriate motion output. In the thesis, the platform is a resistive touch panel that relays information about the ball's location to a microcontroller. A pair of servo motors are controlled by a microprocessor, which in turn determines the ball's speed and acceleration based on the input data. In response to a control signal from a microcontroller, servo motors cause an angular displacement by altering their position. The study will make use of the information gathered from both the physical model and the simulation of the system. In order to test, we will manually perturb the ball's position and then observe the effect on the system's steady-state stability. This thesis attempts to address the following issues:
1. Can a ball balancing platform be built with sufficient performance using only linear control techniques?
2. What, if anything, accounts for the discrepancies between theoretical models and experimental results?
3. To what extent do practical results differ from theoretical predictions when attempting to stabilize a system that is inherently unstable or only partially stable?
