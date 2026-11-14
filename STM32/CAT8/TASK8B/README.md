# CAT8B – STM32 Event-Driven GPIO Control (Software Switch Debouncing)

## Experiment Details

- **Course:** 25CS2741 – Fundamentals of Internet of Things + Lab
- **Assessment:** Continuous Lab Evaluation 8B
- **Date:** 18/09/2026
- **Experiment:** STM32 Event-Driven GPIO Control (Software Switch Debouncing)
- **Board:** STM32 Blue Pill (STM32F103C8T6 ARM Cortex-M3)
- **IDE:** STM32CubeMX and Keil MDK-ARM
- **Framework:** STM32 Hardware Abstraction Layer (HAL)

## 1. Aim

To upgrade the basic GPIO polling application into an edge-triggered toggle controller where each single tactile push-button press inverts the LED state exactly once, eliminating false triggers caused by mechanical contact bounce via a non-blocking software debounce algorithm.

## 2. Hardware Requirements

- STM32F103C8T6 Blue Pill development board
- Momentary normally-open (NO) tactile push-button (`SW1`)
- External 5mm LED (`D1`)
- Current-limiting resistor (220–330 Ω)
- Solderless breadboard and jumper wires
- USB cable / regulated 3.3 V power supply

## 3. Hardware Connections

| Component / Signal | Connection |
|---|---|
| Push-button SW1 | Bridges 3.3 V to PA0 when closed |
| LED D1 Anode (+) | PA1 via 220–330 Ω resistor |
| LED D1 Cathode (−) | GND rail |
| Board Power | USB / regulated 3.3 V |

### GPIO Configuration

| Pin | Mode | Configuration |
|---|---|---|
| PA0 | GPIO_Input | Internal Pull-Down enabled |
| PA1 | GPIO_Output | Push-Pull, Low speed |

## 4. Circuit Diagram

![CAT8B Circuit Diagram](circuit.png)

## 5. Working Principle

When a physical push-button is actuated or released, its metallic contacts vibrate for several milliseconds before settling, generating rapid bursts of HIGH and LOW electrical pulses known as **switch bounce**. Polling a raw input directly would misinterpret these transient oscillations as multiple separate button clicks.

To filter this noise without stalling CPU execution, the firmware executes a non-blocking state-tracking function named `Switch_Update()`.

The algorithm operates in five stages:

1. Sample the instantaneous logic level of `PA0` via `HAL_GPIO_ReadPin()`.
2. Compare the newly sampled raw state against the previously recorded raw reading.
3. Whenever a transition occurs, store the new raw state and capture a fresh millisecond timestamp using `HAL_GetTick()`.
4. Validate and accept the new logic state only once the raw reading stays uninterrupted for a minimum debounce window of `30 ms`.
5. Detect a rising edge (`LOW` → `HIGH`) on the validated stable state and invert `PA1` via `HAL_GPIO_TogglePin()`.

Because timing is tracked via SysTick timestamps rather than `HAL_Delay()`, the main execution loop remains completely responsive throughout the debounce window.

## 6. Core Logic / Pseudocode

1. Initialize the STM32 HAL framework and configure the system clock tree.
2. Configure `PA0` as a digital input with internal pull-down enabled.
3. Configure `PA1` as a push-pull digital output initialized to `LOW`.
4. Define the debounce stability threshold as `30 ms`.
5. Continuously sample the raw digital level on `PA0`.
6. If the raw sample differs from the last raw reading, update the last raw state and reset the reference timestamp via `HAL_GetTick()`.
7. Compute elapsed time (`current_tick - last_change_tick`); if it reaches or exceeds `30 ms`, treat the input as stable.
8. If the newly stabilized state differs from the previously accepted state, update the accepted state.
9. Trigger `HAL_GPIO_TogglePin()` once only when the accepted state transitions to `HIGH`.
10. Repeat indefinitely inside the main `while (1)` loop.

## 7. HAL Functions Used

- `HAL_Init()` – Initializes the HAL driver layer and configures the 1 ms SysTick time base.
- `HAL_GPIO_ReadPin()` – Samples the instantaneous logic level of the push-button pin (`PA0`).
- `HAL_GetTick()` – Returns the elapsed system uptime in milliseconds for non-blocking debounce timing.
- `HAL_GPIO_TogglePin()` – Inverts the output logic state of the LED pin (`PA1`) upon each validated rising edge.

## 8. Expected Observation

- Pressing the tactile push-button once toggles the LED state exactly once (`OFF` → `ON` or `ON` → `OFF`).
- Releasing the push-button produces no unintended state transition.
- Mechanical contact chatter does not trigger rapid flickering or double-toggling.
- The main application loop executes continuously without blocking delays.

## 9. Inference

Event-driven edge detection and non-blocking software debouncing were successfully implemented on the STM32F103C8T6 using the STM32 HAL time base (`HAL_GetTick()`). Enforcing a 30 ms signal stability window effectively suppressed contact-bounce transients, guaranteeing deterministic single-toggle LED behavior per physical actuation while keeping the CPU free for concurrent tasks.

## 10. Project Files

- `Core/Src/main.c` – Main application source code containing the `Switch_Update()` debounce logic.
- `Core/Inc/main.h` – Project pin definitions and header declarations.
- `Blinky.ioc` – STM32CubeMX hardware pinout and clock configuration file.
- `MDK-ARM/Blinky.uvprojx` – Keil MDK-ARM uVision project file.
- `MDK-ARM/startup_stm32f103xb.s` – Cortex-M3 startup and vector table assembly file.
- `circuit.png` – Hardware schematic and breadboard wiring diagram.

## 11. Safety and Wiring Checks

- Wire the push-button strictly between `PA0` and the `3.3 V` supply rail.
- Confirm that `PA0` has its internal pull-down resistor enabled so the pin does not float when open.
- Never apply `5 V` directly to `PA0`.
- Place a 220–330 Ω current-limiting resistor in series with the external LED to protect the GPIO pin.
- Confirm proper LED anode/cathode polarity prior to energizing the circuit.
- Verify a common ground (`GND`) connection across the STM32 Blue Pill and breadboard circuit.
