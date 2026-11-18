# Washing Machine Control (FBD + SFC)

Control logic for a virtual **washing machine**. The wash cycle is a **Sequential Function Chart (SFC)** that runs inside the main **FBD** program. The FBD part handles the outputs, and the SFC steps the machine through the wash phases and loops the wash motion.

## How it works

1. **Start**: the `START` switch starts the cycle.
2. **Fill**: water is let in until the selected level sensor is reached (`ixSetLevel` picks the level).
3. **Heat**: water is heated until the temperature sensor reports OK.
4. **Wash**: the drum turns forward and backward, looping for a set number of rounds.
5. **Drain**: water is drained until the tub is empty.
6. **Spin**: the drum spins at high speed (centrifuge).
7. The cycle ends and the display shows the current phase.

The SFC sets internal flags (`mxFill`, `mxHeat`, `mxForw`, `mxBack`, `mxDrain`, `mxSpin`). The FBD networks map these flags to the real outputs, so each output is only written in one place.

## Program structure

| POU | Language | Description |
|---|---|---|
| `MAIN` | ST | Calls `Control_FBD` every cycle |
| `Control_FBD` | FBD | Main program: output networks and the call to the SFC sequence |
| `Control_FBD.ACT` | SFC | Wash cycle sequence (fill, heat, wash loop, drain, spin) |

## I/O

### Inputs

| Variable | Description |
|---|---|
| `ixStart` | Start switch |
| `ixPulse` | Pulse button |
| `ixSetLevel` | Water level selector |
| `ixLevel1..3` | Water level sensors |
| `ixNotEmpty` | Tub not empty |
| `ixTempOk` | Water temperature reached |

### Outputs

| Variable | Description |
|---|---|
| `qxFill` | Fill water |
| `qxHeat` | Heat water |
| `qxForw` / `qxBack` | Turn drum forward / backward |
| `qxSpin` | High spin (needs `qxForw` or `qxBack`) |
| `qxDrain` | Drain water |
| `qxDispBit1`, `qxDispBit2` | Display bits for the current phase |

## Running

Open `TwinCAT Project Washer.sln`, activate the configuration, and start both `PLCSim` and `PLCStudent`. See the [root README](../README.md) for setup.
