This project implements a **16-depth, 8-bit wide synchronous FIFO** in Verilog.

## Features

- Synchronous read and write on a single clock
- Full and empty status flags
- Handles overflow and underflow conditions
- Maintains correct data order (FIFO behavior)
- Active-low reset

## Files

- `sync_fifo_16x8.v` — FIFO design code
- `sync_fifo_tb.v` — Testbench for functional verification
