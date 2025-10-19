# 🧩 **Day 3 — CMOS Inverter Switching Threshold & Dynamic Behavior**

![Week](https://img.shields.io/badge/Week-4-blue?style=for-the-badge)
![Tasks](https://img.shields.io/badge/Tasks-3_Completed-brightgreen?style=for-the-badge)
![Focus](https://img.shields.io/badge/Focus-Static_and_Dynamic-Cyan?style=for-the-badge)

---

## 📚 Table of Contents


- [Objective — Explore CMOS Inverter Static & Dynamic Characteristics](#objective---explore-cmos-inverter-static--dynamic-characteristics)
- [SPICE Deck 1 — CMOS Inverter VTC Simulation](#spice-deck-1---cmos-inverter-vtc-simulation)
- [Voltage Transfer Curve (VTC) Analysis](#voltage-transfer-curve-vtc-analysis)
- [Switching Threshold (Vm)](#switching-threshold-vm)
- [Analytical Relations](#analytical-relations)
- [SPICE Deck 2 — Transient Response Simulation](#spice-deck-2---transient-response-simulation)
- [Transient Analysis](#transient-analysis)
- [Key Dynamic Parameters](#key-dynamic-parameters)
- [Practical Insights](#practical-insights)
- [Summary Table — Day 3 Key Concepts](#summary-table---day-3-key-concepts)


## 🎯 **Objective — Explore CMOS Inverter Static & Dynamic Characteristics**

On **Day 3**, we dive deep into the **CMOS inverter**, the cornerstone of digital logic. Using **Sky130 SPICE simulations**, we analyze both **static voltage behavior** and **dynamic timing response** to understand how inverters operate under various conditions.  

This session focuses on:

1. 🧩 **Static Characteristics (Voltage Transfer Curve – VTC):**  
   Observe how the **output voltage (Vout)** reacts to varying **input voltage (Vin)**, and determine the **switching threshold (Vm)** where NMOS and PMOS conduct equally.  
   Understanding VTC gives insight into **logic levels, noise margins, and gain regions**.

2. ⚡ **Dynamic Behavior (Transient Simulation):**  
   Investigate how the inverter responds to **time-varying inputs**. Analyze **propagation delays, rise/fall times**, and the effect of **load capacitance (C<sub>L</sub>)** on speed and energy consumption.  

By the end of this day, you’ll be able to:  

* 🧠 Understand how **transistor sizing (W/L ratios)** influences switching threshold and waveform symmetry.  
* 📈 Translate **theoretical inverter behavior** into **SPICE-generated voltage and timing plots**.  
* ⚙️ Gain foundations for **Static Timing Analysis (STA)** and **clock network planning**.  
* 💡 Appreciate the interplay of **device physics, sizing trade-offs**, and **load conditions** on inverter performance.  

---

## ⚙️ **SPICE Deck 1 — CMOS Inverter VTC Simulation**

```spice
*Model & Parameter Setup
.param temp=27

*Include Sky130 process library
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

*Circuit Netlist
XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=0.84 l=0.15
XM2 out in 0   0   sky130_fd_pr__nfet_01v8 w=0.36 l=0.15

Cload out 0 50fF

Vdd vdd 0 1.8V
Vin in  0 1.8V

*Simulation Commands
.op
.dc Vin 0 1.8 0.01

.control
run
setplot dc1
display
.endc

.end
```

## 📉 Voltage Transfer Curve (VTC) Analysis

The VTC shows how Vout changes with Vin, highlighting the inverter’s switching region and logic margins.

### 🧮 **Important Operating Regions**

| Input Range          | Transistor States | Output Voltage | Description                     |
| ------------------- | ---------------- | -------------- | --------------------------------|
| **Low Vin** (< Vth)  | NMOS OFF, PMOS ON | ≈ VDD          | Logic ‘1’ output               |
| **Transition** (~Vm) | Both partially ON | Falling rapidly | High-gain region, sensitive    |
| **High Vin** (> Vth) | NMOS ON, PMOS OFF | ≈ 0 V          | Logic ‘0’ output               |

<p align="center">
  <img width="704" height="545" alt="image" src="https://github.com/user-attachments/assets/2211dca5-dd32-4a53-b4ed-ded44c1fae9c" />
</p>


## ⚖️ **Switching Threshold (Vm)**

The **switching threshold** is defined where:
$[
V_{in} = V_{out}
]$

At this point, both transistors conduct equally, and the inverter is most sensitive to input changes.

| Parameter           | Symbol         | Typical Value @ 1.8 V VDD |
| ------------------- | -------------- | ------------------------- |
| Switching Threshold | V<sub>m</sub>  | ≈ 0.88 V                  |
| Supply Voltage      | V<sub>DD</sub> | 1.8 V                     |

---

## 🧠 **Analytical Relations**

The switching threshold depends on the transistor width ratio ((W/L)_p / (W/L)_n).

| Case        | ((W/L)_p / (W/L)_n) | Effect on Vm   | VTC Behavior         |
| ----------- | ------------------- | -------------- | -------------------- |
| Balanced    | ≈ 2                 | Vm ≈ VDD/2     | Symmetrical VTC      |
| PMOS narrow | < 2                 | Vm shifts up   | Strong ‘0’, weak ‘1’ |
| PMOS wide   | > 2                 | Vm shifts down | Strong ‘1’, weak ‘0’ |


🧮 Expression of Vm in terms of transistor sizes:
$[
V_m \approx \frac{V_{DD} + |V_{tp}| - \sqrt{\frac{\beta_n}{\beta_p}}(V_{DD} - V_{tn})}{1 + \sqrt{\frac{\beta_n}{\beta_p}}}
]$

where $(\beta = \mu C_{ox}(W/L))$

---

## ⚙️ **SPICE Deck 2 — Transient Response Simulation**

```spice
*Model Description
.param temp=27

*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

*Netlist Description
XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=0.84 l=0.15
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.36 l=0.15

Cload out 0 50fF

Vdd vdd 0 1.8V
Vin in 0 PULSE(0V 1.8V 0 0.1ns 0.1ns 2ns 4ns)

*Simulation commands
.tran 1n 10n

.control
run
.endc

.end
```

---

## ⚡ **Transient Analysis**

This simulation examines the **dynamic response** of the inverter to a square-wave input.

### 📊 **Waveform Description**

* **Input (Vin):** PULSE from 0 V to 1.8 V with 2 ns high time and 4 ns period.
* **Output (Vout):** Delayed inversion of Vin, with finite rise and fall times.

<p align="center">
  <img width="704" height="545" alt="image" src="https://github.com/user-attachments/assets/c30a946b-6a7e-4b25-9133-32a73475238b" />
</p>

---

## 📈 **Key Dynamic Parameters**

| Parameter             | Symbol                | Description                                   |
| --------------------- | --------------------- | --------------------------------------------- |
| **Propagation Delay** | t<sub>pd</sub>        | Time between 50% of Vin and Vout transition   |
| **Rise Time**         | t<sub>r</sub>         | Time Vout rises from 10% to 90% of VDD        |
| **Fall Time**         | t<sub>f</sub>         | Time Vout falls from 90% to 10% of VDD        |
| **Load Capacitance**  | C<sub>L</sub> = 50 fF | Affects switching speed and power dissipation |

---

## 🧩 **Practical Insights**

| Observation                            | Inference                                    |
| -------------------------------------- | -------------------------------------------- |
| Increasing PMOS width                  | Improves rise time but increases capacitance |
| Increasing NMOS width                  | Speeds fall time but affects balance         |
| Balanced β<sub>p</sub> ≈ β<sub>n</sub> | Ensures symmetrical switching behavior       |
| Dynamic simulation                     | Reveals realistic delays not seen in VTC     |

---

## 🧠 **Summary Table — Day 3 Key Concepts**

| **Concept**                      | **Key Takeaway**                                                           |
| -------------------------------- | -------------------------------------------------------------------------- |
| **CMOS Inverter Switching**      | Transition between logic states occurs near the switching threshold (Vm).  |
| **Static Characteristics (VTC)** | Defines logic behavior, switching region, and voltage gain.                |
| **Dynamic Response**             | Captures rise/fall times, propagation delay, and waveform transitions.     |
| **Transistor Sizing Impact**     | Larger W/L improves drive strength but affects power and delay trade-offs. |
| **SPICE Transient Simulation**   | Enables visualization of real-time inverter operation and delay metrics.   |

---
