# Software Application

This directory contains the Nios II C firmware that runs on the FPGA SoC.

## Application Summary

`main.c` implements a console-controlled LED counter. The program communicates through the JTAG UART, accepts a simple command, and writes each count value to the LED PIO.

Commands:

| Input | Action |
| --- | --- |
| `u` | Count up by 1. |
| `d` | Count down by 1. |
| `3` | Count up by 3. |

The LED output uses an 8-bit count value even though the hardware LED PIO is 10 bits wide. The extra PIO bits remain available for extension.

## Source Files

| File | Purpose |
| --- | --- |
| `main.c` | Main firmware loop and command handling. |
| `src/alarm_util.c`, `inc/alarm_util.h` | Creates periodic HAL alarms at 250 ms, 100 ms, 10 ms, and 1 ms intervals. |
| `src/delay_wait.c`, `inc/delay_wait.h` | Busy-wait delay helper based on the selected alarm interval. |
| `src/jtag_uart_util.c`, `inc/jtag_uart_util.h` | Prints count values to the Nios II JTAG UART console. |
| `src/led_util.c`, `inc/led_util.h` | Writes the current count value to the LED PIO data register. |
| `src/error_loop.c`, `inc/error_loop.h` | Stops execution after non-recoverable initialization errors. |
| `inc/event_handlers.h` | Placeholder prototypes for switch/key event handlers. |
| `inc/sysid_util.h` | Prototype for optional SysID display helper. |
| `inc/system_validation.h` | Compile-time checks for required hardware components and names. |
| `inc/main_includes.h` | Common include wrapper for the helper modules. |

## BSP Expectations

The application expects a Nios II BSP generated from the `hardware/Embed.qsys` system. The generated `system.h` must define the expected peripherals, including:

- `LED_PIO_BASE`
- `JTAG_UART_BASE`
- `TIMER_0_BASE`
- `SYSID_BASE`
- `ONCHIP_RAM_BASE`
- `ALT_CPU_NAME`

The validation header intentionally fails compilation if important peripherals are missing. This is useful because it catches mismatches between the firmware and the current FPGA image before runtime.

## Build Notes

1. Generate or refresh the BSP from the Platform Designer hardware handoff.
2. Add these C and header files to the Nios II application project.
3. Ensure the include path can resolve the BSP headers.
4. Build the application.
5. Download/run the ELF on the programmed DE10-Lite.
6. Use the Nios II console to enter `u`, `d`, or `3`.

The layout matches the include paths in the sources: `main.c` sits in `software/`, headers are in `software/inc/`, and the helper modules are in `software/src/`. Keep that structure when adding the files to the application project and the project headers resolve with no extra include path.

## Runtime Flow

1. Print a startup banner.
2. Start HAL alarms.
3. Ask the user for a count direction/mode.
4. Print the selected mode.
5. Write the current count value to the LED PIO.
6. Delay using the configured alarm interval.
7. Continue until the count reaches its terminal value.
8. Return to the command prompt and wait for another input.

## Extension Ideas

- Add switch-based mode selection using `slide_pio`.
- Add key interrupts and implement pause/resume controls.
- Read the MAX 10 ADC and display analog values through the JTAG UART or LEDs.
- Use the G-sensor SPI interface to visualize acceleration data.
- Add a makefile or SBT script so the firmware can be rebuilt outside Eclipse.
