# EXPERIMENT DETAILS

**EXP:** CONTINUOUS ASSESSMENT 5 (CAT5)

**DATE:** 10/08/2026

**NAME OF EXP:** Hardware Timer and Interrupt-Based Timing for LED Fading, Pulse Generation, and Stepper Motor Control

**HARDWARE SETUP / PLATFORM:** AT89S52 (11.0592 MHz) + Keil C51 uVision + Oscilloscope

---

## TASK 5B.2: Variable-Frequency Square-Wave Generator

### 1. Hardware Setup & Circuit Diagram

- **MCU:** AT89S52 clocked at 11.0592 MHz
- **Signal Output Pin:** `P1.1` routed to Oscilloscope Channel 1
- **Mode Selector Button:** Tactile switch interfaced to `P3.2`
- **Frequency Modes:** 100 Hz, 500 Hz, 1 kHz, 2 kHz, and 5 kHz

![CAT5B.2 Circuit Diagram](circuit.png)

### 2. Core Logic / Pseudocode / Code Base

**Working Principle:**

Timer 0 is configured in 16-bit Mode 1 to trigger hardware interrupts corresponding to the half-period of the desired output frequency. Precalculated 16-bit timer reload constants are retrieved from lookup arrays based on the active mode. On every overflow interrupt, pin `P1.1` is inverted, synthesizing a symmetrical 50% duty-cycle square wave.

Actuating the push-button connected to `P3.2` steps sequentially through the five predefined output frequencies.

**Pseudocode:**

1. Precalculate the 16-bit Timer 0 initial reload counts corresponding to the half-periods of all five frequency settings.
2. Initialize Timer 0 in Mode 1 (16-bit non-auto-reload) via `TMOD` and enable Timer 0 and global interrupts (`ET0 = 1`, `EA = 1`).
3. Inside the Timer 0 ISR:
   - Re-initialize `TH0` and `TL0` from the lookup tables using the active mode index.
   - Invert the logic level of output pin `P1.1`.
4. Continuously monitor the `P3.2` input pin and apply software debouncing to filter switch bounce.
5. Upon a confirmed button press, advance `freq_mode` modulo 5 to select the next frequency band.

**Code Base:**

- `th_table[]` and `tl_table[]` hold the precomputed high and low byte reload values for each target frequency.
- Using static lookup tables eliminates costly runtime arithmetic inside the ISR, minimizing interrupt latency.
- `freq_mode` tracks the currently selected frequency index across main-loop polling and ISR execution.

**Build Note:** Recompile `TASK5B_2.uvproj` in Keil C51 uVision to update `Objects/TASK5B_2.hex` prior to programming the AT89S52 target board.

### 3. Inference

Combining Timer 0 Mode 1 interrupts with precomputed reload lookup tables provided accurate, selectable square-wave synthesis spanning 100 Hz to 5 kHz. The generated waveform exhibited a stable 50% duty cycle on the oscilloscope, while the debounced push-button allowed smooth real-time switching across frequency modes.
