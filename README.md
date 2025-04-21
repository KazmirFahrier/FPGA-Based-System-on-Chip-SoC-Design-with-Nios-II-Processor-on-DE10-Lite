# FPGA-Based System-on-Chip (SoC) Design with Nios II Processor on DE10-Lite

## 🎯 Objective
The goal of this project was to develop a System on a Chip (SoC) using the **NIOS II soft processor** on the **DE10-Lite FPGA development kit**.

---

## 🛠️ Problems Addressed

- **Lack of Flexibility**: Traditional embedded systems with hard processors can be inflexible in terms of system modifications and upgrades.
- **System Efficiency**: The need for low-latency processing in real-time applications, such as data acquisition and signal processing, without compromising performance or scalability.

---

## 🔧 Technical Approach

- Utilized the **Altera MAX 10 FPGA** to design a System on a Chip (SoC) featuring the **NIOS II soft processor**.
- Designed the system using **Qsys (Platform Designer)** to integrate various hardware components, including:
  - On-chip memory
  - ADC modules
  - Timers
  - Peripherals
- Developed software using **NIOS II Software Build Tools (SBT) for Eclipse** to ensure seamless integration with the hardware.
- Created a **Board Support Package (BSP)** and custom application software for interaction with peripherals such as LEDs, switches, and sensors.

---

## 📦 Key Hardware Component

- **DE10-Lite Development Kit**:  
  Includes peripherals such as:
  - SDRAM
  - Accelerometer
  - LEDs
  - Pushbuttons
  - Temperature sensor

---

## 💻 Software Development

- Created both the **BSP (Board Support Package)** and application-level software.
- Implemented a simple **voltmeter application** to demonstrate system functionality.
- Used the **Eclipse IDE** for development, compilation, testing, and real-time debugging.

---

## ✅ Outcome

- Successfully designed an advanced embedded system architecture showcasing the **potential of FPGA-based SoCs**.
- Demonstrated practical applications in:
  - Real-time signal processing
  - Control
  - Monitoring tasks

---

> 🧠 **This project highlights the synergy between hardware and software in embedded systems using FPGAs**, making it ideal for scalable and efficient real-time applications.
