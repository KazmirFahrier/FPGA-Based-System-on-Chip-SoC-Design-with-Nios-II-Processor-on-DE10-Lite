# 🧠 FPGA-Based System-on-Chip with Nios II on DE10-Lite

This project implements a fully functional embedded **System-on-Chip (SoC)** on the **Intel DE10-Lite FPGA board** using the **Nios II soft-core processor**, developed with **Quartus Prime 16.1** and the **Nios II Software Build Tools for Eclipse**. It integrates both custom hardware IPs and bare-metal C software to demonstrate real-time data acquisition and I/O control.

---

## 🚀 Project Overview

- ⚙️ Built using the **Qsys (Platform Designer)** tool to assemble a custom SoC
- 👨‍💻 Programmed a bare-metal C application using **Nios II SBT for Eclipse**
- 🧪 Developed, tested, and debugged directly on the **DE10-Lite** FPGA board
- 🎯 Final system acts like a **digital voltmeter**, with LED and UART output

---

## 🛠️ System Architecture

### Hardware Components (Module 3):
- 🧠 **Nios II/f Soft-Core CPU**
- 🧮 **On-chip RAM (16KB)** and **On-chip Flash**
- 💡 **LED GPIO** (10-bit output)
- 🎚 **Slide Switches GPIO** (10-bit input, with IRQ)
- ⏱ **Interval Timer** (1ms periodic interrupt)
- 📦 **SDRAM Controller**
- 📈 **SPI Interface for ADXL345 Accelerometer**
- 🔍 **Modular ADC Core**
- 🔌 **JTAG UART** (for printf/debugging)
- 🆔 **System ID Peripheral**
- 🔁 **Clock Crossing Bridge** for peripherals at 40 MHz
- ⏲️ **PLLs** to generate clocks (80 MHz, 40 MHz, 10 MHz)

### Software Features (Module 4):
- 🧰 C application with `led_util.c`, `main.c`, and HAL BSP
- 🔁 LED counter with inversion logic toggle
- 🛜 UART debugging output via JTAG
- 🧪 Real-time interaction using Eclipse console
- 🧠 Optional integration with accelerometer & ADC

---

## 📷 Block Diagram

     +---------------------------+
     |         Nios II          |
     |      Soft Processor      |
     +------------+-------------+
                  |
    +-------------+--------------+
    |                            |
On-chip RAM                 On-chip Flash
    |                            |
  Timer                     System ID
    |                            |
 LED PIO <==> Slide PIO <==> Interval Timer
    |                            |
   SPI <=> Accelerometer     ADC Core
                  |
          JTAG UART Console



