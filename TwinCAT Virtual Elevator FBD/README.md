# Elevator Control (FBD)

FBD control logic for a virtual **three-floor elevator**. The program handles call buttons on each floor, buttons inside the cabin, indicator lights inside and outside the elevator, motor direction and the door.

## Features

- Calls from each floor (outside buttons) and from inside the cabin
- Call is latched and its lamp stays on until the elevator arrives
- Motor runs up or down depending on where the call is
- Floor sensors stop the motor at the right level
- Door is closed while moving and opens on arrival
- Floor number shown on the cabin display
- Green/red buttons for run and stop

## Program structure

| POU | Language | Description |
|---|---|---|
| `MAIN` | ST | Calls `Control_FBD` every cycle |
| `Control_FBD` | FBD | Main elevator logic. Uses `SR`/`RS` latches for calls and run state, `TON`/`TOF` timers for door timing |
| `ctrl_SFC` | SFC | Placeholder for an SFC version of the sequence (not used) |
| `GVL` | — | Global I/O variables linked to the simulator |

## I/O

### Inputs

| Variable | Description |
|---|---|
| `ixButtLevel1..3` | Call buttons outside the elevator, floor 1–3 |
| `ixButtElevat1..3` | Floor buttons inside the cabin |
| `ixElevatLevel1..3` | Sensors that detect the elevator at floor 1–3 |
| `ixButtGreen` | Green button |
| `ixButtRed` | Red button (normally closed) |
| `ixDoorOpen` / `ixDoorClosed` | Door position sensors |

### Outputs

| Variable | Description |
|---|---|
| `qxLampLevel1..3` | Call lamps outside the elevator |
| `qxLampElevat1..3` | Call lamps inside the cabin |
| `qxMotorUp` / `qxMotorDown` | Motor direction |
| `qxDoorClose` | Close the door and keep it closed |
| `qiDispElevat` | Floor number display inside the cabin |

## Running

Open `TwinCAT Virtual Elevator.sln`, activate the configuration, start both `PLCSim` and `PLCStudent`, and use the simulator's visualization to call the elevator. See the [root README](../README.md) for setup.
