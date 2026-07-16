
# WRO2026-PiolinTech

<p align="center">
Documentation for team PiolinTech's robot for WRO 2026 - Future Engineers
</p>

<p align="center">
<img width="823" height="477" alt="image" src="https://github.com/user-attachments/assets/812b7146-2706-4745-93e5-c2bc14222051" />
</p>

<p align="center">
  <a href="https://instagram.com/piolintech">
    <img src="https://img.shields.io/badge/Instagram-@piolintech-E4405F?style=for-the-badge&logo=instagram&logoColor=white" alt="Instagram Piolintech">
  </a>
  <a href="https://youtube.com/@piolintech">
    <img src="https://img.shields.io/badge/YouTube-Piolín_Tech-FF0000?style=for-the-badge&logo=youtube&logoColor=white" alt="YouTube Piolintech">
  </a>
</p>

Welcome to the official repository for Piolín, our autonomous robotic vehicle designed and built for the World Robot Olympiad (WRO) Future Engineers competition. This repository contains the complete mechanical designs, electrical schematics, firmware files, and computer vision algorithms developed by our team. **Piolín** is an advanced autonomous robotic vehicle engineered to compete in the **WRO Future Engineers 2026** category. Running on a high-performance **Raspberry Pi 5** processing core, the robot handles real-time edge computer vision via a **Huskylens** smart camera for strict lane alignment. It fuses visual telemetry with an array of **three ultrasonic distance sensors** to navigate complex track curves, identify lane markers, and safely execute dynamic obstacle evasion.

---

## General Index

* **[Get to know us](./t-gtku)**: Meet the team, our story, and our specific roles in development.
* **[Piolin Overall](./v-photos)**: Orthogonal and perspective photography of the physical vehicle.
* **[Our Robot on Action](./Video)**: Video logs and live track test runs.
* **[Important Documents](./docs)**:
* **[Outside of Piolin](./docs/hardware)**: Hardware design, chassis specs, and mechanical calculations.
* **[Inside of Piolin](./docs/software)**: Software logic, libraries, and communication protocols.

* **[Our Programming](./code)**:
* **[R1](./code/Round1)**: Source code and calibration files used in Round 1.
* **[R2](./code/Round2)**: Optimized logic and sensor adjustments utilized in Round 2.

* **[Our Evolution](./models)**: Detailed progression of our physical designs and prototypes.
* **[Technical Details](./schemes)**: Circuit schematics, power distribution maps, and wiring.
* **[Flowcharts and Important Resources](./embed)**: Core logic, state machines, and state diagrams.

---

### Team Members

