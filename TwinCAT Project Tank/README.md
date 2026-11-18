# Tank Level Control (P Controller)

A **proportional (P) controller** that keeps the water level in a virtual tank at a set point. The pump speed is calculated from the difference between the wanted level and the measured level.

## Control law

```
PumpSpeed = (Set - Is) * Kp
```

| Term | Value |
|---|---|
| `Set` | Constant set point: **50** |
| `Is` | Measured level, `GVL.iwActualLevel` |
| `Kp` | Gain, starts at **10** and can be raised to find where the loop starts to oscillate |

## Implementation notes

The FBD uses `SUB` for the error and `MUL` for the gain.

- **WORD can't be negative.** `iwActualLevel` is a `WORD`, and subtracting from it would wrap around to a large positive number instead of going negative. It is converted with `WORD_TO_INT` before the `SUB`.
- **Negative output is clamped.** If the level is above the set point, the result is negative. A `SEL` block sets negative values to `0`.
- **Back to WORD.** The result is converted with `INT_TO_WORD` before it is written to `GVL.qwPumpSpeed`.

```
iwActualLevel ──WORD_TO_INT──┐
                             ├─ SUB(50, Is) ─ MUL(Kp) ─ SEL(<0 → 0) ─ INT_TO_WORD ─► qwPumpSpeed
            Set = 50 ────────┘
```

The status display shows **High Level** (≥ 60), **Low Level** (≤ 40) or **Normal Level**.

## Program structure

| POU | Language | Description |
|---|---|---|
| `MAIN` | ST | Calls `FunktionBlockDiagram` every cycle |
| `FunktionBlockDiagram` | FBD | P controller and status text logic |
| `SPEED_CONTROL` | ST function | Same P control in Structured Text: `(50 - level) * 10`, clamped at 0 |
| `OUTPUT_TEXT` | ST function | Returns the status string for a given level |

## I/O

| Variable | Type | Description |
|---|---|---|
| `GVL.iwActualLevel` | `WORD` input | Current tank level |
| `GVL.qwPumpSpeed` | `WORD` output | Pump speed 0–100 % (40 % slowly drains, 50 % holds, 60 % fills) |
| `GVL.qsStatusText` | `STRING` output | Text on the info display |

## Tuning

1. Start with `Kp = 10` and watch the level settle.
2. Raise `Kp` step by step. The level reaches the set point faster.
3. Keep going until the level starts to oscillate. That shows the stability limit of the loop.

## Running

Open `TwinCAT Project Tank.sln`, activate the configuration, and start both `PLCsim` and `PLCstudent`. See the [root README](../README.md) for setup.
