# 3.0 Overview of Pin Configuration


The following table summarizes the primary pin assignments for all the components on the Raspberry Pi 5:

| Device Element | Host Controller Interface | Core System Purpose |
| :--- | :--- | :--- |
| **Huskylens Module** | GPIO2 (SDA) / GPIO3 (SCL) / 3V3 / GND | Visual data matrix ingestion & power loop |
| **Steering Servo** | GPIO15 (PWM Output) / 5V / GND | Angle positioning adjustment signals |
| **Ultrasonic Sensor 1** | GPIO17 (Trigger Line) / Shared Echo Rail | Distance tracking (Array Element 1) |
| **Ultrasonic Sensor 2** | GPIO27 (Trigger Line) / Shared Echo Rail | Distance tracking (Array Element 2) |
| **Ultrasonic Sensor 3** | GPIO23 (Trigger Line) / Shared Echo Rail | Distance tracking (Array Element 3) |
| **Protection Resistor** | GPIO22 / GPIO24 / GPIO18 | Hardware safety voltage division |
| **PixyCam 2.1** | *Legacy Connection* | Removed prototyping vision bus |

---

## 3.1 Sensor Information and Pin Configuration

### Ultrasonic Sensors (HC-SR04)

*   **Functionality:** Operates on echolocation principles, emitting an ultrasonic pulse and measuring the time for its return echo to calculate distance. Used for obstacle avoidance.
*   **Role:** Three modules form a staggered front array, covering a full 180-degree safety envelope. This geometric layout wipes out blind spots along the flanks when sweeping through corners.
*   **Evolutionary Jump:** Last year, our early test platforms used a single center sensor, which created blind spots during sharp cornering turns. Upgrading to this triple-staggered array gives the Pi 5 an unbroken spatial safety map, allowing the robot to parallel-align with boundary walls during evasion states.
*   **Pin Configuration:**
    *   **VCC:** Tied directly to the 5V line.
    *   **GND:** Linked to the shared system ground plane.
    *   **TRIG Lines:** Sensor 1 maps to GPIO17, Sensor 2 maps to GPIO27, and Sensor 3 maps to GPIO23.
    *   **ECHO Lines:** Directed across all modules to the shared GPIO22 and GPIO24 input rails via the resistor protection bridge.
*   **Library:** Managed by an asynchronous background thread utilizing hardware timers and custom digital filtering (low-pass median filter) to eliminate false echo spikes from track seams.

### Inline Resistor Layout

*   **Functionality:** Deployed as a 200 Ohm inline protective element to handle clean logic translation and step down input lines.
*   **Engineering Impact:** Because the HC-SR04 ultrasonic sensors output a 5V signal on their Echo lines, they pose a structural hazard to the Raspberry Pi 5's 3.3V-tolerant GPIO architecture. This resistor layout forms a critical protection bridge to safely step down the incoming pulse wave, preventing processor burnout.
*   **Pin Configuration:**
    *   **pin1:** Connects directly to GPIO22, GPIO24, and the combined sensor ECHO lines.
    *   **pin2:** Connects straight to GPIO18.

### Artificial Vision

*   **Functionality:** High-speed vision modules that perform on-board image parsing to detect tracks and paths based on real-time visual signatures.

#### PixyCam 2.1 (Removed)
*   **Role:** Deployed on early prototype revisions for color signature tracking. 
*   **The Technical Failure:** It was decommissioned because ambient lighting variations and venue glare frequently caused tracking drops, forcing us to constantly manually tune color thresholds.
*   **For more information regarding removal, please refer to:** [2.2 Core Processing Shift](./02_HComponents.md).

#### Huskylens AI Camera (Current Primary Vision)
*   **Role:** Serves as our primary visual tracking engine. It processes complex color block tracking internally via its onboard neural network processor, letting the robot lock onto track markers reliably without bogging down the main computer.
*   **Why It Changed Everything:** Unlike the PixyCam, the Huskylens uses machine-learning algorithms to lock onto objects. By using its own onboard processor, it offloads the image tracking algorithms entirely, dropping main CPU consumption by more than 45% and slashing our vision tracking error rate below 3%.
*   **Pin Configuration:** Connected via the hardware I2C bus.
    *   **SDA:** Wire to GPIO2.
    *   **SCL:** Wire to GPIO3 (and pulled up to the 5V power line).
    *   **VCC:** Wire to the stable 3V3 power rail.
    *   **GND:** Wire to the common ground grid.
*   **For the main documentation on Computer Vision Functions and Error Rates:** [Performance Testing and Analytics documentation](./PTesting&Analysis.md).

### Steering Servo Motor

*   **Functionality:** A high-torque actuator used for precise closed-loop angular management of our steering mechanics.
*   **Role:** Drives our custom 3D-printed Ackermann steering linkage, calculating real-time wheel angle differentials to keep the front tires from scrubbing off speed during high-velocity turns.
*   **Evolutionary Jump:** Moving from the low-frequency LEGO smart hub environment to the Pi 5 completely wiped out steering signal lag. We now map the servo directly to the Pi 5's internal hardware PWM clock channel (`PWM0`), preventing data propagation delays and allowing precision handling at high velocities.
*   **Pin Configuration:**
    *   **input:** Wire to GPIO15 (Hardware PWM0 channel).
    *   **vcc:** Wire to the 5V power distribution rail.
    *   **gnd:** Wire to the common ground grid.

