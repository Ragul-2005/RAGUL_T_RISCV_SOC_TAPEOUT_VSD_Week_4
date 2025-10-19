# ⚡ Week 4 — CMOS Circuit Design with Sky130 PDK

## 🧩 RISC-V SoC Tapeout Program

Welcome to **Week 4** of the **RISC-V SoC Tapeout Program**! This week, we transition from digital logic abstraction to **transistor-level building blocks** that form the foundation of every logic gate. 🌱

You'll simulate and analyze **CMOS devices** using **Ngspice** with the **Sky130 PDK**, observing how **transistor physics** directly affects **timing, power, and reliability** on silicon. Each day links **analog device behavior** to **digital circuit performance**. ⚡💡

---

## 📚 Week 4 — Daily Tasks

| Day       | Focus Area                                                      | Folder Link                            | Description                                                                                                                                                       |
| --------- | --------------------------------------------------------------- | -------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Day 1** | 🔍 **NMOS Drain Current (Id) vs Drain-to-Source Voltage (Vds)** | [Day1](./Day1_NMOS_Id_vs_Vds)          | Simulate **Id–Vds** curves for NMOS at multiple **Vgs**, identify **linear** and **saturation** regions, and observe **channel-length modulation**.              |
| **Day 2** | ⚡ **Velocity Saturation & CMOS Inverter VTC**                   | [Day2](./Day2_CMOS_VTC)                | Analyze **Id–Vgs** behavior, estimate **threshold voltage (Vth)**, and construct a **CMOS inverter** to obtain its **Voltage Transfer Characteristic (VTC)**.    |
| **Day 3** | ⏱️ **Switching Threshold & Transient Response**                 | [Day3](./Day3_CMOS_Switching_Dynamics) | Apply a pulse input to the inverter, run **transient simulations**, and extract **switching threshold**, **rise/fall delays**, and **propagation delay**.       |
| **Day 4** | 🧮 **Noise Margin & Logic Robustness**                           | [Day4](./Day4_Noise_Margin_Robustness) | From the VTC, determine **VOH**, **VOL**, **VIH**, **VIL**, and calculate **Noise Margins (NMH, NML)** to evaluate logic stability.                               |
| **Day 5** | ⚙️ **VDD & Device Variation Effects**                             | [Day5](./Day5_Power_Supply_Variation)  | Study how **VDD scaling** and **W/L ratio variations** impact the inverter’s **switching point**, **transition slope**, and **robustness**.                       |

---

## 🌟 Learning Outcomes

By completing Week 4, you will:

* 🧠 Understand **NMOS and PMOS I-V characteristics** in the Sky130 process.
* ⚙️ Build and simulate a **CMOS inverter** from transistor models.
* 📈 Analyze **VTC**, **switching thresholds**, and **dynamic behavior**.
* 🔋 Explore the effects of **VDD and device variations** on performance and noise margin.
* 🔍 Connect **analog transistor physics** to **digital timing closure** considerations for later stages.

---

## 🏁 End of Week 4

**Prepared by:** RAGUL T
**Email:** tha.ragul2005@gmail.com
