# IO Box (FBD)

FBD logic for a virtual **IO box** with 8 push buttons and 8 LEDs. Besides basic button-to-LED mapping, it uses a chain of `TON` timers to switch the LEDs on one after another as a running light.

## How it works

- A start button sets `mxStart`, which starts the timer chain.
- Nine `TON` timers (`fbTime1`–`fbTime9`) are chained. Each timer's done bit (`mxTime1`–`mxTime9`) starts the next timer and drives an LED.
- `mxReturn` restarts the sequence when the last timer is done, so the pattern loops.

## Program structure

| POU | Language | Description |
|---|---|---|
| `MAIN` | ST | Calls `ControlFBD` every cycle |
| `ControlFBD` | FBD | Button, timer and LED logic |

## I/O

| Variable | Direction | Description |
|---|---|---|
| `ixButton1..8` | Input | Push buttons |
| `qxLed1..8` | Output | LEDs |

## Running

Open `TwinCAT Project IO Box.sln`, activate the configuration, and start both `PLCSim` and `PLCStudent`. See the [root README](../README.md) for setup.
