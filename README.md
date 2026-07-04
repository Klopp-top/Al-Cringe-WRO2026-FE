# 🤖 Al-Cringe — WRO 2026 Future Engineers

> **Team Al-Cringe** from 🇺🇿 **Uzbekistan**
> WRO 2026 Season — Future Engineers Challenge

---

## 📹 Summary Video

> 🎬 *Coming soon — will be added before competition*

---

## 👥 The Team

| Photo | Name |
|-------|------|
| *(photo)* | **Тешабаев Махкамбек** |
| *(photo)* | **Нарзиев Акбаршох** |
| *(photo)* | **Содиков Шукрулло** |

We are a team of three high school students from Uzbekistan, competing in WRO Future Engineers for the 2026 season. Our goal: **1st place at the World Final.**

---

## 🗂️ Repository Structure

```
/code        → Source code (Raspberry Pi vision/logic + Arduino Nano firmware)
/elec        → Electrical schematics and wiring diagrams
/mech        → Fusion 360 CAD files, STL files for 3D-printed parts
/t-photos    → Official team photos (required by WRO rules)
/v-photos    → Robot photos from all 6 sides (required by WRO rules)
README.md    → This file
```

---

## 🤖 About the Robot

Our robot uses an **Ackermann steering geometry** with **rear-wheel drive**. The chassis is fully **3D-printed**, designed from scratch in **Fusion 360**.

The architecture splits responsibilities across two boards: the **Raspberry Pi 5** handles computer vision and high-level decision-making, while an **Arduino Nano** takes care of low-level, time-critical control — driving the steering servo and drive motor, and continuously polling the three ultrasonic distance sensors. Sensor readings and motor/servo commands are exchanged between the two boards over a serial (UART/USB) link.

**Key design decisions:**
- Two-board architecture: Raspberry Pi 5 for vision and strategy, Arduino Nano for real-time actuator control and sensor polling — keeps the vision loop on the Pi free from timing-sensitive PWM/sensor work
- Wide field-of-view Pi Camera v3 for obstacle detection
- Single steering servo (Surpas 25g) driving the Ackermann linkage
- Drive motor for the regional stage runs open-loop at 915 RPM (no encoder); a version with encoder feedback is planned for the next stage
- Fully custom 3D-printed chassis, modeled entirely in Fusion 360

---

## 🔧 Hardware Components

| Component | Model | Status | Purpose |
|-----------|-------|--------|---------|
| Main Controller | Raspberry Pi 5 8GB | ✅ Confirmed | Vision + high-level logic |
| Camera | Raspberry Pi Camera v3 | ✅ Confirmed | Computer vision |
| Microcontroller | Arduino Nano | ✅ Confirmed | Servo & motor control, ultrasonic sensor monitoring |
| Steering | Servo Surpas 25g | ✅ Confirmed | Ackermann steering |
| Drive Motor | DC gear motor, 915 RPM (no encoder) | ✅ Confirmed for regional stage | Rear-wheel drive |
| Distance Sensors | Ultrasonic × 3 | ✅ Confirmed | Obstacle & wall detection |
| Battery | Li-ion/LiPo 2S 1300mAh | ✅ Confirmed | Main power |
| Chassis | 3D-printed (custom, Fusion 360) | ✅ Confirmed | Structure |
| Encoder (drive motor) | — | 🔄 Planned for next stage | Closed-loop speed control |

---

## 💻 Software Architecture

**Raspberry Pi 5** — vision and strategy layer
**Arduino Nano** — real-time actuator/sensor layer

```
raspberry_pi/
├── main.py                 → high-level control loop, decision-making
├── camera_manager.py       → frame capture, preprocessing
├── image_algorithms.py     → wall following, obstacle detection, arbitration
├── serial_link.py          → communication with Arduino Nano
└── utils/
    └── image_transform.py  → HSV masking, blob detection, line detection

arduino_nano/
└── main.ino                → servo control, motor control (915 RPM, open-loop),
                               reads 3× ultrasonic sensors, reports over serial
```

**Vision pipeline (Raspberry Pi 5):**
1. Capture frame from Pi Camera v3
2. Generate polygon mask (drivable corridor)
3. Detect colored obstacles (HSV color segmentation)
4. Wall-following PD controller
5. Obstacle avoidance arbitration
6. Send target servo angle + motor command to Arduino Nano over serial

**Control loop (Arduino Nano):**
1. Poll 3 ultrasonic sensors continuously
2. Report distances to Raspberry Pi 5
3. Receive servo angle / motor speed commands from Raspberry Pi 5
4. Drive steering servo and drive motor accordingly

---

## 🚗 Mobility Management

- **Drive:** DC gear motor, rear axle, 915 RPM, open-loop for the regional stage (no encoder yet)
- **Steering:** Surpas 25g servo — Ackermann geometry, 3D-printed linkage
- **Structure:** Fully 3D-printed chassis, designed in Fusion 360

> *Detailed CAD files and assembly photos — see [/mech/README_mech.md](mech/README_mech.md)*

---

## ⚡ Power & Electronics

- **Vision & logic:** Raspberry Pi 5 8GB + Pi Camera v3
- **Real-time control:** Arduino Nano — drives steering servo and drive motor, reads 3× ultrasonic sensors
- **Power:** Li-ion/LiPo 2S 1300mAh battery

> *Wiring diagrams and power calculations — see [/elec/README_elec.md](elec/README_elec.md)*

---

## 🚧 Obstacle Management

**Planned approach:**
- Wall following: PD controller based on pixel analysis of the drivable corridor
- Obstacle detection: HSV color segmentation (red/green blocks)
- Distance confirmation: 3 ultrasonic sensors, read by the Arduino Nano, cross-checked with vision data
- Arbitration: obstacle avoidance takes priority over wall following when a block is detected
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
| *(early season)* | Team formed; hardware budget planned and gradually purchased (self-funded, no government or sponsor support) |
| *(later than initially planned)* | Development start delayed while saving for components |
| *(ongoing)* | Hardware stack finalized — RPi 5 8GB, Pi Camera v3, Arduino Nano, Surpas 25g servo, 915 RPM drive motor, 3× ultrasonic sensors, 2S 1300mAh battery |
| *(ongoing)* | Chassis modeled and 3D-printed from a custom design in Fusion 360 |
| *(ongoing)* | *(updated each session)* |

---

## 🔮 Planned Improvements

- [ ] Add encoder to the drive motor for closed-loop speed control
- [ ] Custom PCB to replace current wiring
- [ ] Implement dynamic HSV color ranges to handle varying lighting conditions
- [ ] Fine-tune ultrasonic sensor placement for improved wall/obstacle detection
- [ ] Refine 3D-printed chassis based on regional-stage test results

---

## 📜 License

MIT License — see [LICENSE](LICENSE) for details.

---

*Built with 🔥 in Uzbekistan — Al-Cringe, WRO 2026*
