<div align="center">
  <img src="https://github.com/user-attachments/assets/839b917a-c2bc-4053-8b2a-f68e43b7efa0" alt="Comparison" style="max-width: 65%; height: auto;" />
</div>

# 2. Robot Hardware and Components Evolution

Our design process has been a journey of overcoming hard technical limitations. We didn't build this current iteration of Piolín overnight; it is the direct result of testing what worked and discarding what failed under actual competition stress. 

---

## 2.1 Technical analysis of each components:

*   ### Component: Huskylens AI Camera Module (Current Primary Vision)
*   **Quantity:** 1
*   **Voltage:** 3.3V - 5.0V
*   **Current Consumption:** ~320 mA (with LCD screen active)
*   **Interface:** I2C Bus / UART Serial Connection
*   **Description:** Smart AI vision sensor capable of hardware-accelerated machine learning object tracking and color block matrix recognition. Used as the core track-line alignment and adaptive visual navigation system. 
<div align="center">
  <img src="https://github.com/user-attachments/assets/737bd86d-a82a-46fa-b683-06f18ec4721b" alt="Component Image" style="max-width: 30%; height: auto; border-radius: 8px; margin: 10px 0;" />
</div>

---

### Component: Raspberry Pi 5 - 8GB RAM (Processing Core)
*   **Quantity:** 1
*   **Voltage:** 5.1V
*   **Current Consumption:** Up to 5.0A (Peak workload output)
*   **Interface:** GPIO / Native I2C / Hardware PWM / UART
*   **Description:** High-performance 64-bit quad-core microcomputer hosting our multi-threaded control architecture. Manages asynchronous sensor data ingestion, executes real-time digital filtering, and computes the master steering PID control outputs. 
<div align="center">
  <img src="https://github.com/user-attachments/assets/9fb3da92-7bec-4a39-8955-44342d2e165f" alt="Component Image" style="max-width: 30%; height: auto; border-radius: 8px; margin: 10px 0;" />
</div>

---

### Component: HC-SR04 Ultrasonic Distance Sensor
*   **Quantity:** 3
*   **Voltage:** 5.0V
*   **Current Consumption:** ~15 mA per unit
*   **Interface:** Digital GPIO (Dedicated Trigger / Echo Pins)
*   **Description:** High-frequency ultrasonic ranging module providing real-time spatial telemetry from 2 cm to 400 cm. Staggered in a triple-sensor array (-30°, 0°, +30°) to drive our background obstacle evasion subroutines and parallel wall-alignment safety tracking loops.
<div align="center">
  <img src="https://github.com/user-attachments/assets/f5ddc783-744e-44fe-a237-15ff04f8e7cc" alt="Component Image" style="max-width: 30%; height: auto; border-radius: 8px; margin: 10px 0;" />
</div>


---

### Component: TB6612FNG Dual Motor Driver IC
*   **Quantity:** 1
*   **Voltage:** Logic (VCC): 2.7V - 5.5V / Motor Power (VM): Up to 13.5V
*   **Current Consumption:** 1.2A Continuous output current (3.2A peak spikes)
*   **Interface:** Hardware PWM (Speed Control) & Digital GPIO (Direction Pins)
*   **Description:** High-efficiency Dual H-Bridge integrated circuit used to route regulated current lines directly to the rear traction DC motor based on instructions parsed from the Raspberry Pi 5 control loop.
<div align="center">
  <img src="https://github.com/user-attachments/assets/dedaad37-bdad-43fa-a155-460b2aa46a68" alt="Component Image" style="max-width: 30%; height: auto; border-radius: 8px; margin: 10px 0;" />
</div>


---

### Component: XL6019E1 DC-DC Automatic Buck-Boost Converter
*   **Quantity:** 1
*   **Voltage:** Input: 5V - 32V / Output: Configured to stable 7.4V Motor Rail
*   **Current Consumption:** Handles up to 5.0A output current capacity
*   **Interface:** Direct Hardwired Power Plane Routing
*   **Description:** High-frequency power management regulator board deployed to electrically isolate inductive motor acceleration surges away from the delicate digital logic rails powering our main processor.
<div align="center">
  <img src="https://github.com/user-attachments/assets/174f5059-9500-4f5d-90b7-229c98128c7a" alt="Component Image" style="max-width: 30%; height: auto; border-radius: 8px; margin: 10px 0;" />
