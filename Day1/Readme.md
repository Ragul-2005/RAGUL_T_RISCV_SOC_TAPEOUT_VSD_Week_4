# ⚡ Day 1 — NMOS Id vs Vds Analysis

### Ngspice + Sky130 — Device-Level CMOS Simulation

---

## 📌 Table of Contents
- [Introduction](#introduction)  
- [Why SPICE Simulations Matter](#why-spice-simulations-matter)  
- [NMOS Device Overview](#nmos-device-overview)  
- [Threshold Voltage & Body Effect](#threshold-voltage--body-effect)  
- [Operating Regions of NMOS](#operating-regions-of-nmos)  
  - [Linear (Resistive) Region](#linear-resistive-region)  
  - [Saturation (Active) Region](#saturation-active-region)  
- [SPICE Simulation Setup](#spice-simulation-setup)  
- [Running Simulations](#running-simulations)  
- [Results & Observations](#results--observations)  
- [Summary](#summary)  
- [Next Steps](#next-steps)  

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


Condition: $$
V_{DS} < V_{GS} - V_{T}
$$



$$
I_D = \mu_n C_{ox} \frac{W}{L} \left[ (V_{GS} - V_T)V_{DS} - \frac{V_{DS}^2}{2} \right]
$$

- Acts like a **voltage-controlled resistor**  
- Id increases **linearly** with Vds  
- Suitable for **amplifiers and switches**

---

### Saturation (Active) Region

Condition: 
$$
V_{DS} \ge V_{GS} - V_{T}
$$


$$
I_D = \frac{1}{2} \mu_n C_{ox} \frac{W}{L} (V_{GS} - V_T)^2
$$

- **Pinch-off** occurs near the drain  
- Id **saturates** and becomes almost constant

---

## SPICE Simulation Setup

Clone the repository:

```bash
git clone https://github.com/kunalg123/sky130CircuitDesignWorkshop.git

