⚙️ Day 2 — Velocity Saturation & CMOS Inverter VTC (Code + Emojis)

💻 *NgspiceSky130 — CMOS Circuit Design & SPICE Simulation Journey*

---

📘 Introduction

On Day 2, we explore short-channel NMOS behavior, focusing on velocity saturation effects
using the Sky130 PDK. We also design and simulate a CMOS inverter and plot its
Voltage Transfer Characteristic (VTC).

---

💡 Understanding Velocity Saturation in MOSFETs

📊 | Aspect                | Observation |
| -------------------- | ----------- |
| Low electric field    | Drift velocity v_d ∝ E |
| High electric field   | v_d → v_sat |
| Impact on transistor  | Slower increase of Id, reduced gain, lower transconductance g_m |

---

🧮 Drain Current with Velocity Saturation

ID = (W/L) * μn * Cox * (VGS - VT) * VDSsat

VDSsat = (Esat * L) / (1 + (Esat * L)/(VGS - VT))

---

💻 SPICE Simulations

### 💻 NMOS Id vs Vgs

* NMOS Id vs Vgs Sweep
```spice
.param temp=27
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

XM1 Vdd n1 0 0 sky130_fd_pr__nfet_01v8 w=0.39 l=0.15
R1 n1 in 55

Vdd vdd 0 1.8V
Vin in 0 1.8V

.op
.dc Vin 0 1.8 0.1

.control
run
display
setplot dc1
.endc

.end
```

Simulation command:
```bash
ngspice day2_nfet_idvgs_L015_W039.spice
```

<p align="center">
  <img width="704" height="545" alt="image" src="https://github.com/user-attachments/assets/fc2e9e8a-0490-4a49-b19d-ae395df5f492" />
</p>


📊 | Parameter                  | Value / Description |
| -------------------------- | ----------------- |
| Threshold Voltage (Vth)    | ≈ 0.45 V |
| Sweep Range VGS            | 0–1.8 V |
| Step Size                  | 0.1 V |

---

### 💻 NMOS Id vs Vds

* NMOS Id vs Vds Sweep
```spice
.param temp=27
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

XM1 Vdd n1 0 0 sky130_fd_pr__nfet_01v8 w=0.39 l=0.15
R1 n1 in 55

Vdd vdd 0 1.8V
Vin in 0 1.8V

.op
.dc Vdd 0 1.8 0.1 Vin 0 1.8 0.2

.control
run
display
setplot dc1
.endc

.end
```

Simulation command:
```bash
ngspice day2_nfet_idvds_L015_W039.spice
```
<p align="center">
  <img width="704" height="545" alt="image" src="https://github.com/user-attachments/assets/4a6a39d6-b2db-4def-b87f-61c40b0a2d63" />
</p>


📈 | Aspect                      | Observation |
| --------------------------- | ----------- |
| Linear Region               | Small VDS, Id ∝ VDS |
| Saturation Region           | Id plateaus at high VDS |
| Nested Sweep                | Outer: VGS 0–1.8 V, step 0.2 V; Inner: VDS 0–1.8 V, step 0.1 V |
| Velocity Saturation Effect  | Limits drain current, reduces gain |
| Short-Channel Effect        | Threshold shift, reduced transconductance |

---
  

🧠 CMOS Inverter — Voltage Transfer Characteristics (VTC) and Theory

In Day 2, we focus on the basic CMOS inverter behavior and the effects of velocity saturation in short-channel devices using Sky130 SPICE simulations.


⚙️ Key Operating Principles

1. MOSFET as a Switch
   - NMOS: Conducts when V_GS > V_th
   - PMOS: Conducts when V_SG > |V_th|

2. Drain Current Dependence
   - Long-channel devices: I_D ∝ (V_GS - V_th)^2
   - Short-channel devices: High electric fields → velocity saturation → limits I_D

3. CMOS Inverter Action
   - Low Input (V_in < V_th): NMOS OFF, PMOS ON → V_out ≈ V_DD
   - Transition Region: Both transistors partially ON → V_out falls rapidly
   - High Input (V_in > V_th): NMOS ON, PMOS OFF → V_out ≈ 0

---

📊 Switching Threshold (V_m)

Switching threshold voltage V_m is the input voltage at which V_in = V_out

- At V_m, NMOS and PMOS currents are equal in magnitude
- Determines logic transition point and noise margins
- For balanced rise/fall times, PMOS is typically sized ≈ 2× NMOS (W/L)

| Parameter           | Symbol         | Typical Value |
| ------------------- | -------------- | ------------- |
| Switching Threshold | V_m            | ≈ 0.88 V     |
| Supply Voltage      | V_DD           | 1.8 V        |
| PMOS/NMOS ratio     | Wp/Wn          | ≈ 2          |

---

🧪 Theory — Velocity Saturation

- In short-channel devices, electrons cannot accelerate indefinitely
- Drain current saturates at high V_DS, limiting switching speed
- Important for low-power, high-speed CMOS design

| Effect                   | Impact on Inverter                              |
| ------------------------ | ----------------------------------------------- |
| Velocity Saturation      | Limits drain current, reduces gain              |
| Short Channel Effect     | Threshold voltage shift, increased leakage      |
| VTC Transition Sharpness | Affected by W/L ratio and mobility saturation  |
| Switching Threshold Vm   | Determines inverter’s balanced operating point |

---

🧩 CMOS Inverter VTC Visualization

- VTC curve represents V_out vs V_in
- Shows transition region, logic high/low levels, and switching threshold
- Sharpness indicates inverter’s gain and speed

💡 Observations:
- Symmetric switching occurs with (W/L)_PMOS ≈ 2 × (W/L)_NMOS
- Propagation delay and noise margin are directly influenced by device sizing and velocity saturation

---

<p align="center">
  <img width="1015" height="717" alt="image" src="https://github.com/user-attachments/assets/2a69dfd7-de17-44aa-a2b7-ce2b918f8cb8" />
</p>


🧠 Summary Table — Day 2 Key Concepts

| Concept                      | Key Takeaway                                                         |
| ---------------------------- | -------------------------------------------------------------------- |
| Velocity Saturation           | Limits electron velocity at high electric fields                     |
| Short Channel Effect          | Reduces drain current and transconductance                           |
| CMOS Inverter VTC             | Shows input-output behavior, transition region, and Vm               |
| Switching Threshold (Vm)      | Determines logic transition point, affects noise margin              |
| SPICE Simulation              | Enables observation of short-channel and velocity saturation effects |
