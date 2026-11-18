# TwinCAT PLC Projects

A collection of PLC control programs built with **Beckhoff TwinCAT 3** (IEC 61131-3). Each project controls a simulated industrial process: an elevator, a washing machine, a tank level loop, a drilling station, conveyors, and a two-PLC handshake system.

The projects use several IEC 61131-3 languages:

- **FBD** (Function Block Diagram)
- **SFC** (Sequential Function Chart)
- **ST** (Structured Text)

## Projects

| Project | Folder | Languages | Summary |
|---|---|---|---|
| Elevator | [`TwinCAT Virtual Elevator FBD`](./TwinCAT%20Virtual%20Elevator%20FBD/) | FBD | Three-floor elevator with floor call buttons, cabin buttons, indicator lights and door control |
| Washing Machine | [`TwinCAT Project Washer FBD`](./TwinCAT%20Project%20Washer%20FBD/) | FBD + SFC | Wash cycle (fill, heat, wash, drain, spin) run as an SFC sequence inside the FBD program |
| Tank Level Control | [`TwinCAT Project Tank`](./TwinCAT%20Project%20Tank/) | FBD + ST | Proportional (P) controller that keeps a tank at a set level |
| Drilling Machine | [`TwinCAT Project Drill Man Auto`](./TwinCAT%20Project%20Drill%20Man%20Auto/) | FBD + SFC + ST | Drilling station with manual and automatic modes, safety interlocks and a blink function block |
| Conveyor | [`TwinCAT Project Conveyor FBD`](./TwinCAT%20Project%20Conveyor%20FBD/) | FBD | Three conveyors that move pallets from one to the next using sensors |
| IO Box | [`TwinCAT Project IO Box FBD`](./TwinCAT%20Project%20IO%20Box%20FBD/) | FBD | Buttons and LEDs with a chain of timers for a running-light effect |
| Master/Slave Handshake | [`ToMasterProgram`](./ToMasterProgram/) | ST + FBD | Two PLCs that exchange signals using a short and a long strobe handshake protocol |

## Project layout

Each project is a standard TwinCAT 3 solution:

```
<Project folder>/
├── <Project>.sln              # Visual Studio / TwinCAT XAE solution
└── <Project>/
    ├── <Project>.tsproj       # TwinCAT system project
    ├── PLCSim/                # Process simulator (the virtual machine being controlled)
    └── PLCStudent/            # Control program
        ├── GVLs/              # Global variable lists (I/O mapping)
        └── POUs/              # Programs, function blocks and functions
```

The control program and the simulator run as two PLC instances on the same TwinCAT runtime. The control program's `%I*` / `%Q*` variables are linked to the simulator's I/O, so the logic can be tested without real hardware.

## Getting started

### Requirements

- Windows with **TwinCAT 3 XAE** (eXtended Automation Engineering) installed
- Visual Studio or the TwinCAT XAE Shell
- A TwinCAT runtime (local machine as target is fine). A 7-day trial license can be generated from TwinCAT.

### Running a project

1. Open the project's `.sln` file in TwinCAT XAE.
2. Pick the local system as the target.
3. **Activate Configuration** (generate a trial license if asked).
4. Put TwinCAT in **Run mode**.
5. Log in to both the simulator and the control PLC, then start them.
6. Open the simulator's visualization to work with the virtual machine.

## Naming convention

Variables use Hungarian-style prefixes:

| Prefix | Meaning | Example |
|---|---|---|
| `ix` | Digital input (`BOOL`, `%I*`) | `ixButtonStart` |
| `qx` | Digital output (`BOOL`, `%Q*`) | `qxMotorUp` |
| `iw` / `qw` | Word input / output | `iwActualLevel`, `qwPumpSpeed` |
| `mx` | Internal memory flag (`BOOL`) | `mxSafetyOK` |
| `mi` / `ms` | Internal integer / string | `miProportional` |
| `fb` | Function block instance | `fbBlink` |
