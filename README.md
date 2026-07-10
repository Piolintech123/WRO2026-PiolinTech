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


## Introduction  
**Piolín** is an advanced autonomous robotic vehicle engineered to compete in the **WRO Future Engineers 2026** category. Running on a high-performance **Raspberry Pi 5** processing core, the robot handles real-time edge computer vision via a **Huskylens** smart camera for strict lane alignment. It fuses visual telemetry with an array of **three ultrasonic distance sensors** to navigate complex track curves, identify lane markers, and safely execute dynamic obstacle evasion.

---


### Team Members

| Member | Information | Contact |
| :---: | :--- | :---: |
| <img src="https://github.com/user-attachments/assets/b11aaff5-3de3-4762-b0c1-a0094b9cf4e7" width="250" height="370" style="border-radius: 50%; object-fit: cover;"> | **Sebastián Martínez**<br>Colegio Bilingüe de Panamá | [📸 Instagram](https://www.instagram.com/sebastian.mvrl/) |
| <img src="https://github.com/user-attachments/assets/dc507a8b-f1c1-435e-9df6-96d2e11e0cba" width="250" height="370" style="border-radius: 50%; object-fit: cover;"> | **Mia Cantoral**<br>Colegio Bilingüe de Panamá | [📸 Instagram](https://www.instagram.com/miaacnt) |
| <img src="https://github.com/user-attachments/assets/0b5f11e1-4c58-45dd-b63a-45f6c2c5726a" width="250" height="370" style="border-radius: 50%; object-fit: cover;"> | **Christian Castrellón**<br>Colegio Bilingüe de Panamá | [📸 Instagram](https://www.instagram.com/cj.chriss) |
| **Coach** | **Hanna Figueroa**<br>Thank you teacher Hanna for being our brightest and biggest inspiration out there. We truly admire and love you! :) | |

---

## 📂 Repository Directory Tree
```text  
├── docs/                           # Compulsory WRO Engineering Journal
│   ├── mobility_and_mechanical.md
│   ├── power_and_sensors.md
│   ├── software_and_obstacle_strategy.md
│   └── systems_thinking.md
├── meet-the-team/                  # Team profiles and background documentation
├── src/                            # Production Software
│   ├── main.py                     # Master execution thread
│   ├── vision.py                   # Huskylens parsing module
│   ├── control.py                  # PID and FSM logic
│   └── tests/                      # Isolated sensor testing scripts
├── hardware/                       # Electro-Mechanical Assets
│   ├── cad/                        # Print-ready STL and step files
│   └── schematics/                 # KiCad project and wiring PDFs
└── README.md                       # Repository Entrypoint
