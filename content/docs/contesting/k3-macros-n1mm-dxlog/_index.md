---
title: "Elecraft K3 Macros in N1MM, DXLog"
date: 2026-09-26
draft: false
tags:
  - Elecraft K3
  - Elecraft
  - K3
  - N1MM
  - N1MM+
  - DXLog
  - Ham Radio
  - Contesting
  - DXPedition
  - RadioSports
  - CAT
categories:
  - Amateur Radio
  - DXpedition
---

When operating an **Elecraft K3** in a contest or DXpedition environment, it is often useful to control functions of the radio directly from your logging software.

Instead of reaching for the front-panel buttons, functions such as **SUB**, **RX**, **SPLIT**, **RIT**, **XIT** and the internal voice memories can be triggered from **N1MM Logger+** or **DXLog**.

This is done using macros and the K3's **CAT command interface**.

This article explains the basic concept and shows how to implement K3 commands in N1MM or DXLog.

## What is a macro?

A macro is simply a predefined command or sequence of commands that can be executed with a single action.

In contest software, macros are particularly useful because they allow you to assign frequently used radio functions to keyboard shortcuts or function keys.

For example, instead of manually pressing the **SUB** button on the K3, you could press **F8** in your logging software.

The logging software then sends a CAT command to the radio:

```text
SWT48;
```

The K3 interprets this as a simulated press of the SUB button.

The important point is that there are several layers involved:

```text
Keyboard / Function key
        ↓
Contest software macro
        ↓
CAT command
        ↓
Elecraft K3
        ↓
Radio function
```

N1MM and DXLog use different macro syntax, but the K3 command itself remains the same.

## Macros in N1MM Logger+

In **N1MM Logger+**, K3 CAT commands can be sent using the `{CATA1ASC ...}` macro.

For example:

```text
F8 SUB,{CATA1ASC SWT48;}
F9 RX,{CATA1ASC SWT25;}
```

Here, `F8` and `F9` are the function keys assigned to the macros.

The important part is:

```text
{CATA1ASC SWT48;}
```

This tells N1MM to send the K3 CAT command:

```text
SWT48;
```

### Example: SUB

```text
F8 SUB,{CATA1ASC SWT48;}
```

Pressing **F8** sends `SWT48;` to the K3.

The command simulates pressing the **SUB** button and therefore toggles the sub-receiver.

### Example: RX

```text
F9 RX,{CATA1ASC SWT25;}
```

Pressing **F9** sends `SWT25;`.

This simulates pressing the **RX** button and toggles the audio of the sub-receiver.

## Where are N1MM macros defined?

The macro definitions are entered in the **Function Key Editor** in N1MM Logger+.

This is where you define what should happen when you press keys such as F1, F2, F3, F8 or F9.

A macro therefore typically consists of:

| Part | Example | Purpose |
| --- | --- | --- |
| Function key | `F8` | Keyboard shortcut |
| Label | `SUB` | Text shown to the operator |
| N1MM macro | `{CATA1ASC SWT48;}` | Command sent to the K3 |

The important thing to remember is that **N1MM's `{CATA1ASC ...}` syntax belongs to N1MM**. The K3 itself does not know anything about `{CATA1ASC}`.

It only receives:

```text
SWT48;
```

## Macros in DXLog

DXLog uses a different macro syntax.

For raw CAT commands, DXLog uses:

```text
$CATRAW:
```

The general syntax is:

```text
$CATRAW:<K3 command>;
```

For example:

```text
$CATRAW:SWT24;
```

sends:

```text
SWT24;
```

to the K3.

Another example:

```text
$CATRAW:SWT25;
```

sends:

```text
SWT25;
```

to the radio.

## N1MM vs DXLog

The underlying K3 command is the same. Only the way the logging program sends it is different.

| Function | N1MM | DXLog | K3 command |
| --- | --- | --- | --- |
| Toggle SUB | `{CATA1ASC SWT48;}` | `$CATRAW:SWT48;` | `SWT48;` |
| Toggle RX/Sub audio | `{CATA1ASC SWT25;}` | `$CATRAW:SWT25;` | `SWT25;` |
| Toggle SPLIT | `{CATA1ASC SWT24;}` | `$CATRAW:SWT24;` | `SWT24;` |

This makes converting an N1MM macro to DXLog relatively straightforward:

### N1MM

```text
{CATA1ASC SWT48;}
```

### DXLog

```text
$CATRAW:SWT48;
```

The K3 command itself has not changed.

## Where are DXLog macros defined?

In DXLog, these commands are normally placed in the **function-key/macro configuration** used by the station setup.

The exact key assignment is a DXLog configuration issue; the important part for the K3 is that the macro contains:

```text
$CATRAW:
```

followed by the K3 CAT command.

For example:

```text
$CATRAW:SWT24;
```

When the macro is executed, DXLog sends the raw CAT command to the radio.

## SWT vs SWH

One of the most useful concepts when controlling a K3 through CAT is the difference between: SWT and SWH. They correspond to two different ways of interacting with a physical K3 button.

### SWT — Switch Tap

`SWTxx;` simulates a **short press** of a front-panel button.

It activates the button's primary function.

For example:

```text
SWT24;
```

simulates a short press of the **SPLIT** button.

In other words:

```text
SWT = press and release
```

### SWH — Switch Hold

`SWHxx;` simulates holding the button down for approximately **0.5 seconds**.

This activates the button's secondary function where applicable.

For example:

```text
SWH24;
```

simulates holding the **SPLIT** button.

According to the K3 behaviour described in this guide, this enables Split and automatically sets VFO B to **+1 kHz**.

So:

```text
SWT24;
```

and

```text
SWH24;
```

are deliberately different.