</div>

---

### Component: High-Torque Micro Servo Motor
*   **Quantity:** 1
*   **Voltage:** 4.8V - 6.0V
*   **Current Consumption:** ~250 mA (Idle) / 1.2A (Stall peak under load)
*   **Interface:** Hardware PWM ($50\text{ Hz}$ frequency, $20\text{ ms}$ periodic window)
*   **Description:** Metal-geared actuator linked directly to our physical front steering linkage blocks to translate spatial orientation adjustments into exact mechanical Ackermann angles.
<div align="center">
  <img src="https://github.com/user-attachments/assets/847ce23e-9087-465c-9e8f-98352007fc7a" alt="Component Image" style="max-width: 30%; height: auto; border-radius: 8px; margin: 10px 0;" />
</div>

  
## 2.2 The Evolutionary Timeline: From Blind Navigation to AI Vision

### Phase 1: Last Year (The Cameraless Baseline)
When we first started last year, we went through the competitions without a camera at all. Our entire navigation strategy relied purely on spatial distancing using basic sensors to bounce off walls and find lanes, and to be fair, it relied on luck. While this mechanical simplicity made our platform highly reliable and less prone to code crashes, it lacked any form of intelligent track awareness. It functioned well on standard straight paths, but it could not adapt to dynamic color cues or sudden obstacle shifts on the field.

### Phase 2: This Year's First Model (The PixyCam 2.1 Transition)
To meet the demands of the 2026 game rules, we designed our first model of this season to incorporate a PixyCam 2.1 image sensor. This was our first step into vision-based navigation. 
*   **The Component Specs:** Operating at 5V, the PixyCam 2.1 tracked color signatures at 60 frames per second using its own internal processor.
*   **The Technical Failure:** While fast on paper, the PixyCam 2.1 struggled heavily with lighting variations. The overhead venue lights at the track created severe glare zones on the floor tiles. The camera repeatedly lost its color lock matrix, causing the steering system to register false tracking errors. We spent more time manually tuning color thresholds for changing ambient light profiles than refining our actual driving algorithms.

### Phase 3: The Current Competition Model (The Huskylens AI Platform)
To fix the lighting and tracking dropouts once and for all, we upgraded to the Huskylens AI Camera Module. 
*   **The Component Specs:** Operating at 3.3V–5.0V via a direct I2C bus connection to our master controller, the Huskylens features an onboard Kendryte K210 dual-core RISC-V processor and a 2.0-inch IPS display screen. 
*   **Why It Changed Everything:** Unlike the PixyCam, the Huskylens uses machine-learning algorithms to lock onto objects and color tracks. We mounted it at a fixed height of 9.5 cm with a 15-degree downward pitch. This geometric positioning shields the physical lens from overhead glare. It processes color block tracking internally and sends clean, filtered coordinate packages to our main thread, dropping our vision tracking error rate down below 3%. *(For a complete statistical breakdown of these vision error rates under varying track conditions and how we calculated them, please refer to [Performance Testing and Analytics documentation](WRO2026-PiolinTech/docs/10_PTesting&Analysis.md) )*
  
---

## 2.3 Core Processing Shift: Moving from EV3/Pybricks to Raspberry Pi 5

Our processing architecture had to undergo an identical overhaul to support this leap in vision technology.

### The Old Setup: LEGO EV3 & Pybricks MicroPython
We originally built our control loops around the LEGO EV3 intelligent brick running Pybricks MicroPython.
*   **The Setup Details:** A single-core ARM9 processor running at 300MHz, using standard RJ12 sensor ports.
*   **The Technical Bottleneck:** The EV3 hub simply did not have the processing power or bus speeds to handle real-time coordinate streams from an external camera while simultaneously calculating traction loops. It introduced a critical 75ms data propagation lag. At driving speeds above 0.4 m/s, the robot would physically overshoot the lane center before the MicroPython script could calculate and send the new angle to the steering motor.

