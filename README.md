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

![op of sel=1](https://github.com/user-attachments/assets/febdaf94-d816-49b8-a137-94cc9be8de8c)
![op of sel=0](https://github.com/user-attachments/assets/cd3fa077-2e4e-425f-8260-d5357f72797a)
![mux 2x1 pt1](https://github.com/user-attachments/assets/73fbf36a-34e2-4846-997a-2bb7586559e4)
![mux 2x1 pt 3](https://github.com/user-attachments/assets/6ba313fc-f0d4-4590-b23c-df9b403ee8ec)
![mux 2x1 pt 2](https://github.com/user-attachments/assets/0b431e2f-f661-4719-a65c-be5cabd998f3)
![inverter schematic](https://github.com/user-attachments/assets/6429df19-0a0e-490a-89db-2c60fb975061)
![hybrid schematic](https://github.com/user-attachments/assets/6d9764ab-af2b-4856-a2b0-1de0be89981a)
![tspc pt 3](https://github.com/user-attachments/assets/331ba1f6-a95a-4895-89a8-915969da4d07)
![tspc pt 2](https://github.com/user-attachments/assets/849b69eb-566f-42fd-a477-df4509edee05)
![tspc pt 1](https://github.com/user-attachments/assets/55309726-44f4-477e-a94d-cef1f3248df3)
![tspc 270  ps](https://github.com/user-attachments/assets/0c2cb510-bb17-42c3-a5a3-e7ecc5cc12b5)
![tspc 0 deg delay](https://github.com/user-attachments/assets/677863c9-713c-4a98-b33f-186096cde7cc)
![Screenshot from 2025-05-21 13-31-01](https://github.com/user-attachments/assets/42525429-54d5-4372-9dea-2e583f4d8555)


### Summary

> This project enhances the original PLL design by addressing phase detection limitations using a hybrid XOR + TSPC approach, selected through a novel DFF-controlled MUX setup.

---