| Command | Meaning | Example |
| --- | --- | --- |
| `SWTxx;` | Short button press | `SWT24;` |
| `SWHxx;` | Button hold | `SWH24;` |

This distinction is important when converting physical K3 button operations into CAT commands.

## K3 button simulation commands

The following commands simulate pressing buttons on the K3.

| Command | K3 function | Description |
| --- | --- | --- |
| `SWT13;` | A=B | Copy VFO A frequency/mode to VFO B |
| `SWT11;` | A/B | Swap VFO A and VFO B |
| `SWT24;` | SPLIT | Toggle Split mode |
| `SWH24;` | SPLIT (Hold) | Enable Split and automatically set VFO B to +1 kHz |
| `SWT12;` | REV | Temporarily listen on the VFO B / TX frequency |
| `SWT09;` | XFC | Check the transmit frequency |
| `SWT48;` | SUB | Toggle the sub-receiver |
| `SWT25;` | RX (Sub) | Toggle sub-receiver audio |
| `SWT16;` | RIT | Toggle RIT |
| `SWT17;` | XIT | Toggle XIT |
| `SWT20;` | CLR | Reset RIT/XIT offset to 0 Hz |
| `SWT21;` | M1 | Play internal K3 memory message 1 |
| `SWT31;` | M2 | Play internal K3 memory message 2 |
| `SWT35;` | M3 | Play internal K3 memory message 3 |
| `SWT39;` | M4 | Play internal K3 memory message 4 |

## A practical DXLog example

Suppose you want to create three DXLog function keys for a K3:

- F8 = SUB
- F9 = RX Sub
- F10 = SPLIT

The K3 commands are:

```text
SWT48;
SWT25;
SWT24;
```

The corresponding DXLog macros become:

```text
$CATRAW:SWT48;
$CATRAW:SWT25;
$CATRAW:SWT24;
```

So the complete flow is:

| Function key | DXLog macro | K3 command | Result |
| --- | --- | --- | --- |
| F8 | `$CATRAW:SWT48;` | `SWT48;` | Toggle SUB |
| F9 | `$CATRAW:SWT25;` | `SWT25;` | Toggle RX/Sub audio |
| F10 | `$CATRAW:SWT24;` | `SWT24;` | Toggle Split |

This is a useful pattern to remember:

> **DXLog macro = `$CATRAW:` + K3 CAT command**

## Direct K3 CAT commands

Not every operation needs to simulate a button press.

The K3 also supports direct CAT commands that explicitly set a state.

This can be preferable when you want to tell the radio exactly what state it should be in rather than simply toggling a button.

For example:

```text
FT1;
```

explicitly enables Split.

And:

```text
FT0;
```

disables it.

That is different from:

```text
SWT24;
```

which simply simulates pressing the SPLIT button and therefore toggles the current state.

## Toggle commands vs explicit commands

This distinction is worth keeping in mind when designing contest macros.

| Type | Example | Behaviour |
| --- | --- | --- |
| Button simulation | `SWT24;` | Toggle the current state |
| Direct command | `FT1;` | Explicitly set the state |
| Direct command | `FT0;` | Explicitly clear the state |

For complex automated sequences, explicit commands can be useful because the resulting state is deterministic.

## Direct K3 CAT command reference

The commands included in the original guide are:

| CAT command | K3 function | Description |
| --- | --- | --- |
| `FT1;` | Split ON | Explicitly enable Split |
| `FT0;` | Split OFF | Explicitly disable Split |
| `SB1;` | Sub RX ON | Enable the sub-receiver |
| `SB0;` | Sub RX OFF | Disable the sub-receiver |
| `FR0;` | RX VFO A | Select VFO A for the main receiver |
| `FR1;` | RX VFO B | Select VFO B for the main receiver |
| `FT0;` | TX VFO A | Set the transmitter to VFO A |
| `FT1;` | TX VFO B | Set the transmitter to VFO B |
| `PA1;` | Preamp ON | Enable the preamplifier |
| `PA0;` | Preamp OFF | Disable the preamplifier |
| `ATT1;` | ATT ON | Enable the attenuator |
| `ATT0;` | ATT OFF | Disable the attenuator |
| `RT1;` | RIT ON | Enable RIT |
| `RT0;` | RIT OFF | Disable RIT |
| `XT1;` | XIT ON | Enable XIT |
| `XT0;` | XIT OFF | Disable XIT |

## Putting it all together

Once you understand the three layers, creating your own K3 macros becomes much easier.

Think of the system like this:

```text
             N1MM                         DXLog
               │                            │
               │ {CATA1ASC ...}             │ $CATRAW:
               │                            │
               └──────────┬─────────────────┘
                          │
                          ▼
                    K3 CAT command
                          │
                 ┌────────┴────────┐
                 │                 │
              SWTxx;             SWHxx;
                 │                 │
             short press       long press
                 │                 │
                 └────────┬────────┘
                          ▼
                     K3 function
```

For example, the same K3 operation can be expressed as:

### N1MM

```text
F8 SUB,{CATA1ASC SWT48;}
```

### DXLog

```text
F8 SUB,$CATRAW:SWT48;
```

### K3

```text
SWT48;
```

All three ultimately perform the same K3 operation: toggling the sub-receiver.

## Conclusion

For contest operation, K3 CAT macros provide a convenient way to bring frequently used radio functions directly into the logging software.

The most important concepts are:

1. **N1MM uses `{CATA1ASC ...}` to send K3 CAT commands.**
2. **DXLog uses `$CATRAW:` for raw K3 CAT commands.**
3. **`SWTxx;` simulates a short button press.**
4. **`SWHxx;` simulates a button hold.**
5. **Direct CAT commands can explicitly set a radio state instead of toggling it.**

Once these principles are understood, building function-key macros for a K3 becomes straightforward.
