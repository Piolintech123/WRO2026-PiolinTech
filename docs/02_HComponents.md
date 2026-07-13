<div align="center">
  <img src="https://github.com/user-attachments/assets/839b917a-c2bc-4053-8b2a-f68e43b7efa0" alt="Comparison" style="max-width: 65%; height: auto;" />
</div>

# 2. Robot Hardware and Components Evolution

Our design process has been a journey of overcoming hard technical limitations. We didn't build this current iteration of Piolín overnight; it is the direct result of testing what worked and discarding what failed under actual competition stress. 

---

## 2.1 The Evolutionary Timeline: From Blind Navigation to AI Vision

### Phase 1: Last Year (The Cameraless Baseline)
When we first started last year, we went through the competitions without a camera at all. Our entire navigation strategy relied purely on spatial distancing using basic sensors to bounce off walls and find lanes, and to be fair, it relied on luck. While this mechanical simplicity made our platform highly reliable and less prone to code crashes, it lacked any form of intelligent track awareness. It functioned well on standard straight paths, but it could not adapt to dynamic color cues or sudden obstacle shifts on the field.

### Phase 2: This Year's First Model (The PixyCam 2.1 Transition)
To meet the demands of the 2026 game rules, we designed our first model of this season to incorporate a PixyCam 2.1 image sensor. This was our first step into vision-based navigation. 
*   **The Component Specs:** Operating at 5V, the PixyCam 2.1 tracked color signatures at 60 frames per second using its own internal processor.
*   **The Technical Failure:** While fast on paper, the PixyCam 2.1 struggled heavily with lighting variations. The overhead venue lights at the track created severe glare zones on the floor tiles. The camera repeatedly lost its color lock matrix, causing the steering system to register false tracking errors. We spent more time manually tuning color thresholds for changing ambient light profiles than refining our actual driving algorithms.

### Phase 3: The Current Competition Model (The Huskylens AI Platform)
To fix the lighting and tracking dropouts once and for all, we upgraded to the Huskylens AI Camera Module. 
*   **The Component Specs:** Operating at 3.3V–5.0V via a direct I2C bus connection to our master controller, the Huskylens features an onboard Kendryte K210 dual-core RISC-V processor and a 2.0-inch IPS display screen. 
*   **Why It Changed Everything:** Unlike the PixyCam, the Huskylens uses machine-learning algorithms to lock onto objects and color tracks. We mounted it at a fixed height of 9.5 cm with a 15-degree downward pitch. This geometric positioning shields the physical lens from overhead glare. It processes color block tracking internally and sends clean, filtered coordinate packages to our main thread, dropping our vision tracking error rate down below 3%. *(For a complete statistical breakdown of these vision error rates under varying track conditions and how we calculated them, please refer to [Performance Testing and Analytics documentation](WRO2026-PiolinTech/docs/12_PTesting&Analysis.md)
  
---

## 2.2 Core Processing Shift: Moving from EV3/Pybricks to Raspberry Pi 5

Our processing architecture had to undergo an identical overhaul to support this leap in vision technology.

### The Old Setup: LEGO EV3 & Pybricks MicroPython
We originally built our control loops around the LEGO EV3 intelligent brick running Pybricks MicroPython.
*   **The Setup Details:** A single-core ARM9 processor running at 300MHz, using standard RJ12 sensor ports.
*   **The Technical Bottleneck:** The EV3 hub simply did not have the processing power or bus speeds to handle real-time coordinate streams from an external camera while simultaneously calculating traction loops. It introduced a critical 75ms data propagation lag. At driving speeds above 0.4 m/s, the robot would physically overshoot the lane center before the MicroPython script could calculate and send the new angle to the steering motor.

### The New Setup: Raspberry Pi 5 (8GB)
To handle true multi-threaded sensor fusion, we migrated completely away from LEGO to a custom hardware platform centered on the Raspberry Pi 5.
*   **The Setup Details:** Operating at 5.1V with a 2.4GHz quad-core 64-bit ARM processor, utilizing direct hardware I2C buses and standard GPIO pins.
*   **The Engineering Impact:** The Pi 5 gives us true parallel processing. We split our code into concurrent threads: one thread continuously polls the ultrasonic sensors at 40Hz, while a separate thread grabs the I2C vision matrices from the Huskylens. This completely wiped out our data lag, dropping our control loop response time down to under 1ms and letting us push the robot's track velocity to its absolute physical limits.

---

## 2.3 Sensor and Power Architecture Components
s power pin. This electrical isolation guarantees zero power-induced system resets.
