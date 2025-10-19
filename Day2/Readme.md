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

Simulation command:
ngspice day2_nfet_idvgs_L015_W039.spice

📊 | Parameter                  | Value / Description |
| -------------------------- | ----------------- |
| Threshold Voltage (Vth)    | ≈ 0.45 V |
| Sweep Range VGS            | 0–1.8 V |
| Step Size                  | 0.1 V |

---

### 💻 NMOS Id vs Vds

* NMOS Id vs Vds Sweep
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

Simulation command:
ngspice day2_nfet_idvds_L015_W039.spice



📈 | Aspect                      | Observation |
| --------------------------- | ----------- |
| Linear Region               | Small VDS, Id ∝ VDS |
| Saturation Region           | Id plateaus at high VDS |
| Nested Sweep                | Outer: VGS 0–1.8 V, step 0.2 V; Inner: VDS 0–1.8 V, step 0.1 V |
| Velocity Saturation Effect  | Limits drain current, reduces gain |
| Short-Channel Effect        | Threshold shift, reduced transconductance |

---
  

---

🔬 CMOS Inverter — Voltage Transfer Characteristics (VTC)

📊 | Concept                     | Key Takeaway |
| --------------------------- | ------------ |
| MOSFET as Switch            | NMOS ON when VGS>Vth, PMOS ON when VSG>|Vth| |
| Inverter Action             | Low Vin: NMOS OFF, PMOS ON → Vout≈VDD; Transition: Both ON → Vout falls; High Vin: NMOS ON, PMOS OFF → Vout≈0 |
| Velocity Saturation Impact  | Limits Id at high VDS, reduces gain |
| Short Channel Effect        | Threshold voltage shift, reduced transconductance |
| Switching Threshold Vm      | Determines balanced logic transition & noise margin |
| VTC Curve                   | Shows Vout vs Vin, transition region, and gain |

---

➡️ Next Step: Proceed to Day 3 — CMOS Switching Threshold & Dynamic Simulations
