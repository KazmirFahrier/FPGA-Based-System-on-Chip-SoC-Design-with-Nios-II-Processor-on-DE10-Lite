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
- 📉 Optionally reads ADC values and outputs them on display

---


---

## 💾 Requirements

- [x] Quartus Prime Lite Edition 16.1
- [x] DE10-Lite Development Kit
- [x] Nios II SBT for Eclipse
- [x] USB Blaster II driver
- [x] `Embed.zip` and `EmbedSoft.zip` from project resources

---

## ⚙️ Setup Instructions

### 🔧 Hardware Design (Module 3)
1. Create Quartus project with `DE10-Lite` template
2. Open **Qsys**, add and configure all components listed above
3. Export relevant ports and generate HDL
4. Integrate Qsys output into top-level Verilog file
5. Compile the design to generate `.sof` file
6. Program board via **Quartus Programmer**

### 💻 Software Development (Module 4)
1. Launch **Nios II SBT for Eclipse**
2. Create a new application from template (`Embed_System`)
3. Import provided `main.c`, `src/`, `inc/`
4. Configure the **BSP** (set RAM for stacks, `jtag_uart` for I/O)
5. Build BSP and application
6. Run the ELF on hardware using `Run As > Nios II Hardware`

---

## 🎯 Observed Hardware Behavior

- 🔄 **LEDs** toggle in response to counter logic or program-defined patterns
- 🎛️ **Slide Switches** trigger input detection and interrupts
- 🧭 **Accelerometer** readings update LED pattern when board is tilted
- ⚡ **ADC readings** display as raw hexadecimal values on 7-segment LEDs
- ✅ **Reset** clears LED state and sets displays to a known pattern (e.g., all “8”)
- 🧪 Verified full system response through physical board observation and lab notes

---

## 📈 Performance Metrics

- **Max Clock Frequency (Fmax):** Up to ~285 MHz in some configurations
- **Logic Utilization:** Up to 23% (varies by module)
- **Memory Use:** Within DE10-Lite constraints
- **Successful SPI, ADC, and IRQ operation confirmed**

---

## 📚 Learn More

- [MAX 10 Device Handbook](https://www.intel.com/content/www/us/en/programmable/products/fpga/max-series/max-10.html)
- [Qsys System Design Tutorial (Intel)](https://www.intel.com/content/dam/www/programmable/us/en/pdfs/literature/tt/tt_qsys_intro.pdf)

---


## 🧑‍💻 Author

Built and tested by **Kazmir Fahrier** as part of the **FPGA Capstone Project (Course 4)**.  
Special thanks to Intel, Coursera, and Terasic for the DE10-Lite platform.

---









  



