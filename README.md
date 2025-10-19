# ⚡ Week 4 — CMOS Circuit Design with Sky130 PDK

## 🧩 RISC-V SoC Tapeout Program

<div align="center">
  <img src="https://img.shields.io/badge/Week4-CMOS%20Circuit%20Design-blue?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Tools-Ngspice-orange?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Language-SPICE-red?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Simulation-Transient%20Analysis-green?style=for-the-badge" />
</div>


Welcome to **Week 4** of the **RISC-V SoC Tapeout Program**! This week, we transition from digital logic abstraction to **transistor-level building blocks** that form the foundation of every logic gate. 🌱

You'll simulate and analyze **CMOS devices** using **Ngspice** with the **Sky130 PDK**, observing how **transistor physics** directly affects **timing, power, and reliability** on silicon. Each day links **analog device behavior** to **digital circuit performance**. ⚡💡

---

## 📚 Week 4 — Daily Tasks

| Day       | Focus Area                                                      | Folder Link                            | Description                                                                                                                                                       |
| --------- | --------------------------------------------------------------- | -------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Day 1** | 🔍 **NMOS Drain Current (Id) vs Drain-to-Source Voltage (Vds)** | [Day1](https://github.com/Ragul-2005/RAGUL_T_RISCV_SOC_TAPEOUT_VSD_Week_4/tree/bc3954986d678cce57a9bcf270ed0b5ffc434d1b/Day1)          | Simulate **Id–Vds** curves for NMOS at multiple **Vgs**, identify **linear** and **saturation** regions, and observe **channel-length modulation**.              |
| **Day 2** | ⚡ **Velocity Saturation & CMOS Inverter VTC**                   | [Day2](https://github.com/Ragul-2005/RAGUL_T_RISCV_SOC_TAPEOUT_VSD_Week_4/tree/bc3954986d678cce57a9bcf270ed0b5ffc434d1b/Day2)                | Analyze **Id–Vgs** behavior, estimate **threshold voltage (Vth)**, and construct a **CMOS inverter** to obtain its **Voltage Transfer Characteristic (VTC)**.    |
| **Day 3** | ⏱️ **Switching Threshold & Transient Response**                 | [Day3](https://github.com/Ragul-2005/RAGUL_T_RISCV_SOC_TAPEOUT_VSD_Week_4/tree/bc3954986d678cce57a9bcf270ed0b5ffc434d1b/Day3) | Apply a pulse input to the inverter, run **transient simulations**, and extract **switching threshold**, **rise/fall delays**, and **propagation delay**.       |
| **Day 4** | 🧮 **Noise Margin & Logic Robustness**                           | [Day4](https://github.com/Ragul-2005/RAGUL_T_RISCV_SOC_TAPEOUT_VSD_Week_4/tree/bc3954986d678cce57a9bcf270ed0b5ffc434d1b/Day4) | From the VTC, determine **VOH**, **VOL**, **VIH**, **VIL**, and calculate **Noise Margins (NMH, NML)** to evaluate logic stability.                               |
| **Day 5** | ⚙️ **VDD & Device Variation Effects**                             | [Day5](https://github.com/Ragul-2005/RAGUL_T_RISCV_SOC_TAPEOUT_VSD_Week_4/tree/bc3954986d678cce57a9bcf270ed0b5ffc434d1b/Day5)  | Study how **VDD scaling** and **W/L ratio variations** impact the inverter’s **switching point**, **transition slope**, and **robustness**.                       |

---

## 🌟 Learning Outcomes

By completing Week 4, you will:

* 🧠 Understand **NMOS and PMOS I-V characteristics** in the Sky130 process.
* ⚙️ Build and simulate a **CMOS inverter** from transistor models.
* 📈 Analyze **VTC**, **switching thresholds**, and **dynamic behavior**.
* 🔋 Explore the effects of **VDD and device variations** on performance and noise margin.
* 🔍 Connect **analog transistor physics** to **digital timing closure** considerations for later stages.

---

<div align="center">
  <img src="https://img.shields.io/github/last-commit/Ragul-2005/RAGUL_T_RISCV_SOC_TAPEOUT_VSD_Week_4?style=for-the-badge" alt="Last Commit" />
  <img src="https://img.shields.io/github/repo-size/Ragul-2005/RAGUL_T_RISCV_SOC_TAPEOUT_VSD_Week_4?style=for-the-badge" alt="Repo Size" />
  <img src="https://img.shields.io/github/issues/Ragul-2005/RAGUL_T_RISCV_SOC_TAPEOUT_VSD_Week_4?style=for-the-badge" alt="Open Issues" />
  <img src="https://img.shields.io/github/stars/Ragul-2005/RAGUL_T_RISCV_SOC_TAPEOUT_VSD_Week_4?style=for-the-badge" alt="Stars" />
  <img src="https://img.shields.io/github/forks/Ragul-2005/RAGUL_T_RISCV_SOC_TAPEOUT_VSD_Week_4?style=for-the-badge" alt="Forks" />
</div>

## 🏁 End of Week 4

- **Prepared by:** RAGUL T
- **Email:** tha.ragul2005@gmail.com
