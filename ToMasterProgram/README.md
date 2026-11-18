# Master/Slave Handshake

Two PLC programs that coordinate with each other over digital I/O using a **strobe-based handshake protocol**. One PLC starts a request and the other responds. Each side only moves on once the other has confirmed the last step, so neither side gets ahead of the other.

The solution (`handshake.sln`) has two PLC projects:

| PLC | Role |
|---|---|
| `StudentPLC` | Starts the handshake: picks short or long mode and raises the strobe |
| `slavePLC` | Answers the handshake, runs a task (car animation), and reports back when it is done |

## Handshake protocols

The slave supports two modes, chosen with `ixShort` / `ixLong`.

### Short handshake

```
Master                     Slave
  │── strobe HIGH ─────────►│  step 0: idle, waiting
  │◄──────────── start/ack ─│  step 10: ready, ack held high
  │── start ───────────────►│
  │◄──────────── strobe ────│  step 20: slave strobe held high
  │── strobe LOW ──────────►│
  │                         │  back to idle
```

### Long handshake

```
Master                     Slave
  │── strobe HIGH ─────────►│  step 0:  idle
  │◄────────── strobe 1 ────│  step 10: hold strobe 1
  │── strobe LOW ──────────►│
  │◄────── start request ───│  step 20: ask master to start
  │── start ───────────────►│
  │◄────────── strobe 2 ────│  step 30: hold strobe 2
  │── strobe HIGH ─────────►│
  │── strobe LOW ──────────►│  step 40: wait for final low
  │                         │  back to idle
```

Both handshakes are written as `CASE`-based state machines in Structured Text, with a `step` variable and an `oxwait` output that is TRUE when the block is idle.

## Slave program

| POU | Language | Description |
|---|---|---|
| `MAIN` | ST | Calls `RunSlave` |
| `RunSlave` | FBD | Connects the handshake, lamps and car control |
| `SlaveHandeshake` | ST FB | Picks long or short mode and combines their outputs |
| `ShortHandshake` | ST FB | Short handshake state machine |
| `LongHandshake` | ST FB | Long handshake state machine |
| `CarContorl` | ST FB | Task run during the handshake: moves two cars (-2 and -3 per second) until both reach their goal, then sets `oxEnd` |
| `LampSlave` | ST FB | Drives the strobe indicator lamps |
| `LampDelay` | ST FB | Timed lamp sequence (2 s steps) that shows when a strobe is fully confirmed |

## Master-side program

| POU | Language | Description |
|---|---|---|
| `MAIN` | ST | Calls `RunStudent` |
| `RunStudent` | FBD | Short/long buttons, strobes and lamps |
| `LampStudent` | ST FB | Drives the strobe indicator lamps |
| `LampDelay` | ST FB | Timed lamp sequence (150 ms steps) |
| `ShortHandshake`, `LongHandshake` | ST FB | Interfaces for the master side of the protocol |

## I/O

| Variable | Description |
|---|---|
| `oxStrobOne`, `oxStrobTwo` | Strobe outputs to the other PLC |
| `ixStrobOne`, `ixStrobTwo` | Strobe inputs from the other PLC |
| `ixShort`, `ixLong` | Handshake mode select (slave) |
| `IXButtonShort`, `IXButtonLong` | Mode buttons (master side) |
| `OIntRedCar`, `OintCopCar` | Car positions for the visualization |

## Running

Open `handshake.sln`, activate the configuration, and start both PLC projects. Link the strobe outputs of one PLC to the strobe inputs of the other in the I/O mapping. See the [root README](../README.md) for setup.
