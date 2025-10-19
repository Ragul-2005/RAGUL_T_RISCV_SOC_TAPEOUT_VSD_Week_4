
# ⚡ **Day 5 — CMOS Inverter Supply & Device Variation Analysis**


<!-- Badges in two rows, centered -->
<p align="center">
  <img src="https://img.shields.io/badge/Week-4-blue?style=for-the-badge" alt="Week">
  <img src="https://img.shields.io/badge/Task-5_Completed-brightgreen?style=for-the-badge" alt="Task">
<img src="https://img.shields.io/badge/Focus-Supply%5Fand%5FDevice-Voltage-orange?style=for-the-badge" alt="Focus">
</p>
<p align="center">
  <img src="https://img.shields.io/badge/Tool-NGSPICE-red?style=for-the-badge" alt="Tool">
  <img src="https://img.shields.io/badge/Library-Sky130-yellow?style=for-the-badge" alt="Library">
  <img src="https://img.shields.io/badge/Level-Intermediate-purple?style=for-the-badge" alt="Level">
</p>

---

## 📑 Table of Contents

1. [Objective — CMOS Inverter Supply & Device Variation Analysis](#-objective-—-cmos-inverter-supply--device-variation-analysis)
2. [Key Learning Outcomes](#-key-learning-outcomes)
3. [SPICE Deck — Supply Voltage Variation](#-spice-deck-—-supply-voltage-variation)
4. [Simulation and Result Visualization](#-simulation-and-result-visualization)
5. [SPICE Deck — Device Width Variation](#-spice-deck-—-device-width-variation)
6. [Running the Device Variation Simulation](#-running-the-device-variation-simulation)
7. [Combined View: Supply & Device Variations](#-combined-view-supply--device-variations)
8. [Key Observations](#-key-observations)
9. [Summary Table — Day 5 Key Concepts](#-summary-table-—-day-5-key-concepts)

## 🎯 **Objective — Studying Supply Voltage and Device Parameter Impact on Inverter Behavior**

The goal of **Day 5** is to analyze how **supply voltage (V<sub>DD</sub>)** and **transistor sizing (W<sub>p</sub>/W<sub>n</sub> ratio)** affect the **Voltage Transfer Characteristic (VTC)** and static performance of a CMOS inverter using **Sky130 SPICE simulations**.

This experiment extends the previous noise margin study by exploring how **power supply fluctuation** and **device width scaling** influence:

* Switching thresholds
* Output logic levels
* Noise tolerance
* Inverter robustness

---

## 🧩 **Key Learning Outcomes**

By the end of this session, you will:

* 🔋 Understand how **supply voltage reduction** impacts logic swing and output stability.
* ⚙️ Explore the effect of **PMOS/NMOS width scaling** on the inverter transfer curve.
* 📈 Observe how **VTC curves shift** with VDD and device sizing changes.
* 🧠 Relate these observations to **low-power digital circuit design** and **threshold balancing**.
* 🔬 Interpret SPICE data to make **device-level performance trade-offs**.

---

## ⚙️ **SPICE Deck — Supply Voltage Variation Study**

This deck sweeps **VDD** from 1.8 V down to 0.8 V in steps of 0.2 V to observe the corresponding change in the inverter’s transfer characteristics.

```spice
*Model Description
.param temp=27

*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

*Netlist Description
XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=1 l=0.15
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.36 l=0.15

Cload out 0 50fF

Vdd vdd 0 1.8V
Vin in 0 1.8V

.control
let powersupply = 1.8
alter Vdd = powersupply
	let voltagesupplyvariation = 0
	dowhile voltagesupplyvariation < 6
		dc Vin 0 1.8 0.01
		let powersupply = powersupply - 0.2
		alter Vdd = powersupply
		let voltagesupplyvariation = voltagesupplyvariation + 1
	end

plot dc1.out vs in dc2.out vs in dc3.out vs in dc4.out vs in dc5.out vs in dc6.out vs in \
xlabel "Input Voltage (V)" ylabel "Output Voltage (V)" \
title "CMOS Inverter — VTC as a Function of Supply Voltage"
.endc

.end
```

---

## 🧪 **Simulation and Result Visualization**

Run the SPICE file:

```bash
ngspice day5_inv_supplyvariation_Wp1_Wn036.spice
```

This simulation performs multiple DC sweeps for decreasing **VDD values** and overlays the resulting curves.

---

### 🖼️ **Image 01 — Supply Voltage Variation (VTC Stack)**

**Description:**
This plot displays six **VTC curves**, each corresponding to a different **supply voltage** ranging from **1.8 V → 0.8 V**.
As the supply voltage decreases:

* The **logic swing** reduces (VOH ≈ VDD ↓).
* The **transition region widens**, indicating slower and weaker switching.
* The **switching threshold (V<sub>M</sub>)** shifts downward, showing reduced drive strength.

This experiment demonstrates **how low-voltage operation limits noise margin** and **degrades signal integrity** — an important observation for low-power digital circuits.

<p align="center">
  <img width="706" height="545" alt="image" src="https://github.com/user-attachments/assets/678f70c9-27d1-4491-bd4c-fe7b5782ac2e" />
</p>

---

## ⚙️ **SPICE Deck — Device Width Variation Study**

Now we alter the **device sizing** while keeping the same load and supply to understand the impact of **(W<sub>p</sub>/W<sub>n</sub>) ratio** on transfer characteristics.

```spice
*Model Description
.param temp=27

*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

*Netlist Description
XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=7 l=0.15
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.42 l=0.15

Cload out 0 50fF

Vdd vdd 0 1.8V
Vin in 0 1.8V

*Simulation commands
.op
.dc Vin 0 1.8 0.01

.control
run
setplot dc1
display
.endc

.end
```

---

## ⚙️ **Running the Simulation**

To visualize the transfer curve:

```bash
ngspice day5_inv_devicevariation_wp7_wn042.spice
plot out vs in
```

---

### 🖼️ **Image 02 — Device Sizing Variation (W<sub>p</sub>/W<sub>n</sub> Impact)**

**Description:**
This graph shows the inverter’s **VTC curve** for a large **PMOS width (7 µm)** and **NMOS width (0.42 µm)** — a high **W<sub>p</sub>/W<sub>n</sub> ratio**.
You can observe:

* The **transition point** shifts left, indicating stronger PMOS pull-up.
* The **VOH** level remains near VDD, ensuring strong logic ‘1’.
* The **VOL** stays near 0 V, confirming full logic swing.

This setup increases **NMH (high-level noise margin)** at the cost of **NML**, making the inverter better at maintaining logic ‘1’ levels.

<p align="center">
  <img width="705" height="545" alt="image" src="https://github.com/user-attachments/assets/8cada59e-e212-48aa-88cd-bc779966cb89" /></p>

---

### 🖼️ **Image 03 — Combined View of Supply and Device Variations**

**Description:**
This comparative plot illustrates the **collective behavior** of the inverter under both **supply voltage scaling** and **transistor sizing**.
The visual highlights:

* **Steeper slopes** at higher VDD — faster transitions and higher gain.
* **Flattened curves** at lower VDD — reduced noise immunity.
* **Leftward shift** with larger PMOS width — improved logic-high strength.

The figure provides a comprehensive picture of **how design and environmental parameters co-influence inverter robustness**.

<p align="center">
  <img width="1039" height="705" alt="image" src="https://github.com/user-attachments/assets/5d091de2-f1cd-496c-b792-dc6858de8bda" /></p>

---

## 📊 **Key Observations**

| Parameter Change | Effect on VTC                          | Noise Margin Behavior | Design Implication                       |
| ---------------- | -------------------------------------- | --------------------- | ---------------------------------------- |
| ↓ Supply Voltage | Reduces logic swing, widens transition | NMH ↓ / NML ↓         | Low-power trade-off: less noise immunity |
| ↑ PMOS Width     | Left shift of VTC                      | NMH ↑, NML ↓          | Improves logic-‘1’ strength              |
| ↑ NMOS Width     | Right shift of VTC                     | NMH ↓, NML ↑          | Improves logic-‘0’ strength              |
| Balanced Wp/Wn   | Symmetrical VTC                        | Stable NMH & NML      | Optimum for standard-cell design         |

---

## 🧠 **Summary Table — Day 5 Key Concepts**

| **Concept**                     | **Takeaway**                                            |
| ------------------------------- | ------------------------------------------------------- |
| **Supply Variation**            | Affects overall logic swing and noise margin.           |
| **Device Sizing Ratio (Wp/Wn)** | Controls switching threshold and symmetry.              |
| **Low-VDD Operation**           | Saves power but reduces robustness.                     |
| **VTC Curve Analysis**          | Visual method to estimate voltage stability.            |
| **SPICE-Based Evaluation**      | Accurately captures non-ideal transistor behavior.      |
| **Design Insight**              | Trade-off exists between power, speed, and reliability. |

---

### 🧠 **Summary Table — Day 5 Key Concepts**

| **Concept**                     | **Key Takeaway**                                                 |
| ------------------------------- | ---------------------------------------------------------------- |
| CMOS Transient Response         | Demonstrates dynamic switching of inverter with pulse input      |
| Rise Time (tr) & Fall Time (tf) | Define output transition times; key to speed performance         |
| Propagation Delay (tpLH, tpHL)  | Determines switching speed and timing characteristics            |
| SPICE Transient Simulation      | Enables visualization of inverter delay and waveform transitions |
| Performance Factors             | Delay influenced by load capacitance, transistor sizing, and VDD |

---



