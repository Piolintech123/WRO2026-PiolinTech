## The Evolution of PiolínTech

The development of the Piolín robot for the WRO 2026 Future Engineers category was not a linear path. It required a rigorous iterative engineering process that moved from a basic proof of concept prototype to a highly tuned autonomous vehicle ready for competition. This section documents our engineering journey in detail. We explore the mechanical, electrical, and software transformations across our primary development phases. By analyzing our past failures and our iterative solutions, we demonstrate the robustness and reliability of our final design.

---

### [Phase 1: The Initial Prototype](./models/evolution/Phase1.md)

Phase 1 represents our foundational attempt at solving the WRO track challenges. The primary goal during this early stage was simply to achieve basic mobility, verify sensor integration, and test initial lane tracking capabilities. However, this phase was heavily constrained by our reliance on off the shelf structural elements, legacy hardware, and rudimentary control loops.

#### Mechanical and Structural Baseline
During our initial mechanical drafting, we heavily utilized standard plastic building elements to rapidly prototype the chassis. While this approach allowed for quick assembly, we immediately encountered severe structural limitations. For instance, specific structural beams caused major issues. We used a standard technic piece that has exactly 3 holes (two horizontal and one vertical) for our early steering knuckles. This specific component suffered from immense torsional flex when subjected to lateral cornering forces. This flex resulted in unpredictable steering geometry, making reliable PID calibration nearly impossible. The center of gravity was also excessively high, causing the inner wheels to lift during high speed turns and destabilizing the entire platform.

#### Electrical and Processing Architecture
The first iteration relied on a fragmented processing approach. We were attempting to bridge legacy control bricks with newer microcontrollers, which led to significant communication bottlenecks and hardware limitations. 
*   **Motor Control:** We integrated the TB6612FNG motor driver to handle propulsion and steering. We mapped our logic to the `ain1` and `ain2` pins to ensure proper dual channel H-bridge operation for the main motors. However, our initial power delivery system lacked proper shielding and capacitors, leading to dangerous voltage spikes.
*   **Data Logging Limitations:** We attempted to log telemetry data using an EV3 control brick. This proved highly ineffective because the EV3 had no micro SD card connected at the time. Without expandable storage, our onboard data logging was severely restricted, making it incredibly difficult to analyze sensor performance after a test run.

#### Software and Logic Limitations
The software architecture in Phase 1 was strictly synchronous and relied on inferior peripheral components.
*   **Latency:** This blocking architecture meant that whenever the system processed a sensor input, the steering loop paused. This resulted in a stuttering movement profile where the robot would zig-zag down the straights instead of maintaining a smooth trajectory.
*   **Vision Failures:** We initially utilized a different camera module that lacked onboard processing. This generic camera sent raw video feeds directly to the main processor, overwhelming the system and causing frequent false positives due to ambient light changes.

#### Phase 1 Performance Metrics

| Metric | Target | Phase 1 Result | Status |
| :--- | :--- | :--- | :--- |
| **Top Speed** | 1.2 m/s | 0.6 m/s | ❌ Fail |
| **Steering Latency**| < 15 ms | 45 ms | ❌ Fail |
| **Chassis Flex** | < 2 mm | 5.0 mm | ❌ Fail |
| **Weight** | < 1.5 kg | 1.8 kg | ❌ Fail |

**Engineering Takeaway from Phase 1:** 
Standard building blocks and legacy hardware are insufficient for high velocity autonomous racing. We needed to design custom rigid components, upgrade our vision hardware, and completely overhaul our software to operate asynchronously.

<br>

---

### [Phase 2: The PiolínTech Overhaul](./models/evolution/Phase2.md)

Phase 2 marks the evolution from a basic prototype to a competition grade machine. We discarded the modular plastic framework and embraced custom fabrication, advanced embedded Linux processing, and AI accelerated computer vision.

#### Advanced Mechanical Redesign
To resolve the structural failures of Phase 1, we transitioned entirely to 3D modeling using Blender. This allowed us to design a bespoke chassis that maximized rigidity while minimizing weight.
*   **Custom Bevel Gears:** We 3D printed modified bevel gears to create a highly responsive Ackermann steering linkage. This eliminated the flex found in the older 3-hole structural pieces and provided a rigid direct steering response.
*   **Low Center of Gravity:** The battery and Raspberry Pi 5 were relocated to the absolute lowest deck of the chassis. This drastically improved cornering stability and eliminated wheel lift.

