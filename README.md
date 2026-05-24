# STM32 PWM LED Brightness Control

This repository contains an embedded C project developed for the STM32 Nucleo-F446RE development board. The project demonstrates how to generate a Pulse Width Modulation (PWM) signal using a hardware timer to smoothly control the brightness of the onboard user LED by varying its duty cycle.

---

## Project Overview

The objective of this project is to implement digital brightness control using a hardware timer configured in PWM mode.
* **Method:** Pulse Width Modulation (PWM) Mode 1.
* **Output Peripheral:** Onboard Green User LED (PA5) mapped to Timer 2, Channel 1 (TIM2_CH1).
* **Control Mechanism:** Varying the Capture/Compare Register (CCR) value to alter the average voltage delivered to the LED.

### Theoretical Background

A PWM signal controls the average analog voltage delivered to a load by cycling a digital signal between HIGH and LOW states. The average output voltage ($V_{avg}$) is directly proportional to the duty cycle:

$$V_{avg} = D \times V_{DD}$$

Where:
* $V_{DD}$ = Peak output voltage (3.3V for STM32 GPIO)
* $D$ = Duty Cycle, calculated as:

$$D = \frac{T_{ON}}{T_{ON} + T_{OFF}} = \frac{CCR}{ARR}$$

---

## Hardware Requirements

* Development Board: STM32 Nucleo-F446RE (ARM Cortex-M4)
* Connectivity: USB Type-A to Mini-B cable (for programming and debugging)

---

## Hardware & Peripheral Configuration

The system is configured via STM32CubeMX with the following settings:

| Peripheral | MCU Pin | Configuration Mode | Function |
| :--- | :--- | :--- | :--- |
| **User LED (LD2)** | `PA5` | Alternate Function | `TIM2_CH1` (PWM Output) |

### Clock & Timer Parameter Calculation
* **System Clock:** Configured via internal oscillator (RCC Bypass mode) to run at a maximum stable frequency of **84 MHz**.
* **Timer Frequency Formula:**

$$f_{PWM} = \frac{f_{CLK}}{(PSC + 1) \times (ARR + 1)}$$

To target a specific PWM frequency (e.g., 1 kHz) with a clock frequency of 90 MHz as an example calculation:
* Prescaler Register (PSC): `89` (Timer clock frequency becomes 1 MHz)
* Auto-Reload Register (ARR): `999` (Defines the period threshold)
* Capture/Compare Register (CCR): Configured between `0` and `999` to adjust the duty cycle dynamically from 0% to 100%.

---

## Software & Tools Used

* STM32CubeMX: For graphical clock configuration, hardware pin mapping to alternative functions (`TIM2_CH1`), and peripheral generation.
* STM32CubeIDE: For writing control loops, building the binaries, and target flash programming.

---

## Getting Started

1. Clone the Repository:
   ```bash
   git clone [https://github.com/your-username/your-repo-name.git](https://github.com/your-username/your-repo-name.git)
