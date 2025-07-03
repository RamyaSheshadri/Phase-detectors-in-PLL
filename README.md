# 🔁 Phase Detectors in PLL

### 📌 Overview

This is a continuation of the previous project where we did a **comparative analysis of PLL in 45nm and 180nm technology**.

- We had built a **basic block diagram of Phase Locked Loops in Cadence Virtuoso**.
- One of the drawbacks we observed in that project was:
  - While **XOR gate** can be used as a Phase Detector for simplicity,
  - It **limits the phase detection range from 0–180 degrees**.
  - Beyond this phase difference, we observed that the **output was not accurate**.
  - Presence of dead zones.

---
Dead zones are regions of phase difference where the output of the Phase Detector doesn’t change (or gives no useful info) — meaning it can’t detect small changes in phase.
For phase difference near 0° or 180°, the XOR output becomes flat — giving very little/no change in output.
### ⚙️ Improvement in This Project

To overcome the above drawback, we proposed:

#### 1. TSPC-Based Phase Detector
- TSPC helps **increase the phase detection range up to 360 degrees**.
- It overcomes the limitation of XOR and gives **more accurate output over the full range**.
- Minimizes the dead zones.

#### 2. Novel MUX-Based Phase Detector
- We came up with a **new idea** of using a **MUX-based Phase Detector**.
- This design **incorporates the best of both XOR and TSPC**, hence optimizing the circuit.
- Selection between the two is done using a **D Flip-Flop**.

---

### Working Logic

- A **D Flip-Flop** is used to generate a **select line (SEL)**.
- Based on the value of `SEL`, the corresponding Phase Detector is chosen:

| SEL | Phase Detector Selected | Implies...                        |
|-----|--------------------------|----------------------------------|
| 0   | XOR Gate                 | Phase difference is 0–180°       |
| 1   | TSPC Circuit             | Phase difference is 180–360°     |

---

###  Why This Works

-  **If SEL = 0**:
  - XOR gate is used.
  - This makes the circuitry **simpler**, as XOR logic is **basic** and easy to implement.

- **If SEL = 1**:
  - TSPC circuit is chosen.
  - This helps in **minimizing the dead zones** which were observed earlier.

---

### Summary

> This project enhances the original PLL design by addressing phase detection limitations using a hybrid XOR + TSPC approach, selected through a novel DFF-controlled MUX setup.

---

