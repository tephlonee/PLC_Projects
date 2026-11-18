# Conveyor Control (FBD)

FBD control logic for **three conveyors in a row**. A pallet is moved from conveyor 1 to conveyor 2 to conveyor 3, using a sensor at each conveyor to know where it is.

## How it works

- Each conveyor has a sensor (`S1`, `S2`, `S3`) that detects a pallet.
- When a pallet is on a conveyor and the next one is free, both conveyors run to hand the pallet over.
- Transport memory flags (`mxTrpConveyor1to2`, `mxTrpConveyor2to3`) latch while a pallet is moving between two conveyors. That keeps the motors running while the pallet is between sensors.
- The handover stops once the pallet reaches the next sensor.

## Program structure

| POU | Language | Description |
|---|---|---|
| `MAIN` | ST | Calls `Control_FBD` every cycle |
| `Control_FBD` | FBD | Conveyor logic with self-holding transport flags |

## I/O

| Variable | Direction | Description |
|---|---|---|
| `ixSensor1..3` | Input | Pallet detected on conveyor 1–3 |
| `qxRunConveyor1..3` | Output | Run motor for conveyor 1–3 |
| `mxTrpConveyor1to2` | Memory | Pallet moving from conveyor 1 to 2 |
| `mxTrpConveyor2to3` | Memory | Pallet moving from conveyor 2 to 3 |

## Running

Open `TwinCAT Project Conveyor.sln`, activate the configuration, and start both `PLCSim` and `PLCStudent`. See the [root README](../README.md) for setup.
