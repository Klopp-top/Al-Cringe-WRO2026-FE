# Engineering Journal — Al-Cringe, WRO 2026 Future Engineers

This journal documents the design decisions, iterations, failures, and fixes made throughout the development of our robot. Entries are added as the project progresses.

---

## Entry 1 — Chassis: from hybrid LEGO differential to fully 3D-printed, and fixing chassis flex

**Context:**
Our first chassis version combined a 3D-printed frame with LEGO parts used for the differential. This let us prototype the drivetrain quickly without designing and printing a differential from scratch.

**Problem #1 — LEGO differential broke:**
During testing, one of the LEGO differential parts broke under load. Since it was a proprietary LEGO piece, it couldn't be repaired or easily substituted, only replaced. This pushed us to redesign the differential as a fully 3D-printed part instead of relying on a hybrid LEGO/3D-print solution — giving us full control over strength, tolerances, and the ability to iterate the design ourselves instead of being limited by an off-the-shelf part.

**Problem #2 — Front chassis plate flexed under load:**
Once we moved to the full 3D-printed chassis, we noticed the front section of the plate (holding the steering assembly) flexed noticeably under load — see photo below. The original design used a thin, narrow section between mounting points, which wasn't rigid enough to hold its shape once the steering mechanism and servo were mounted and under stress.

*(photo: `/mech/chassis_v1_flex.png` — top-down view of the plate showing the thin flexing section between the front and rear mounting blocks)*

**Fix:**
We added a reinforcement rib along the flexing section of the plate to increase stiffness without significantly increasing weight or print time. This resolved the flex and gave the steering assembly a stable, rigid mounting base.

**Steering mechanism:**
Steering is handled by a single servo connected to a standard rack-style steering linkage (no custom or exotic mechanism — a simple, reliable Ackermann-style link setup). Photo below shows the full assembly from underneath, including the servo, drive motor, differential, and wheel mounts.

*(photo: `/mech/chassis_bottom_view.png` — bottom view of assembled chassis: steering servo + linkage at front, drive motor + differential + gearing at rear, 4 wheels mounted)*

**Outcome:**
- Differential: fully 3D-printed, no dependency on LEGO parts
- Chassis: reinforced front section, no more flex under load
- Steering: servo + simple rack linkage, confirmed working

---

## Entry 2 — *(next entry goes here)*

*(date, what was done, what worked / didn't, what changed and why)*

---

### How to use this journal
Keep entries short: 3–6 sentences per problem, plus a photo where useful. Focus on **why** a decision was made, not just what the robot looks like now — that's what earns points in the "Systems Thinking" and "Mechanical Design" rubric criteria.
