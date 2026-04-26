# FPGA-Based System-on-Chip (SoC) Design with Nios II Processor on DE10-Lite

This repository contains a complete FPGA-based embedded-system project for the Terasic DE10-Lite development board. The design builds a custom System-on-Chip around the Intel/Altera Nios II Gen2 soft processor, then runs a C application that controls the board LEDs through memory-mapped peripherals and communicates with the host PC over the JTAG UART.

The project was developed as part of the University of Colorado Boulder FPGA Design for Embedded Systems specialization capstone work. It demonstrates the full hardware/software flow: Platform Designer system integration, Quartus hardware compilation, BSP generation, firmware development, programming, and board-level validation.

## Project Goals

- Build a configurable FPGA SoC instead of relying on a fixed hard-processor microcontroller.
- Integrate processor, memory, clocks, debug, and I/O peripherals with Avalon-MM interconnects.
- Create a Nios II software application that exercises the generated hardware system.
- Validate the design on real DE10-Lite hardware using LEDs, switches, timers, and JTAG UART output.
- Preserve the capstone guide and lab notebook artifacts so future readers can reproduce the work.

## Repository Contents

| Path | Purpose |
| --- | --- |
| `hardware/` | Quartus project, Platform Designer system, generated Verilog, and timing constraints. |
| `software/` | Nios II C application source and helper modules for alarms, delays, JTAG UART output, LED writes, and validation. |
| `FPGA_Course4_CapstoneProjectGuideModule3.pdf` | Capstone module guide for the hardware/software integration stage. |
| `FPGA_Course4_CapstoneProjectGuideModule4.pdf` | Capstone module guide for final integration and validation. |
| `LabNotebook.pdf` | Development notebook and project record. |

## Hardware Platform

- Board: Terasic DE10-Lite
- FPGA: Intel/Altera MAX 10, device `10M50DAF484C6GES`
- Primary tools: Quartus Prime Lite / Platform Designer 16.1-era project files
- Top-level Quartus entity: `DE10_LITE_Default`
- Platform Designer system: `Embed.qsys`
- Generated SoC wrapper: `Embed.v`

## SoC Architecture

The design uses the Nios II Gen2 processor as the central controller. Instruction and data masters connect to memory and peripherals through Avalon-MM interconnect fabric. A clock-crossing bridge separates the main processor/memory clock domain from the lower-speed peripheral clock domain.

Key hardware blocks:

- `nios2_gen2_0`: 80 MHz Nios II Gen2 soft processor with instruction and data masters.
- `onchip_ram`: 16 KB on-chip RAM used as the reset and exception memory target.
- `onchip_flash_0`: MAX 10 on-chip flash region available to the processor.
- `sdram`: external SDRAM controller for the DE10-Lite SDRAM.
- `jtag_uart`: console I/O between the Nios II program and the host computer.
- `timer_0`: 32-bit Avalon timer used by HAL alarms and firmware delay routines.
- `led_pio`: 10-bit output PIO mapped to the DE10-Lite red LEDs.
- `slide_pio`: 10-bit input PIO mapped to the DE10-Lite slide switches, with edge interrupt support.
- `spi_0`: SPI master configured for the board accelerometer/G-sensor interface.
- `modular_adc_0`: MAX 10 modular ADC block with channel 1 enabled.
- `sysid`: System ID peripheral used to validate that the software is built for the matching hardware.

Clocking:

- `MAX10_CLK1_50` provides the 50 MHz board clock.
- `altpll_0` generates 80 MHz clocks for the CPU/SDRAM domain and a 40 MHz clock for selected peripherals.
- `altpll_1` feeds the modular ADC clock path from the ADC clock input.

Memory/peripheral map highlights:

| Slave | Base Address | Notes |
| --- | ---: | --- |
| SDRAM | `0x04000000` | External SDRAM controller. |
| Peripheral bridge window | `0x08000000` | Timer, switches, LEDs, SysID, and JTAG UART appear behind the clock-crossing bridge. |
| On-chip flash data | `0x09200000` | MAX 10 internal flash data interface. |
| On-chip RAM | `0x09404000` | 16 KB on-chip memory. |
| Nios II debug memory | `0x09408800` | Processor debug access region. |
| Modular ADC sample store CSR | `0x09409000` | ADC sample-store control/status registers. |
| SPI controller | `0x09409200` | SPI master control registers. |
| Modular ADC sequencer CSR | `0x09409240` | ADC sequencer control/status registers. |
| On-chip flash CSR | `0x09409248` | Flash control/status registers. |

## Firmware Behavior

The firmware in `software/main.c` runs an interactive LED counter:

1. Initializes the HAL alarm system.
2. Prints startup messages through the JTAG UART.
3. Waits for a command from the host console.
4. Drives the LED PIO with an 8-bit count pattern.
5. Prints each count value to the JTAG UART.

Supported commands:

| Command | Behavior |
| --- | --- |
| `u` | Count upward by 1 from `0x00` to `0xFF`. |
| `d` | Count downward by 1 from `0xFF` to `0x00`. |
| `3` | Count upward by 3 from `0x00`. |

The helper modules keep the firmware modular:

- `alarm_util.*`: starts 250 ms, 100 ms, 10 ms, and 1 ms HAL alarms.
- `delay_wait.*`: implements delay modes using the alarm counters.
- `jtag_uart_util.*`: formats count output for the JTAG UART console.
- `led_util.*`: writes LED values to the memory-mapped LED PIO.
- `system_validation.h`: compile-time checks that required Platform Designer peripherals exist.
- `error_loop.*`: halts execution after unrecoverable initialization errors.

## Build and Run Workflow

1. Open `hardware/Embed.qpf` in Quartus Prime Lite.
2. Open `hardware/Embed.qsys` in Platform Designer and regenerate HDL if the SoC has changed.
3. Compile the Quartus project to produce the FPGA programming file.
4. Program the DE10-Lite through the Quartus Programmer.
5. Create or refresh the Nios II BSP from the generated hardware description.
6. Add the files in `software/` to the Nios II application project.
7. Build the firmware with Nios II Software Build Tools for Eclipse or the command-line SBT flow.
8. Run the application on the target and open the Nios II console.
9. Enter `u`, `d`, or `3` in the console and observe the LED count sequence on the board.

## Validation Checklist

- Quartus recognizes the device as `10M50DAF484C6GES`.
- Platform Designer generates the `Embed` system without missing IP warnings.
- The BSP-generated `system.h` includes definitions for `LED_PIO_BASE`, `JTAG_UART_BASE`, `TIMER_0_BASE`, `SYSID_BASE`, and the Nios II CPU.
- The software prints the startup banner in the JTAG UART console.
- LEDs update in the same sequence printed in the console.
- Invalid console input is handled without crashing the application.

## Troubleshooting

- If software compilation reports missing peripheral macros, regenerate the Platform Designer system and recreate the BSP.
- If the JTAG UART console is silent, confirm the FPGA is programmed and the application is launched against the active Nios II target.
- If LEDs do not update, verify that `led_pio` is present in the generated `system.h` and that the DE10-Lite pin assignments were preserved.
- If timing fails after clock or memory edits, review the PLL settings and the SDC constraints in `hardware/DE10_LITE_Default.sdc`.
- If SysID validation fails, rebuild the BSP after regenerating hardware so the software and FPGA image match.

## Future Improvements

- Add a complete command-line SBT build script for firmware reproducibility.
- Extend the application to read slide switches and ADC values at runtime.
- Display sampled data on the seven-segment displays or VGA output.
- Add screenshots of the Platform Designer block diagram and board demo.
- Add a release package containing the generated `.sof`, `.elf`, and BSP settings.
