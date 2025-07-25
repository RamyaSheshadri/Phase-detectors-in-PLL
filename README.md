# Phase Detectors in PLL

### Overview

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
### Improvement in This Project

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

## Basic working of a 2x1 Multiplexer:

###  Basic Working of 2×1 Multiplexer (MUX)

A **2×1 MUX** (2-input, 1-output multiplexer) is a digital circuit that **selects one of two input signals** based on the value of a **single select line (SEL)**.

#### Inputs:
- **IN0** → First input
- **IN1** → Second input
- **SEL** → Select signal

####  Output:
- **OUT** → Output based on SEL

####  Logic Table:

| SEL | OUT  |
|-----|------|
| 0   | IN0  |
| 1   | IN1  |

- When **SEL = 0**, the MUX forwards **IN0** to the output.
- When **SEL = 1**, the MUX forwards **IN1** to the output.

####  In Our Design:
- **IN0 = XOR-based Phase Detector output**
- **IN1 = TSPC-based Phase Detector output**
- **SEL is generated by a D Flip-Flop**
- Based on `SEL`, the MUX dynamically selects the appropriate phase detector output.

This helps in **switching between XOR and TSPC detectors**, depending on the phase range, to **optimize accuracy and reduce dead zones**.


### Schematic:
![mux 2x1 pt 2](https://github.com/user-attachments/assets/6bf1143a-d299-411a-b896-6dc8913fce89)

### Block diagram:
![mux 2x1 pt 3](https://github.com/user-attachments/assets/3c976abd-26f0-4070-ba52-bb82f65fbbbc)

### Output waveform:
![mux 2x1 pt1](https://github.com/user-attachments/assets/76c6af76-d195-421b-ac01-94c722d25227)



## TSPC circuit:

### Basic Working of TSPC Phase Detector

TSPC stands for **True Single Phase Clocking** — a design style commonly used in **high-speed digital circuits**, especially in **phase detectors** and **flip-flops**.

#### Key Features:
- Uses **only one clock signal** (no clock inversion or multiple phases)
- Works at **high frequencies** with **low power consumption**
- Eliminates race conditions seen in traditional dynamic logic

---

###  Working Principle:

- TSPC logic typically uses **dynamic logic** with **precharge and evaluate phases** controlled by a **single clock**.
- The circuit consists of **NMOS and PMOS transistors** arranged such that:
  - When **clock is low** → Precharge phase (output node gets precharged)
  - When **clock is high** → Evaluate phase (circuit evaluates input logic and updates output)

---

###  In Our Project:

- We used a **TSPC-based Phase Detector** to detect phase differences between two input signals.
- Unlike XOR, it can detect **phase differences from 0° to 360°**.
- The dynamic logic in TSPC ensures **fast switching** and **accurate edge detection**.

---

###  Why TSPC is Better Here:

- **No dead zones** → Full 0–360° phase detection range
- **High speed** → Ideal for faster PLL locking
- **Less clock complexity** → Only one phase of clock needed
- Helps maintain **low power** and **compact layout**

### Schematic:
![tspc pt 2](https://github.com/user-attachments/assets/4d406c15-cc9e-4795-91df-93780d551521)

### Block diagram:
![tspc pt 3](https://github.com/user-attachments/assets/5e4efa03-c861-420f-8c97-d0b4487384e6)

### Output waveform:
![tspc pt 1](https://github.com/user-attachments/assets/248a6115-2f40-41c6-8783-0bc6ef2f0ecf)

### At 0 degree delay:
![tspc 0 deg delay](https://github.com/user-attachments/assets/85ee34a9-6423-496c-921f-568993cba77f)

### At 270 degree delay:
![tspc 270  ps](https://github.com/user-attachments/assets/24e3d61a-dc1a-455b-9f07-b61cd0c02494)


### Why Not Just Use TSPC Alone?

While **TSPC Phase Detectors** are capable of detecting the full **0°–360°** phase range, we chose **not to use TSPC alone** for the following reasons:

####  Reasons Against Using Only TSPC:
- For **0°–180° phase difference**, the XOR gate works **perfectly fine**.
- TSPC, although powerful, involves:
  - More **transistor count**
  - Slightly **higher complexity**
  - Increased **power and area overhead** if used unnecessarily

---

###  Why the Hybrid Approach is Better:

- By **using XOR for 0°–180°**, we:
  - Keep the circuit **simple**
  - Save **power** and **silicon area**
  - Leverage XOR’s **clean, glitch-free output**

- For **180°–360°**, where XOR fails, we switch to **TSPC** for its **accuracy and wider range**.

####  This selective usage gives us the **best of both worlds**:
| Range        | Phase Detector | Reason                   |
|--------------|----------------|--------------------------|
| 0° – 180°    | XOR            | Simple, accurate, light  |
| 180° – 360°  | TSPC           | Required for full range  |

---

###  Final Thought:

> We designed this Hybrid PD **intentionally**, not just for novelty — but to **balance simplicity and accuracy** based on the operating condition. This makes our design both **efficient** and **smart** 



## Hybrid Phase Detector:
###  Hybrid Phase Detector Architecture (Our Novel Approach)

To overcome the limitations of standalone Phase Detectors, we designed a **Hybrid Phase Detector** that combines:

-  **XOR-based Phase Detector** → Accurate and simple for 0°–180° range
-  **TSPC-based Phase Detector** → Robust and linear for 180°–360° range
-  **2×1 Multiplexer (MUX)** → Selects between XOR and TSPC outputs
-  **D Flip-Flop (DFF)** → Controls the `SEL` line for MUX selection

---

### Working Concept:

- A **D Flip-Flop** generates the `SEL` signal based on the system logic or operating region.
- This `SEL` signal decides which Phase Detector is active:

| SEL | Selected PD | Phase Range Detected |
|-----|-------------|----------------------|
| 0   | XOR         | 0° to 180°           |
| 1   | TSPC        | 180° to 360°         |

- The **MUX** acts as a switch, forwarding the output of the selected Phase Detector to the rest of the PLL system.

---

### Benefits of Hybrid Architecture:

-  **Combines simplicity and accuracy** in one design
-  **Eliminates dead zones** present in XOR-only designs
-  **Improves phase detection over full 0–360° range**
-  **Dynamic selection** ensures optimal performance based on phase condition
-  **Novel idea** not seen in standard PLL implementations

---

### Summary:

> This hybrid Phase Detector intelligently chooses between two different architectures to achieve **complete and accurate phase detection**, making the PLL more efficient, robust, and lock faster across all conditions.


### Schematic:
![hybrid schematic](https://github.com/user-attachments/assets/98a17f24-3e14-418c-9196-a1f827cf5a68)

### Output waveform when SEL=0:
![op of sel=0](https://github.com/user-attachments/assets/5b523d0c-717f-475e-b656-221c79d75a07)

### Output waveform when SEL=1:
![op of sel=1](https://github.com/user-attachments/assets/19814921-b32c-4f37-b74f-0c312615f65f)

### Why is Hybrid PD Power < TSPC Alone?

At first glance, the Hybrid Phase Detector seems more complex than TSPC alone. However, it actually consumes **less power overall**, and here’s why:

---

#### 1. TSPC Runs Continuously in Standalone Mode
- In a pure TSPC-based Phase Detector, the circuit is **always active**, runs 100% of the time in a standalone design
- Dynamic nodes **switch every clock cycle**, even when phase difference is within 0–180°, where XOR would be sufficient.
- This leads to **unnecessary dynamic power consumption**.

---

#### 2. Hybrid Activates Only the Required Block
- In the Hybrid design, the **MUX selects XOR or TSPC** based on phase conditions.
- For **0°–180°**, only the **XOR block is active**, and **TSPC remains idle**.
- This **conditional activation** saves power significantly.

---

#### 3. MUX and DFF Overhead is Minimal
- The **MUX** and **D Flip-Flop** used for selection logic:
  - Have **very low switching activity**
  - Consume **negligible power** compared to full TSPC circuitry
- Hence, the extra logic does **not significantly increase total power**.

---

### Conclusion:
> Although the Hybrid PD includes both XOR and TSPC blocks, it is **power-optimized** by design. It ensures that **only one block is active at a time**, avoiding unnecessary switching and reducing total power consumption compared to TSPC-only implementation.


### Conclusion

In this project, we successfully improved the phase detection mechanism in PLLs by:

- Identifying the **limitations of XOR-based detectors** (dead zones beyond 180°)
- Introducing a **TSPC-based Phase Detector** to cover the full 0°–360° range
- Proposing a **novel Hybrid Architecture** that uses:
  - XOR for 0°–180° (simplicity)
  - TSPC for 180°–360° (accuracy)
  - D Flip-Flop + MUX to dynamically switch between them

---

### What Makes Our Work Stand Out

-  **Combines accuracy with efficiency**
-  **Minimizes dead zones**
-  **Reduces unnecessary complexity**
-  **Original hybrid idea** — not commonly found in literature

---

### Outcome

The final design is:
- More **robust** than XOR-only designs
- More **efficient** than TSPC-only designs
- **Scalable** for use in high-speed or low-power PLL systems

> Overall, our hybrid phase detector offers an optimized solution with improved range, accuracy, and design efficiency — making it a strong candidate for real-world PLL applications.

> This project enhances the original PLL design by addressing phase detection limitations using a hybrid XOR + TSPC approach, selected through a novel DFF-controlled MUX setup.

---