| Member | Information | Contact |
| :---: | :--- | :---: |
| <img src="https://github.com/user-attachments/assets/b11aaff5-3de3-4762-b0c1-a0094b9cf4e7" width="250" height="370" style="border-radius: 50%; object-fit: cover;"> | **Sebastián Martínez**<br>Colegio Bilingüe de Panamá | [📸 Instagram](https://www.instagram.com/sebastian.mvrl/) |
| <img src="https://github.com/user-attachments/assets/dc507a8b-f1c1-435e-9df6-96d2e11e0cba" width="250" height="370" style="border-radius: 50%; object-fit: cover;"> | **Mia Cantoral**<br>Colegio Bilingüe de Panamá | [📸 Instagram](https://www.instagram.com/miaacnt) |
| <img src="https://github.com/user-attachments/assets/0b5f11e1-4c58-45dd-b63a-45f6c2c5726a" width="250" height="370" style="border-radius: 50%; object-fit: cover;"> | **Christian Castrellón**<br>Colegio Bilingüe de Panamá | [📸 Instagram](https://www.instagram.com/cj.chriss) |
| **Coach** | **Hanna Figueroa**<br>Thank you teacher Hanna for being our brightest and biggest inspiration out there. We truly admire and love you! :) | |

---

## Goal & Structure

### Our Vision

Our primary objective is to develop an elegant, highly reproducible autonomous vehicle capable of completing both the Open Challenge and the Obstacle Challenge with maximum speed and reliability. By using a hybrid setup, combining the raw mechanical flexibility of the LEGO/SPIKE ecosystem with custom DC motors, microcontrollers, and a Raspberry Pi 5, we bridge the gap between educational building platforms and advanced industrial robotics.

### Team Goals 

* **Maintain Trajectory Precision**: Achieve zero lateral sliding during turns by refining our physical Ackermann implementation and steering PID loops.
* **Zero-Latency Obstacle Detection**: Maximize edge processing speeds with the Raspberry Pi 5 and HuskyLens 2, keeping detection loop latency below 15ms.
* **Mechanical Modularity**: Ensure that any structural, sensor, or cabling component can be swapped out in under 2 minutes during pit lane runs.
* **Flawless Execution at Nationals**: Deliver consistent, collision-free runs under varying light conditions and surface frictions.

---

## 1. Dimension Table

The following table outlines the key physical and mechanical dimensions of Piolín, strictly adhering to the WRO Future Engineers regulations:

| Technical Parameter | Specification Value | Engineering Notes / WRO Rules |
| --- | --- | --- |
| **Total Length** | 170 mm | Well within the maximum 250 mm limit. |
| **Total Width** | 140 mm | Calculated track width edge to edge of tires. |
| **Total Height** | 120 mm | Lowered center of gravity profile. |
| **Wheelbase ($L$)** | 165 mm | Measured pivot to pivot distance for Ackermann calculations. |
| **Track Width ($W$)** | 140 mm | Center to center lateral tire spacing. |
| **Rear Tires (Propulsion)** | 56.18 mm (Diameter) | High-grip LEGO SPIKE Blue rubber wheels. |
| **Front Tires (Steering)** | 42.8 mm (Diameter) | Low-friction guide wheels for effortless steering pivot. |
| **Total Vehicle Mass** | 721.41 g | Mass optimized to reduce momentum during sudden turns. |

---

## 2. Feature Table

Our hardware selection is strategically divided to separate real-time sensory-actuator tasks from heavy mathematical processing:

| System | Component | Primary Feature / Technical Specification |
| --- | --- | --- |
| **High-Level Processor** | **Raspberry Pi 5** | 2.4 GHz Quad-Core ARM Cortex-A76. Runs camera image parsing, PID calculations, and state machines. |
| **Low-Level Microcontroller** | **Arduino Nano** | ATmega328P. Handles instant hardware PWM generation for steering and millisecond sensor interrupts. |
| **Computer Vision Engine** | **HuskyLens 2** | AI-driven camera connected via hardware I2C; localizes color pillars on the fly. |
| **Distance Telemetry** | **HC-SR04 (x3)** | Active ultrasonic transducers angled at 30 degrees to sense diagonal walls predicted ahead. |
| **Propulsion Power** | **Metal-Geared DC Motors** | High-RPM micro motors providing rapid acceleration out of tight turns. |
| **Steering Precision** | **Micro Metal Servo** | Digital high-torque feedback actuator driving the steering rack without backlash. |
| **Power Protection** | **Voltage Dividers** | 1 kOhm and 2 kOhm resistor pairs stepping Echo signals down from 5V to a safe 3.3V. |

---

## 4. Structural Evolution (v1, v2, & v3)

The mechanical architecture of our robot transitioned through three distinct phases to resolve physical weaknesses under live track conditions:

1. **Version 1 (Phase 1.0 - LEGO Technic Base)**:
The early model relied entirely on a standard LEGO Technic chassis driven by the LEGO Mindstorms EV3 Intelligent Brick. The primary limitation was structural play; the flexible nature of plastic snap-pin connectors allowed significant chassis twist under high steering torque, leading to physical track drift. Additionally, the EV3 processor suffered from severe loop latency and thread jitter when trying to parse ultrasonic data and color readings concurrently.
2. **Version 2 (Phase 1.5 - SPIKE Box Braced)**:
To address chassis flex, the frame was rebuilt using cross-braced white and grey LEGO SPIKE Prime beams, forming a rigid overhead bridge. While this successfully eliminated the vertical chassis twist, the system still suffered from electrical contact dropouts. Standard RJ12 telephone-style cables vibrated loose inside the EV3 ports during track runs, requiring manual tape reinforcement on critical telemetry lines to prevent software freezes.
3. **Version 3 (Current Hybrid Configuration)**:
The current active configuration implements a hybrid structural paradigm. We preserved the rigid SPIKE Prime box-frame structure for compliance and modularity, but replaced the heavy EV3 brick and LEGO motors with a Raspberry Pi 5, an Arduino Nano, and micro metal-geared DC motors. By soldering standard pin connectors and mounting the side ultrasonic sensors at a 30-degree forward-facing angle, we achieved stable telemetry, predictive wall sensing, and a lower overall center of gravity.

---

## 5. Logic (Flowchart Logic)

### Multi-Threaded Software Architecture

The control software relies on a deterministic execution flow designed to prevent sensor polling delays from lagging our physical actuation. Thread 1 constantly queries the three HC-SR04 sensors and the HuskyLens 2 camera over I2C to write raw telemetry to a shared memory block. Thread 2 reads these clean values at a constant execution speed of 100 Hz to update the steering and propulsion states.



```
             [System Boot & Initialization]
                           │
                           ▼
                [Read Sensor Telemetry]
         (3x Ultrasonic Distances & Camera I2C)
                           │
          ┌────────────────┴────────────────┐
          ▼                                 ▼
 [Obstacle Detected?]             [Clear Path / Wall Following]
   (Center < 25 cm)                       (Center >= 25 cm)
          │                                 │
          ▼                                 ▼


[Query Huskylens Camera]            [Run PD Control Loop]
(Check Pillar: Red vs. Green)      Error = Dist_Left - Dist_Right
│                        Output = Kp * e + Kd * (de/dt)
▼                                 │
[Inject Steering Shift Angle]                  ▼
(Execute Dodge Maneuver)         [Adjust Servo Direction]

```

### Proportional-Derivative (PD) Control Loop

When navigating clear stretches of the track, the vehicle maintains central lane positioning using a PD wall-following algorithm. The system continuously evaluates the difference between the left and right ultrasonic distance sweeps to compute an instantaneous corrective error:

$$e(t) = \text{Dist}_{\text{left}} - \text{Dist}_{\text{right}}$$

The error is processed by our steering loop to determine the target angle for the servo:

$$u(t) = K_p \cdot e(t) + K_d \cdot \frac{de(t)}{dt}$$

Where $K_p$ represents our proportional gain (correcting current drift) and $K_d$ represents our derivative gain (counteracting the rate of drift to prevent overshooting at the exit of turns).

### Obstacle Challenge Decision Logic

When the center-facing ultrasonic sensor reports a distance below 25cm, a software override suspends the PD wall-following routine. The control system queries the HuskyLens 2 color-signature block:

* **Red Pillar Identified**: The robot shifts its target trajectory offset to the right of the obstacle.
* **Green Pillar Identified**: The robot shifts its target trajectory offset to the left of the obstacle.
* **No Active Pillar Identified**: The robot defaults to ultrasonic braking to prevent high-speed collisions.

---

## Media & Resources

### Vehicle Photos

Physical reference orthogonal views are located in the [Vehicle Images](./v-photos) directory:

| Front View | Back View | Left View |
| :---: | :---: | :---: |
| <img src="./v-photos/Ptech_Front.png" width="300"> | <img src="./v-photos/Ptech_Back.png" width="300"> | <img src="./v-photos/Ptech_Left.png" width="300"> |
| **Right View** | **Top View** | **Bottom View** |
| <img src="./v-photos/Ptech_Right.png" width="300"> | <img src="./v-photos/Ptech_Top.png" width="300"> | <img src="./v-photos/Ptech_Down.png" width="300"> |

### Team Photos

Team pictures, project timelines, and development workspace documentation are located under the [Get to know us](./t-gtku) directory.

### Performance Videos

Video records of our open track trials and competitive speed runs are located in the [Videos](./videos) directory.


