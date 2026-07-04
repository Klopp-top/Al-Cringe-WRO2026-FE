# Component Justification — Al-Cringe, WRO 2026 Future Engineers

This document explains **why** each component was chosen, what alternatives were considered, and what trade-offs were made. This directly supports the "Mobility & Mechanical Design" and "Power & Sensor Architecture" evaluation criteria — judges want to see reasoning, not just a parts list.

---

## Main Controller — Raspberry Pi 5 8GB

**Why:**
- Enough CPU power to run the OpenCV vision pipeline (frame capture, HSV masking, contour detection, PD control) in real time, without offloading vision to an external accelerator
- 8GB RAM gives comfortable headroom for frame buffers and running the control loop without swapping
- Native CSI camera interface (no USB overhead/latency like a webcam would add)
- Large community and documentation base — faster debugging, easier to find working examples for GPIO/serial issues

**Alternatives considered:** *(fill in — e.g. Raspberry Pi 4, Jetson Nano, laptop + separate MCU)*
**Trade-off accepted:** *(e.g. higher power draw / cost than a smaller SBC, but justified by vision processing needs)*

---

## Camera — Raspberry Pi Camera v3

**Why:**
- Wide field of view helps the robot see more of the drivable corridor and detect obstacles earlier, giving the PD controller more reaction time
- Native CSI ribbon connection to the Pi — lower latency and simpler wiring than USB cameras
- Autofocus and good low-light performance compared to older Pi camera modules

**Alternatives considered:** *(e.g. USB webcam, Pi Camera v2, wide-angle third-party module)*
**Trade-off accepted:** *(e.g. lens distortion at the edges from the wide FOV — may need correction in software)*

---

## Microcontroller — Arduino Nano

**Why:**
- Splits responsibilities: the Raspberry Pi handles vision and decision-making, while the Nano handles time-critical, low-level tasks (servo PWM, motor control, polling 3 ultrasonic sensors) — this keeps the vision loop from being interrupted by hardware timing
- Small form factor, easy to mount inside the 3D-printed chassis
- Simple to program and debug independently from the vision code (can bench-test motor/servo/sensor logic without the Pi running)
- Cheap and easy to replace if damaged during testing

**Alternatives considered:** *(e.g. doing everything on the Pi via GPIO directly, using an ESP32 instead)*
**Trade-off accepted:** *(added serial communication complexity between Pi and Nano, in exchange for more reliable real-time control)*

---

## Steering — Surpas 25g Servo

**Why:**
- Lightweight (25g), keeps the front of the chassis light — important since the front section already needed a reinforcement rib to handle load (see Engineering Journal, Entry 1)
- Sufficient torque for the steering linkage load at this vehicle weight and speed
- Standard servo horn/mounting size — fits directly into our 3D-printed steering mount design

**Alternatives considered:** *(e.g. MG996R or other higher-torque/heavier servos)*
**Trade-off accepted:** *(lower torque ceiling than a heavier servo — acceptable since our steering geometry doesn't need high force, just precision)*

---

## Drive Motor — DC Gear Motor, 915 RPM (no encoder, regional stage)

**Why:**
- 915 RPM (combined with our wheel diameter and gear ratio) gives us a top speed appropriate for the track size and turn radius, without being so fast that the vision/control loop can't react in time
- Open-loop control (no encoder) was chosen for the regional stage to reduce complexity and cost while we finish validating the rest of the system — motor speed is set directly via PWM duty cycle
- Affordable and available quickly, which mattered given our component budget was self-funded and built up gradually

**Alternatives considered:** *(e.g. different RPM/gear ratio, stepper motor, higher-cost motor with built-in encoder)*
**Trade-off accepted:** *(no closed-loop speed feedback yet — speed can drift with battery voltage drop over a run; encoder is planned for the next stage to fix this)*

---

## Distance Sensors — Ultrasonic × 3

**Why:**
- Cheap and reliable at the short ranges relevant to wall/obstacle proximity on the track
- Provide a fast, independent (non-vision) distance signal the Arduino Nano can act on immediately — useful as a backup/cross-check to the camera-based obstacle detection, and for crash-avoidance if vision misses something
- Three sensors let us cover front + left/right side distances for wall-following and turn timing

**Alternatives considered:** *(e.g. IR distance sensors, LiDAR, relying on vision alone)*
**Trade-off accepted:** *(ultrasonic sensors can have blind spots/interference at certain angles — mitigated by combining with vision data rather than relying on ultrasonic alone)*

---

## Battery — Li-ion/LiPo 2S 1300mAh

**Why:**
- 2S (7.4V nominal) matches the voltage needs of our drive motor and steering servo through the regulation stage, without needing a bulkier 3S pack
- 1300mAh capacity balances run-time for multiple test/competition rounds against keeping overall vehicle weight low (heavier battery = more load on motor and steering)

**Alternatives considered:** *(e.g. 3S pack, higher-capacity 2S pack)*
**Trade-off accepted:** *(shorter run-time than a higher-capacity pack — acceptable since rounds are short and we can recharge/swap between rounds)*

---

## Chassis — Fully 3D-Printed (designed in Fusion 360)

**Why:**
- Full design control over mounting points, weight distribution, and dimensions — no compromises from an off-the-shelf chassis
- Iteration is fast and cheap: when the LEGO differential broke and the front plate flexed (see Engineering Journal, Entry 1), we could redesign and reprint within a day instead of waiting on new purchased parts
- Lets the whole design be reproducible from the shared STL/Fusion files, supporting the GitHub reproducibility criterion

**Alternatives considered:** *(e.g. hybrid chassis with LEGO/off-the-shelf parts — this was our original approach, see Journal Entry 1 for why we moved away from it)*
**Trade-off accepted:** *(more iteration time spent on mechanical design/printing vs. buying a ready-made chassis, but this is exactly the kind of engineering process WRO evaluates)*

---
