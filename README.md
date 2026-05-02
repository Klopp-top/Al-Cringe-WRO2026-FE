# 🤖 Al-Cringe — WRO 2026 Future Engineers

> **Team Al-Cringe** from 🇺🇿 **Uzbekistan**  
> WRO 2026 Season — Future Engineers Challenge

---

## 📹 Summary Video

> 🎬 *Coming soon — will be added before competition*

---

## 👥 The Team

| Photo | Name | Age | Role |
|-------|------|-----|------|
| *(photo)* | **Нарзиев Акбаршох** | 16 y.o. — Grade 10 | Hardware & Mechanical |
| *(photo)* | **Хафизов Шохрухмирзо** | 17 y.o. — Grade 11 | Software & Computer Vision |
| *(photo)* | **Назаров Асадбек** | 17 y.o. — Grade 11 | Electronics & Integration |
| *(photo)* | **Эргашев Навруз** | — | Mentor |

We are a team of three high school students from Uzbekistan, competing in WRO Future Engineers for the 2026 season. Our goal: **1st place at the World Final.**

---

## 🗂️ Repository Structure

```
/code        → Python source code (vision, motors, main logic)
/elec        → Electrical schematics and wiring diagrams
/mech        → CAD files, STL files for 3D-printed parts
/t-photos    → Official team photos (required by WRO rules)
/v-photos    → Robot photos from all 6 sides (required by WRO rules)
README.md    → This file
```

---

## 🤖 About the Robot

Our robot uses an **Ackermann steering geometry** with **rear-wheel drive**.  
Vision and decision-making run entirely on the **Raspberry Pi 5**, with no intermediate microcontroller — this reduces latency compared to Pi + Arduino architectures.

**Key design decisions:**
- Single-board architecture: RPi 5 handles both CV pipeline and motor control directly via GPIO
- Wide-angle camera (120° FOV) for maximum field of view during obstacle detection
- NEMA 17 stepper motor for precise, repeatable speed control without encoder tuning
- DRV8825 driver with 1/32 microstepping for smooth, quiet motion

---

## 🔧 Hardware Components

| Component | Model | Status | Purpose |
|-----------|-------|--------|---------|
| Main Controller | Raspberry Pi 5 8GB | ✅ Confirmed | Vision + logic + motor control |
| Camera | Pi Camera v3 Wide (120°) | ✅ Confirmed | Computer vision |
| Drive Motor | NEMA 17 Stepper (42HS40) | ✅ Confirmed | Rear-wheel drive |
| Motor Driver | DRV8825 | ✅ Confirmed | Stepper control (1/32 step) |
| Steering | Servo MG996R | ✅ Confirmed | Ackermann steering |
| IMU | MPU-6050 | 🔄 In consideration | Gyroscope + accelerometer |
| Distance Sensors | HC-SR04 × 2 | 🔄 In consideration | Lateral wall detection |
| Battery | Li-Po 3S 11.1V 2200mAh | 🔄 In consideration | Main power |
| DC-DC Converter | LM2596 5V 5A | 🔄 In consideration | Power regulation for RPi |

---

## 💻 Software Architecture

**Language:** Python 3.11  
**Computer Vision:** OpenCV  
**Motor Control:** RPi.GPIO (direct GPIO, no Arduino)

```
main.py
├── camera_manager.py       → frame capture, preprocessing
├── image_algorithms.py     → wall following, obstacle detection, arbitration
├── motor_controller.py     → NEMA 17 via DRV8825 (STEP/DIR on GPIO)
├── servo_controller.py     → steering servo via PCA9685 (I2C)
└── utils/
    └── image_transform.py  → HSV masking, blob detection, line detection
```

**Vision pipeline:**
1. Capture frame from Pi Camera v3 Wide
2. Generate polygon mask (drivable corridor)
3. Detect colored obstacles (HSV color segmentation)
4. Wall-following PD controller
5. Obstacle avoidance arbitration
6. Output servo angle + motor speed

---

## 🚗 Mobility Management

- **Drive:** NEMA 17 stepper motor — rear axle, controlled via DRV8825 in 1/32 microstepping mode
- **Steering:** Servo motor — Ackermann geometry, 3D-printed linkage
- **Structure:** 3D-printed multi-layer chassis (base / middle / top)

> *Detailed torque calculations, CAD files, and assembly photos — see [/mech/README_mech.md](mech/README_mech.md)*

---

## ⚡ Power & Electronics

- **Brain:** Raspberry Pi 5 8GB — Python, OpenCV, direct GPIO control
- **Camera:** Pi Camera v3 Wide — CSI interface, no USB latency
- **Motor driver:** DRV8825 — STEP/DIR signals from RPi GPIO
- **Servo driver:** PCA9685 — I2C PWM controller
- **Power:** Li-Po 3S → DC-DC converter → 5V rail for RPi

> *Wiring diagrams and power calculations — see [/elec/README_elec.md](elec/README_elec.md)*

---

## 🚧 Obstacle Management

> 🔄 *Algorithm details will be documented here as development progresses*

**Planned approach:**
- Wall following: PD controller based on pixel analysis of drivable corridor
- Obstacle detection: HSV color segmentation (red/green blocks)
- Arbitration: obstacle avoidance takes priority over wall following when block is detected
- Crash detection: emergency evasive turn on frontal wall proximity

---

## 🏁 Performance Videos

### Open Challenge
> 🎬 *Coming soon*

### Obstacle Challenge
> 🎬 *Coming soon*

---

## 📓 Engineering Journal

All design decisions, iterations, and test results are documented in the engineering journal.

👉 [View Engineering Journal](docs/engineering-journal.md)

| Date | Entry |
|------|-------|
| 2026-05-02 | Initial hardware stack selection — RPi 5, NEMA 17, Pi Camera v3 Wide, DRV8825 |
| *(ongoing)* | *(updated each session)* |

---

## 🔮 Planned Improvements

- [ ] Replace breadboard with custom PCB for cleaner wiring
- [ ] Add lateral HC-SR04 sensors for improved wall detection and parking
- [ ] Implement dynamic HSV color ranges to handle varying lighting conditions
- [ ] Add MPU-6050 gyroscope for precise 90° and 180° turns
- [ ] 3D-print dedicated mount for Raspberry Pi (currently taped)

---

## 📜 License

MIT License — see [LICENSE](LICENSE) for details.

---

*Built with 🔥 in Uzbekistan — Al-Cringe, WRO 2026*
