# Drilling Machine (Manual + Auto)

Control program for a virtual **drilling station** with two sliders (A and B) and a drill that moves up and down. The machine can run in **manual** mode (one button per motion) or **automatic** mode (a full drilling cycle run as a sequence). It also has emergency stop and safety interlocks.

## Features

- **Manual mode**: buttons move slider A, slider B and the drill in and out
- **Auto mode**: a sequence runs the full cycle (slide in, drill down, drill up, slide out)
- **Emergency stop**: two emergency stop buttons, latched until reset
- **Safety circuit**: two safety stops must be OK before anything can move
- **Cycle stop**: the stop button lets the current cycle finish before stopping
- **Status lamps**: started, emergency stop OK, safety OK, home position
- **Blink function block**: reusable `BLINK` FB for flashing lamps

## Programming structure

The program was written with a focus on clean, readable structure:

- **Naming**: internal flags use the `mx` prefix with a descriptive name, e.g. `mxSafetyOK`, `mxCycleStopActivated`, `mxEmergencyButtonPressed`.
- **One assignment per variable**: every output (`qx`) and memory flag (`mx`) is assigned in exactly one place.
- **One assignment per network**: each FBD network sets a single variable, so the logic is easy to follow.
- **Self-holding (latch) circuits** are used wherever a memory function is needed, e.g. emergency stop and run state.
- **Function block for blinking**: the blink logic is in its own `BLINK` FB.
- **Sequence in SFC**: the auto cycle is an SFC, separate from the output logic.

## Program structure

| POU | Language | Description |
|---|---|---|
| `MAIN` | ST | Calls `ControlFBD` every cycle |
| `ControlFBD` | FBD | Main program: safety, modes, latches and output networks |
| `ControlFBD.ACT` | SFC | Automatic drilling sequence |
| `BLINK` | ST function block | Two `TON` timers making an on/off pulse with set `OnTime` and `OffTime` |

## I/O

### Inputs

| Variable | Description |
|---|---|
| `ixButtonStart` / `ixButtonStop` | Start / stop |
| `ixAuto` | Auto/manual mode selector |
| `ixBtnReset` | Reset after emergency stop |
| `ixBtn1EmStopOk`, `ixBtn2EmStopOk` | Emergency stop buttons (OK when TRUE) |
| `ixSafety1StopOk`, `ixSafety2StopOk` | Safety stops (OK when TRUE) |
| `ixBtnAout`, `ixBtnAin`, `ixBtnBout`, `ixBtnBin` | Manual slider buttons |
| `ixBtnDrillOut`, `ixBtnDrillIn` | Manual drill buttons |
| `ixSliderAout/Ain`, `ixSliderBout/Bin` | Slider end position sensors |
| `ixDrillUp`, `ixDrillDown` | Drill end position sensors |

### Outputs

| Variable | Description |
|---|---|
| `qxSliderAout/Ain`, `qxSliderBout/Bin` | Slider motions |
| `qxDrillDown`, `qxDrillUp` | Drill motion |
| `qxLampStarted` | Machine running |
| `qxLampEmStopOk` | Emergency stop circuit OK |
| `qxLampSafetyOk` | Safety circuit OK |
| `qxLampHomePos` | Machine in home position |

## Running

Open `TwinCAT Project Drill.sln`, activate the configuration, and start both `PLCSim` and `PLCStudent`. See the [root README](../README.md) for setup.
