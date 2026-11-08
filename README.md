# FPGA Digital Lock System

A digital locker lock (like the ones in gyms or changing rooms) implemented in **VHDL** and deployed on a **Nexys A7-100T** FPGA board. The user sets a 3-character hexadecimal code to lock the locker and must enter the same code to unlock it.

## Tech Stack
- **Language:** VHDL
- **Tools:** Xilinx Vivado
- **Hardware:** Digilent Nexys A7-100T (100 MHz clock, 7-segment display, buttons, LEDs)

## How It Works
1. The locker starts **free** (status LED off).
2. The user presses **ADD DIGIT** to start entering a code; the input LED turns on.
3. Each character (0–F) is selected with the **UP / DOWN** buttons and confirmed with **ADD DIGIT**.
4. After the 3rd character, the code is stored, the display turns off and the locker becomes **locked** (status LED on).
5. To unlock, the user enters 3 characters again. If they match the stored code, the locker is free again; otherwise it stays locked.
6. **RESET** returns the system to its initial state.

## Architecture
| Module | Role |
|---|---|
| `top_level.vhd` | Connects all modules to the board's inputs and outputs |
| `fsm.vhd` | Finite State Machine controlling the lock/unlock flow (`IDLE`, `ENTER_CODE_1..3`, `LOCKED`, `UNLOCK_CODE_1..3`, `CHECK_CODE`, `UNLOCKED`, `FAILED_UNLOCK`) |
| `debouncer.vhd` | Filters mechanical button bounce so each press is registered once |
| `modifica_cifra.vhd` | Increments/decrements the currently selected character (0–F) |
| `ssd.vhd` | Drives the 7-segment display |
| `constraint.txt` | Pin mapping (XDC constraints) for the Nexys A7-100T |

Full project documentation is available in `digital_lock_system.pdf`.

## How to Run
1. Create a new RTL project in **Vivado** for the Nexys A7-100T (`xc7a100tcsg324-1`).
2. Add the `.vhd` files from `src/` as design sources and `constraint.txt` as a constraints file (`.xdc`).
3. Set `top_level` as the top module.
4. Run Synthesis, Implementation and Generate Bitstream, then program the board.
