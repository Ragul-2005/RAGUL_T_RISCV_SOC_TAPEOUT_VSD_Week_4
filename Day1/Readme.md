# ⚡ Day 1 — NMOS Id vs Vds Analysis

<p>
  <img src="https://img.shields.io/badge/Week-4-blue?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Tasks-1_Completed-brightgreen?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Focus-NMOS%20Device%20Analysis%20%26%20SPICE%20Simulation-purple?style=for-the-badge" />
</p>
<p>
  <img src="https://img.shields.io/badge/Tool-Ngspice-lightblue?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Tool-Sky130%20PDK-orange?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Status-Completed-brightgreen?style=for-the-badge" />
</p>

### Ngspice + Sky130 — Device-Level CMOS Simulation

---

## 📌 Table of Contents
 [Introduction](#introduction)  
- [Why SPICE Simulations Matter](#why-spice-simulations-matter)  
- [NMOS Device Overview](#nmos-device-overview)  
- [Threshold Voltage & Body Effect](#threshold-voltage--body-effect)  
- [Operating Regions of NMOS](#operating-regions-of-nmos)  
  - [Linear (Resistive) Region](#linear-resistive-region)  
  - [💨 Drift Current Concept](#-drift-current-concept)  
  - [⚙️ Pinch-Off and Saturation Region](#️-pinch-off-and-saturation-region)  
- [Introduction to SPICE](#introduction-to-spice)  
- [⚙️ Setting Up SPICE with Sky130](#️-setting-up-spice-with-sky130)  
- [🧱 Understanding a SPICE Netlist](#-understanding-a-spice-netlist)  
- [✍️ Circuit Syntax Explained](#️-circuit-syntax-explained)  
- [🚀 Running Your First SPICE Simulation](#-running-your-first-spice-simulation)  
- [📊 Observations — Id vs Vds](#-observations--id-vs-vds)  
- [🧠 Summary](#-summary) 

---

## Introduction

This session begins our hands-on exploration of **analog CMOS design** using **Ngspice** with the **Sky130 PDK**.  
We focus on the **NMOS transistor**, analyzing how its **drain current (Id)** varies with **drain-to-source voltage (Vds)** under different **gate voltages (Vgs)**.

---

## Why SPICE Simulations Matter

**SPICE (Simulation Program with Integrated Circuit Emphasis)** allows us to study circuits **before fabrication**, saving time and resources.  

**Key benefits include:**

- ✅ Functional validation for different bias conditions  
- ⚡ Performance assessment (speed, gain, timing)  
- 🔋 Power analysis (static and dynamic)  
- 🔧 Optimizing transistor dimensions and biasing  
- 🧠 Learning device behavior and voltage-current relationships  

---

## NMOS Device Overview

An **NMOS transistor** is a **voltage-controlled device** that allows current between the **Drain (D)** and **Source (S)**, regulated by the **Gate (G)**.

**Terminals:**

| Terminal | Description |
|---------|-------------|
| Gate (G) | Controls channel conductivity |
| Source (S) | Electron entry point |
| Drain (D) | Electron exit point |
| Body (B) | Substrate, usually connected to GND |

<p align="center">
<img width="350" height="297" alt="image" src="https://github.com/user-attachments/assets/6f0b88d8-1d87-41e7-b0b0-cbe0525fc83e" />
</p>

---

## Threshold Voltage & Body Effect

- Below **Vth**, the transistor is **OFF**.  
- Above **Vth**, a **strong inversion layer** forms → transistor **ON**.  
- **Body effect:** Raising substrate voltage increases **Vth**.


The threshold voltage is calculated as:

<p style="font-size:40px;">
$$
V_T = V_{T0} + \gamma \left( \sqrt{2 \Phi_F + V_{SB}} - \sqrt{2 \Phi_F} \right)
$$
</p>



Where:  
$(V_{T0})$: Zero-bias threshold voltage
* $(V_{SB})$: Source-to-body voltage
* $(\gamma)$: Body effect coefficient
* $(\Phi_F)$: Fermi potential

<p align="center">
<img width="350" height="297" alt="image" src="https://github.com/Ragul-2005/RAGUL_T_RISCV_SOC_TAPEOUT_VSD_Week_4/blob/main/Day1/Images/unnamed.jpg?raw=true" />
</p>

---

## Operating Regions of NMOS

### Linear (Resistive) Region


Condition:   
$[
V_{DS} < (V_{GS} - V_T)
]$

$$
I_D = \mu_n C_{ox} \frac{W}{L} \left[ (V_{GS} - V_T)V_{DS} - \frac{V_{DS}^2}{2} \right]
$$

- Acts like a **voltage-controlled resistor**  
- Id increases **linearly** with Vds  
- Suitable for **amplifiers and switches**

<p align="center">
<img width="913" height="598" alt="image" src="https://github.com/user-attachments/assets/ab04dbd5-9e58-4dbf-8dc6-5e61331b6012" />
</p>
---

---
### 💨 Drift Current Concept

In the linear region, the current is mainly due to **carrier drift**:

$$
Q_i(x) = -C_{ox} \left[ (V_{GS} - V(x)) - V_T \right]
$$

$$
I_D = \mu_n C_{ox} \frac{W}{L} \left( V_{GS} - V_T - \frac{V_{DS}}{2} \right) V_{DS}
$$
<p align="center">
  <img width="1024" height="1024" alt="image" src="https://github.com/user-attachments/assets/4177f287-e85f-468e-8e78-257d0ff14a2f" />
</p>


> Id increases linearly with Vds before pinch-off occurs.

---

### ⚙️ Pinch-Off and Saturation Region

When 
$[
V_{DS} \ge (V_{GS} - V_T)
]$

- The channel near the **drain** is **pinched-off**.  
- Beyond this, **Id** saturates and remains almost constant.

$$
I_D = \frac{1}{2} \mu_n C_{ox} \frac{W}{L} (V_{GS} - V_T)^2
$$

<p align="center">
  <img width="748" height="349" alt="image" src="https://github.com/user-attachments/assets/aa04fa35-8d4e-4ac1-ac07-b9e15b3156fd" />
</p>


> NMOS acts like a **constant-current source** in this region.

---

## Introduction to SPICE

**SPICE (Simulation Program with Integrated Circuit Emphasis)** is a powerful circuit analysis tool used to simulate electronic circuits before physical fabrication.  
It allows designers to verify performance, power, and behavior across multiple operating conditions and process variations.

**Why use SPICE?**
- ✅ Predict circuit behavior before tapeout  
- ⚡ Analyze DC, AC, and transient responses  
- 🧩 Model device-level effects using PDK libraries  
- 🔧 Optimize design parameters for power, speed, and area  

---

## ⚙️ Setting Up SPICE with Sky130

To begin simulation using **Ngspice** with the **SkyWater 130nm open-source PDK**, follow these steps:

```bash
sudo apt install ngspice
git clone https://github.com/kunalg123/sky130CircuitDesignWorkshop.git
cd sky130CircuitDesignWorkshop/design
```
### 📁 Important Files in the Repository

| File Path                                                                | Description                                        |
| ------------------------------------------------------------------------ | -------------------------------------------------- |
| `/sky130_fd_pr/cells/nfet_01v8/sky130_fd_pr__nfet_01v8__tt.pm3.spice`    | SPICE model for the NFET (typical process corner). |
| `/sky130_fd_pr/cells/nfet_01v8/sky130_fd_pr__nfet_01v8__tt.corner.spice` | Corner model for NFET (used for PVT variations).   |
| `/sky130_fd_pr/models/sky130.lib.pm3.spice`                              | Main library containing all Sky130 process models. |

ℹ️ **Note:** All example `.spice` files for daily experiments are located inside the `design/` directory.

```bash
ragul@ragul-Dell-G15-5530:~/sky130CircuitDesignWorkshop$ ls
design  README.md
ragul@ragul-Dell-G15-5530:~/sky130CircuitDesignWorkshop$ cd design/
ragul@ragul-Dell-G15-5530:~/sky130CircuitDesignWorkshop/design$ ls
day1_nfet_idvds_L2_W5.spice      day4_inv_noisemargin_wp1_wn036.spice
day2_nfet_idvds_L015_W039.spice  day5_inv_devicevariation_wp7_wn042.spice
day2_nfet_idvgs_L015_W039.spice  day5_inv_supplyvariation_Wp1_Wn036.spice
day3_inv_tran_Wp084_Wn036.spice  id_test.cir
day3_inv_vtc_Wp084_Wn036.spice   sky130_fd_pr
```

## 🧱 Understanding a SPICE Netlist

The SPICE netlist defines the **circuit**, **model**, and **simulation commands**.
Below is an example setup for **NMOS Id–Vds simulation**:

```spice
*** Model Description ***
.param temp=27

*** Including sky130 library files ***
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

*** Netlist Description ***
XM1 vdd n1 0 0 sky130_fd_pr__nfet_01v8 w=5 l=2
R1 n1 in 55
Vdd vdd 0 1.8
Vin in 0 1.8

*** Simulation Commands ***
.op
.dc Vdd 0 1.8 0.1 Vin 0 1.8 0.2

.control
run
display
setplot dc1
.endc

.end
```

---

## ✍️ Circuit Syntax Explained

| Element                     | Description                                  |
| --------------------------- | -------------------------------------------- |
| **XM1**                     | MOSFET instance name                         |
| **vdd n1 0 0**              | Node connections — Drain, Gate, Source, Body |
| **sky130_fd_pr__nfet_01v8** | Sky130 NMOS model name                       |
| **w=5 l=2**                 | Transistor dimensions (in micrometers)       |
| **Vdd, Vin**                | DC sources for biasing                       |

🧠 *Example:*

```
M1 D G S B sky130_fd_pr__nfet_01v8 W=2u L=0.15u
```

---

## 🚀 Running Your First SPICE Simulation

Run the simulation in the terminal:

```bash
ngspice day1_nfet_idvds_L2_W5.spice
```

Inside the Ngspice prompt, plot the drain current:

```bash
plot -vdd#branch
```

This command plots the **drain current (Id)** vs **drain-to-source voltage (Vds)** for multiple gate voltages (Vgs).

<p align="center">
<img width="704" height="550" alt="image" src="https://github.com/user-attachments/assets/32a5eda1-773d-4409-8f90-931c05029f98" />
</p>
---

## 📊 Observations — Id vs Vds

In the plotted waveforms, observe:

* 📈 **Linear Region:** Current increases linearly with Vds at small voltages.
* 🌀 **Saturation Region:** Id flattens out as Vds increases (channel pinch-off).
* ⚙️ **Effect of Vgs:** Higher Vgs increases Id due to stronger channel inversion.

---

## 🧠 Summary

| Concept                  | Key Takeaway                                                |
| ------------------------ | ----------------------------------------------------------- |
| **SPICE Simulation**     | Essential for pre-fabrication circuit verification          |
| **NMOS Device Behavior** | Controlled by gate voltage (Vgs)                            |
| **Resistive Region**     | Linear Id–Vds behavior                                      |
| **Saturation Region**    | Id saturates ∝ (Vgs − Vth)²                                 |
| **Body Effect**          | Raises Vth when substrate potential increases               |
| **Ngspice**              | Open-source simulator for device and circuit-level analysis |

---
