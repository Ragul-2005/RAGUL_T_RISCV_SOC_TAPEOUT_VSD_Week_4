# ⚙️ Day 2 — Velocity Saturation & Basics of CMOS Inverter VTC

### *NgspiceSky130 — CMOS Circuit Design & SPICE Simulation Journey*

---

## 📘 Introduction

On **Day 2**, we explore **short-channel NMOS behavior**, focusing on **velocity saturation** effects using the **Sky130 PDK**.  
We also design and simulate a **CMOS inverter** and plot its **Voltage Transfer Characteristic (VTC)**, linking **device physics** to **digital logic performance**.

---

## ⚡ Understanding Velocity Saturation in MOSFETs

In **short-channel devices** (e.g., 130 nm), carrier velocity **saturates** at high electric fields due to scattering, instead of increasing linearly.

### ✨ Key Points

- **Low fields:** Drift velocity \(v_d \propto E\)  
- **High fields:** \(v_d \rightarrow v_{sat}\)  
- **Impact:** Slower increase of drain current \(I_D\), reduced **gain**, and lower **transconductance** \(g_m\)

---

## 🧮 Drain Current with Velocity Saturation

The **saturation-region drain current** for short-channel NMOS:

\[
I_D = \frac{W}{L} \mu_n C_{ox} (V_{GS} - V_T) V_{DSsat}
\]

with

\[
V_{DSsat} = \frac{E_{sat} L}{1 + \frac{E_{sat} L}{V_{GS} - V_T}}
\]

where \(E_{sat}\) is the critical electric field at which velocity saturation begins.

> Observation: As channel length \(L\) decreases, **velocity saturation dominates**, limiting current and transconductance.

---

## 🔬 SPICE Simulation — NMOS Id vs Vgs

Perform a **DC sweep** on NMOS to extract **threshold voltage (Vth)**:

```spice
*Model Description
.param temp=27


*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt


*Netlist Description

XM1 Vdd n1 0 0 sky130_fd_pr__nfet_01v8 w=0.39 l=0.15

R1 n1 in 55

Vdd vdd 0 1.8V
Vin in 0 1.8V

*simulation commands

.op
.dc Vin 0 1.8 0.1 

.control

run
display
setplot dc1
.endc

.end
```

💻 Run Simulation
ngspice day2_nfet_idvgs_L015_W039.spice


Plot I<sub>D</sub> vs V<sub>GS</sub> and extract Vth using the linear extrapolation method:

Plot √I<sub>D</sub> vs V<sub>GS</sub>

Extrapolate the straight portion to intersect the V<sub>GS</sub> axis

Parameter	Value
Extracted V<sub>th</sub>	≈ 0.45 V