#### Processing Power and Sensor Upgrades
The electrical architecture was completely rebuilt around the Raspberry Pi 5 to handle the demands of the WRO track.
*   **Vision Acceleration:** We abandoned the older camera and integrated the HuskyLens AI camera via I2C. By offloading the color and centroid detection directly to the HuskyLens onboard FPGA, we freed up the Raspberry Pi 5 CPU for navigation tasks.
*   **Clean Power Delivery:** We implemented a split rail power system. This ensured the Raspberry Pi 5 received a perfectly clean 5V, while the TB6612FNG driver received unrestricted current directly from the LiPo battery via the `ain1` and `ain2` logic pathways.

#### Asynchronous Software Integration
The most significant leap in Phase 2 was the software overhaul. We rewrote the core logic using the Python `asyncio` library.
*   **Non-Blocking Loops:** The PID lane tracking algorithm now runs on a dedicated high frequency thread (100 Hz). This ensures the steering servo receives constant PWM updates regardless of what the vision system is processing.
*   **Deterministic Parking:** We implemented precise motor encoder tracking for the final lap. Instead of relying on inaccurate timers, the robot calculates its exact spatial distance from the start row to execute a flawless position based deceleration sequence.

#### Phase 2 Performance Metrics

| Metric | Target | Phase 2 Result | Status |
| :--- | :--- | :--- | :--- |
| **Top Speed** | 1.2 m/s | 1.35 m/s | ✅ Pass |
| **Steering Latency**| < 15 ms | 8 ms | ✅ Pass |
| **Chassis Flex** | < 2 mm | 0.5 mm | ✅ Pass |
| **Weight** | < 1.5 kg | 1.1 kg | ✅ Pass |

**Engineering Takeaway from Phase 2:** 
Custom 3D fabrication combined with asynchronous logic and FPGA accelerated vision creates a highly deterministic and reliable platform capable of handling the high speed requirements of the track.

<br>

---

### Visualizing the Evolution

The physical transformation of Piolín is best understood by comparing the structural layouts of our iterations. Below are the orthographic and perspective renders of the chassis evolution. These highlight the transition from bulky prototyping to sleek custom engineering.

#### [PiolínTech V1 (Visual)](./models/PTechV1.png)
*(Click the link above to view the high resolution file in the repository)*

![PiolínTech V1](/models/PTechV1.png)

**V1 Design Analysis:**
As seen in the V1 render, the chassis is characterized by a higher profile and a heavy reliance on standard grid plate mounting. The sensor array is mounted statically, and the wiring harness is exposed to the elements. The steering geometry in this visual relies on the older flexible linkages that caused our initial latency issues. The generic camera is mounted too high, causing perspective distortion. The overall footprint is bulky, resulting in a larger turning radius that struggled with the inner corners of the WRO track.

#### [PiolínTech V2 (Visual)](./models/PTechV2.png)
*(Click the link above to view the high resolution file in the repository)*

![PiolínTech V2](/models/PTechV2.png)

**V2 Design Analysis:**
The V2 visual demonstrates the massive leap in our engineering capabilities. 
1.  **Streamlined Chassis:** The main body is clearly lower and more compact. The custom 3D printed chassis components designed in Blender perfectly cradle the Raspberry Pi 5 and the battery, lowering the center of mass.
2.  **Integrated Vision:** The new HuskyLens AI module is mounted on a custom vibration dampened TPU bracket. It is angled precisely to capture both the track lines and upcoming obstacles without processing unnecessary background noise.
3.  **Optimized Drivetrain:** The front steering assembly clearly features our custom printed bevel gears. This provides a tighter turning radius and aggressive Ackermann geometry. The wiring for the dual ultrasonic sensors and the TB6612FNG driver is routed internally to prevent snagging during high speed maneuvers.

---

### Summary of Evolution
The journey from V1 to V2 encapsulates the core engineering ethos of the WRO Future Engineers category. By systematically identifying bottlenecks in our hardware flex, power distribution, and software latency, we successfully engineered a fully custom autonomous vehicle. The transition from off the shelf parts to Blender 3D modeled components, combined with the jump to HuskyLens vision and asynchronous Python programming, ensures PiolínTech is ready to compete at the highest level.
