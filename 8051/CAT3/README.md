# CAT3 – Timer-Driven LED Sequence Generator and Push-Button Mode Controller

**EXP:** CAT3

**DATE:** 27/07/2026

**NAME OF EXP:** Timer-Driven LED Sequence Generator and Push-Button Mode Controller

**HARDWARE SETUP / PLATFORM:**
- Microcontroller: AT89S52 (8-bit)
- Architecture: MCS-51 (8051)
- Development Board: AT89S52 Target Development Board
- Output Peripherals: Four discrete/on-board LEDs wired to GPIO port pins
- Input Peripheral: Single tactile push-button wired to a GPIO input pin

**TASK NO:**
- Architect and code a bare-metal, register-level GPIO port controller.
- Initialize four port lines as digital output channels for LED control.
- Initialize a dedicated port line as a digital input channel for switch sensing.
- Develop four distinct LED animation states in Embedded C.
- Integrate software switch debouncing to guarantee a single state transition per actuation.

**CIRCUIT DIAGRAM:**

![CAT3 Circuit Diagram](circuit.png)

**CORE LOGIC / PSEUDOCODE / CODE BASE:**

**1. Port & Pin Initialization**
- Assign four GPIO port pins to drive the external/on-board LEDs as outputs.
- Set up one GPIO port pin as an input line to read the push-button state.
- Control pin directions and logic levels directly through Special Function Registers (SFRs).

**2. Switch Sensing & Debouncing**
- Continuously poll or monitor the push-button input pin for state changes.
- Apply a timing delay filter to eliminate mechanical contact bounce noise.
- Confirm legitimate press events to prevent multiple false triggers.
- Increment the active state index upon each verified button actuation.

**3. LED Display Patterns**

- **Mode 1 – Chaser / Running Sequence:** Illuminates LED1 through LED4 one by one in a continuous loop.
- **Mode 2 – 4-Bit Binary Up-Counter:** Increments the LEDs to represent binary values cycling from `0000` to `1111`.
- **Mode 3 – Complementary Toggle:** Switches the output pattern back and forth between `1010` and `0101`.
- **Mode 4 – Synchronous Blink:** Toggles all four LEDs simultaneously between ON (`1111`) and OFF (`0000`).

**4. State Machine Flow**
- Upon power-up or reset, all four LEDs start in the OFF (`0000`) state.
- Every debounced button press steps the controller into the next sequential mode.
- Pressing the button while in Mode 4 wraps the state machine back to Mode 1.

**INFERENCE:**

A multi-mode LED pattern generator and push-button controller was successfully built on the AT89S52 microcontroller. The implementation validates direct SFR manipulation for digital I/O, reliable software debouncing for mechanical switches, and deterministic state-machine transitions across sequential, binary, alternating, and synchronous display patterns.