### The New Setup: Raspberry Pi 5 (8GB)
To handle true multi-threaded sensor fusion, we migrated completely away from LEGO to a custom hardware platform centered on the Raspberry Pi 5.
*   **The Setup Details:** Operating at 5.1V with a 2.4GHz quad-core 64-bit ARM processor, utilizing direct hardware I2C buses and standard GPIO pins.
*   **The Engineering Impact:** The Pi 5 gives us true parallel processing. We split our code into concurrent threads: one thread continuously polls the ultrasonic sensors at 40Hz, while a separate thread grabs the I2C vision matrices from the Huskylens. This completely wiped out our data lag, dropping our control loop response time down to under 1ms and letting us push the robot's track velocity to its absolute physical limits.
<div align="center">
  <img src="https://github.com/user-attachments/assets/cfe3f9ee-133d-4ae6-8f36-8364eacce575" alt="Component Image" style="max-width: 30%; height: auto; border-radius: 8px; margin: 10px 0;" />
</div>


---

## 2.4 Sensor and Power Architecture Components

To complement our processing core shift, our peripheral sensor and power routing networks had to undergo an identical engineering optimization.

### Spatial Awareness: 3x HC-SR04 Ultrasonic Distance Sensors
*   **The Component Specs:** Operating at a strict 5.0V input rail, the HC-SR04 features a functional distance sensing band of 2 cm to 400 cm with an effective acoustic measurement cone of 15 degrees. 
*   **The Integration Details:** We structurally staggered three identical ultrasonic sensors along our custom front bumper array at fixed angular geometric offsets: -30° (Left), 0° (Center), and +30° (Right). This specific array layout expands our front safety boundary envelope to an unbroken 180 degrees, completely neutralizing physical blind spots during corner sweeps. To prevent raw signal flutter or false echo bounces caused by physical track seams and venue floor vibrations, we route the signal strings through a software-based low-pass median filter running inside our dedicated background thread.

### Actuation: High-Torque Steering Servo & Rear DC Motor
*   **The Component Specs:** The front steering steering changes are managed via a high-torque micro servo operating across a 4.8V–6.0V range via hardware Pulse Width Modulation (PWM) lines. Rear traction, linear track velocity, and mid-corner acceleration are governed by a high-output DC motor managed by a **TB6612FNG Motor Driver Module** (configured at 2.7V–5.5V for logic, with up to 13.5V for the motor input bus).
*   **The Integration Details:** The steering linkage is bound directly to a custom 3D-printed mechanical assembly utilizing Ackermann steering geometry. This mechanical configuration balances steering angle differentials between the inner and outer front wheels during tight cornering arcs, preventing the tires from scrubbing off valuable linear velocity. The TB6612FNG driver routes high-amperage current pulses (sustaining 1.2A continuous, with 3.2A peak capabilities) directly to the rear motor axle, translating target software variables into immediate physical propulsion.

### Power Isolation: XL6019E1 DC-DC Converter & 22.5W Power Bank
*   **The Component Specs:** Our central mobile power reservoir consists of a 22.5W intelligent power bank equipped with multi-protocol Power Delivery (PD) and Quick Charge capabilities. The power distribution is managed via an **XL6019E1 automatic buck-boost converter module** featuring a high-frequency 180KHz switching architecture and up to 5A output current handling capacity.
*   **The Integration Details:** In our early designs this season, we made the mistake of running the Raspberry Pi 5 logic rails and the high-draw DC propulsion motors off a single, shared power rail. Whenever the vehicle accelerated sharply out of an evasion turn, the motor's sudden current draw dropped the line voltage under 4.5V, triggering immediate processing brownouts and kernel panics on the Pi 5. To fix this structural flaw, we utilized the XL6019E1 converter to isolate the lines. The power bank splits its output at the source: a clean, dedicated 5.1V logic line runs straight to the Pi 5, while a separate power leg feeds the XL6019E1, which steps up and regulates a completely isolated high-current feed directly to the motor driver's power pin. This setup completely eliminates inductive electrical noise feedback, ensuring zero power-induced system resets across extensive track tests.

