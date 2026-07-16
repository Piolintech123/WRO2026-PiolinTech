# 10. Performance Testing and Analytics
---

## 10.1 Vision Subsystem Statistical Validation

To calculate the tracking error rates of our primary visual tracking engines under varying environmental conditions, we ran continuous benchmark tracking loops across 5,000 operational frames.

### 10.1.1 Error Rate Mathematical Formulation
The raw frame tracking error rate ($E_{\text{vision}}$) is calculated using the following formula:

$$E_{\text{vision}} = \left( \frac{F_{\text{dropped}} + F_{\text{false-positive}}}{F_{\text{total}}} \right) \times 100$$

Where:
*   $F_{\text{dropped}}$: Frames where a valid track line or target obstacle was present but the sensor failed to output coordinate bounding boxes.
*   $F_{\text{false-positive}}$: Frames where background glare or track artifacts caused false target detections.
*   $F_{\text{total}}$: Total frame count in the test run ($5,000$ frames).

  
### 10.1.2 Benchmark Data under Variable Conditions

The table below shows performance data across different testing environments, comparing our legacy configuration against our current machine-learning architecture:

| Environment Profile | Lux Level | Legacy PixyCam 2.1 Error Rate | Current Huskylens Error Rate |
| :--- | :--- | :--- | :--- |
| **Standard Ambient (Lab)** | 450 lx | 4.2% | 0.8% |
| **High Overhead Glare** | 1,200 lx | 14.8% | 2.1% |
| **Low-Contrast Shadowing** | 150 lx | 18.5% | 2.9% |
| **Composite Track Mean** | **-** | **12.5%** | **1.93%** |

---

## 10.2 Spatial Sensing and Ranging Reliability

The triple-staggered ultrasonic array was benchmarked against calibrated hardware physical baselines to evaluate noise distribution and response curves.


```

              [Left Sensor]       [Center Sensor]      [Right Sensor]
                 (GPIO15)            (GPIO17)             (GPIO23)
                    ╲                   │                    ╱
                     ╲                  │                   ╱


180° Coverage Sweep:    ▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀

```

### 10.2.1 Data Filtering and Noise Rejection
Raw distance pulses are subjected to an inline low-pass median filter running on a dedicated background execution thread. The filter discards any outlier data points ($D_n$) that deviate from the running array median ($M$) by a threshold variant:

$$\Delta D = |D_n - M| > 5\text{ cm}$$

Discarded data points are dropped from the navigation matrix to prevent sudden, erratic steering corrections.

### 10.2.2 Ranging Accuracy Profile

| True Physical Distance | Raw Sensor Mean Output | Filtered Output Mean | Standard Deviation ($\sigma$) |
| :--- | :--- | :--- | :--- |
| **5.0 cm** | 5.2 cm | 5.02 cm | 0.08 cm |
| **15.0 cm** | 15.6 cm | 15.04 cm | 0.12 cm |
| **30.0 cm** | 31.2 cm | 30.11 cm | 0.24 cm |
| **50.0 cm** | 52.8 cm | 50.29 cm | 0.45 cm |

---

## 10.3 Kinematic Control Loop Analysis (PID)

Steering actuation adjustments are managed via a dedicated proportional-integral-derivative (PID) tracking loop that targets the visual center line ($X_{\text{target}} = 160$) provided by the Huskylens I2C packet stream.

### 10.3.1 Control Equation

$$u(t) = K_p e(t) + K_i \int_{0}^{t} e(\tau) d\tau + K_d \frac{de(t)}{dt}$$

Where the tracking error is defined as:

$$e(t) = X_{\text{target}} - X_{\text{measured}}$$

### 10.3.2 Loop Optimization Impact
*   **Hardware PWM Channel Allocation:** By migrating the steering servo signal out of software timing maps and anchoring it to the Pi 5's internal `PWM0` hardware clock channel (GPIO 15), data propagation latency dropped from $18\text{ ms}$ to less than $1.2\text{ ms}$.
*   **Transient Response Performance:** The hardware clock assignment completely decoupled steering updates from heavy multi-threaded CPU overhead. This modification keeps our steering loop exceptionally stable, suppressing transient overshoot and damping oscillation cycles down to zero within $220\text{ ms}$ during aggressive lane recovery maneuvers.

---
