# CAT4 – 8051 4×4 Matrix Keypad, Seven-Segment Display, and LED Interfacing

## Experiment Details
- **Course:** 25CS2741 – Fundamentals of Internet of Things + Lab
- **Date:** 03/08/2026
- **Microcontroller:** AT89S52 (8051 Architecture)
- **Software:** Keil µVision IDE
- **Programming Language:** Embedded C

## Aim
To design and implement an embedded interface connecting a 4×4 matrix keypad, a seven-segment display, and output LEDs to the AT89S52 microcontroller, enabling real-time numeric display and dynamic LED pattern execution based on user keystrokes.

## Hardware Requirements
- AT89S52 Target Development Board
- 4×4 Matrix Membrane/Tactile Keypad
- D168K Seven-Segment LED Display
- On-board Status LEDs
- USB ISP Programmer

## Pin Configuration
- **P2.0–P2.3:** 4×4 Keypad column lines
- **P2.4–P2.7:** 4×4 Keypad row lines
- **P0.0–P0.7:** Seven-segment display data lines (`a`–`g` and `dp`)
- **P1.0:** On-board output LED

## Circuit Diagram

![CAT4 Circuit Diagram](circuit.png)

## Task Description
- Perform row-column matrix scanning to identify active keystrokes on the 4×4 keypad.
- Output the matching hex/decimal character onto the seven-segment display using a lookup table.
- Trigger specific LED animations mapped to the detected key value.
- Support running chaser, alternating toggle, binary up-counter, and synchronous blink routines.
- Clear the display and reset all active LED states when key `0` is pressed.

## Algorithm
1. Configure I/O ports for the matrix keypad, seven-segment display, and output LEDs.
2. Enter an infinite polling loop to scan keypad rows and columns sequentially.
3. Debounce and decode the coordinates of any pressed key.
4. Load the corresponding 7-segment bit pattern onto Port 0 to display the digit.
5. Branch to the mapped LED pattern routine associated with that key input.
6. Loop continuously to monitor for new keystrokes or a reset command (`0`).

## Source Files
- `week4_logic.c` – Bare-metal Embedded C source code.
- `week4.uvproj` – Keil µVision project configuration file.
- `Objects/week4.hex` – Compiled Intel HEX firmware binary.
- `circuit.png` – Hardware schematic and wiring diagram.

## Result
Successfully interfaced a 4×4 matrix keypad, seven-segment display, and LEDs with the AT89S52 microcontroller. User keystrokes were accurately decoded via matrix scanning to update the seven-segment readout and control the corresponding bare-metal LED output patterns.
