# Capstone – Make the Computer Draw a Box

**Course:** Nand to Tetris – I (ETCCNT301), School of Engineering & Technology, K.R. Mangalam University
**Student:** Mayank Singh
**Roll number:** 2501730489

## Personal configuration

| Parameter | Rule | Value |
|---|---|---|
| K | Last two digits of roll number | **89** |
| Box start row | K | 89 |
| Box height | Fixed | 20 rows (rows 89–108) |
| Box width | 16 × ((K mod 4) + 1) = 16 × 2 | 32 pixels (2 words per row) |
| First screen word | SCREEN + 32 × 89 = 16384 + 2848 | 19232 |
| Assembler extra feature | K mod 3 = 2 | Duplicate-label warning |

## Contents

| File | Description |
|---|---|
| `CPU.hdl` | Hack CPU (A/D registers, ALU, jump logic, PC) |
| `Memory.hdl` | Memory map: 16K RAM, 8K Screen, Keyboard |
| `Computer.hdl` | Top-level computer: ROM32K + CPU + Memory |
| `assembler.py` | Two-pass Hack assembler in Python (`K = 89`) |
| `Box.asm` | Keyboard-controlled black box, assembly source |
| `Box.hack` | Machine code of `Box.asm` (43 instructions), produced by `assembler.py` |
| `make_box.py` | Helper that generates `Box.asm` / `Box.hack` for a given K |
| `index.html` | Lab web page |

## Part A – Hack computer

`CPU.hdl`, `Memory.hdl` and `Computer.hdl` are tested in the Nand to Tetris Hardware Simulator with the official `.tst` / `.cmp` scripts (CPU, Memory, ComputerAdd, ComputerMax, ComputerRect).

## Part B – Assembler

```
python assembler.py Prog.asm        # writes Prog.hack
```

- **Pass 1:** strips comments and blank lines and records label addresses; labels do not count as instructions.
- **Pass 2:** translates A- and C-instructions to 16 bits; new variables get RAM addresses from 16 upwards.
- **Extra feature (K mod 3 = 2):** prints `Line N: warning - label (NAME) is written twice` for duplicate labels.
- **Errors:** invalid instructions are reported with the source line number, e.g. `Line 2: invalid instruction "D=X+1"`, and no `.hack` file is written.

## Part C – Box program

```
python assembler.py Box.asm         # writes Box.hack
```

Load `Box.hack` in the CPU Emulator, set the speed to fast, and click the keyboard: a black 32 × 20 pixel box appears at row 89 while a key is held and disappears when it is released.

`Box.asm` keeps a `color` variable (−1 while a key is pressed, 0 otherwise) and fills 20 rows × 2 words starting at address 19232, moving 30 words forward after each row.

## How to run the box

1. Open the CPU Emulator.
2. Load `Box.hack`.
3. Set animation to *No animation* and run.
4. Hold any key to draw the box; release to clear it.
