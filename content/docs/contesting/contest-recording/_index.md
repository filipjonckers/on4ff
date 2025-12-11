---
date: '2025-12-10T20:20:00+02:00'
draft: false
title: 'Contest recording'
weight: 500
sidebar:
    open: true
tags: ["HAM", "Contesting", "Windows", "Radio Amateur", "Radio sports", "QSO", "CQWW", "QSOrder", "Recording", "MK2R+", "MicroHAM", "N1MM+", "N1MM", "DXLog", "HRDlog", "QRZ", "RumlogNG"]
---

## Continuous contest audio recording

[QSORDER by Vasily, K3IT](https://github.com/k3it/qsorder/releases) can be used to create continuous audio recordings during radio sport contests.

### Running QSORDER in the Background on Windows 11

#### Reliable, Continuous Contest Audio Recording Without Interruptions

Accidental closure of the QSORDER recording window is a common frustration for contest operators. When running N1MM Logger+, particularly in high-rate environments or multi-operator setups, a lost recording can mean losing essential verification data.

To prevent this, you can launch QSORDER in the background on Windows 11 using a simple VBScript. The recorder runs silently, cannot be closed by accident, and provides both QSO-by-QSO recordings and a mandatory continuous recording of the entire contest.

This article also includes important guidance on choosing the correct audio device — crucial for capturing exactly what the operator hears.

### Why Continuous Recording Is Essential

Major contest sponsors increasingly require full-time audio recordings.
For example, CQWW rules specify:

- Top operators must provide continuous recordings on request
- Any claimed top-finisher entry may be asked to submit recordings to verify QSOs
- Incomplete or missing recordings may lead to reclassification or disqualification

Because of these rules, it is no longer sufficient to record only individual QSOs. A complete, uninterrupted audio file for the entire contest weekend is now considered best practice for all serious entrants — and mandatory for many categories.

The ```--continuous``` option in QSORDER fulfills exactly this requirement.

### Selecting the Correct Audio Input Device

#### Record the Same Audio the Operator Hears

To make the recordings useful for adjudication, training, and post-contest analysis, you must record the exact audio the operator hears in the headphones — not a processed monitor channel, and not audio from only one radio when operating SO2R.

#### Recommended approach: “What You Hear”

When using devices like the MicroHAM MK2R+, audio routing is flexible. The simplest and most reliable choice is often the device named: ``` “What You Hear”, “Stereo Mix”, or “Monitor” ``` depending on the audio interface driver.

Selecting “What You Hear” (i.e. MicroHAM MK2R+) ensures that QSORDER captures:

- Radio audio
- Beeps, filters, and EQ
- SO2R switching audio
- Any pre- or post-processing applied to the operator channel

This guarantees a faithful recording exactly matching the operator’s experience — ideal for later review.

#### Finding the Right Device Index

QSORDER selects audio inputs by device index number. To identify the correct one:

- Open Windows Sound Settings → Recording tab
- Look for:
  - “What You Hear”
  - “Stereo Mix”
  - “Line (MicroHAM MK2R)”

Or any other monitor/loopback device

Note its position and test it with QSORDER until the expected audio is captured. Insert the matching ```--device-index=``` value into your script.

For more detailed explanations and alternatives, see [N6TV’s excellent presentation on contest audio recording techniques (Visalia 2019)](https://www.kkn.net/~n6tv/N6TV_Visalia_2019_Recording_A_Contest.pdf), which outlines several device-selection strategies.

## The Background-Recording Script for Windows 11

Save the following code as a ```start_recording.vbs``` file and double-click to start QSORDER silently:

```shell
' -------------------------------------------------------------------------------------------
' This will create a continuous recording - besides the QSO per QSO recording
' by starting QSORDER recorder in the background
' Script by ON4FF, Filip Jonckers
' -------------------------------------------------------------------------------------------
' check below that the paths to N1MM+ directory are correct
' and set device index to sound input device
' check that the QSO broadcasts UDP port is enabled and correct (default 12060)
' station nr is the N1MM station number to be used in the file name (optional)
' WshShell option 0 means no window
' WshShell option False means don't wait until program finishes
' to stop this process create a batch file with:
'   taskkill /IM qsorder.exe /F
' -------------------------------------------------------------------------------------------

Set WshShell = CreateObject("WScript.Shell")

WshShell.Run """C:\Users\myusername\Documents\N1MM Logger+\QsoRecording\qsorder.exe""  --device-index=1 --path=C:\Users\myusername\Documents\RECORDING --continuous --port=12060 --buffer-length=45 --delay=10", 0, False

```

Tip: Put a shortcut to the script on the desktop for easy access before the contest.

### Notes

- Ensure that the path to qsorder.exe is correct.
- Ensure that the path to the recording directory is correct.
- ```--device-index=1``` should be replaced with the index of your “What You Hear”/monitor device.
- You may run a separate QSORDER instance per N1MM station or radio depending on the audio configuration.
- Use left audio channel for radio 1 and right audio channel for radio 2 (SO2R)
- Use the 'What You Hear' option when using a MicroHAM MK2R+.
- The ```--continuous``` recording is essential for modern contest rule compliance.


## Stop recording

When troubleshooting or at the end of the contest it is always handy to be able to stop the recording.

Create a ```stop_qsorder.bat``` file containing:

```shell
taskkill /IM qsorder.exe /F
```

Run it to terminate ALL background QSORDER instances.

## Using Other Logging Programs (DXLog, etc.)

Although the examples above use N1MM Logger+, the same background-recording method works with other contest logging software such as DXLog, Win-Test, or any logger that provides a UDP QSO broadcast.

QSORDER simply listens on a UDP port for QSO information.
As long as your logging software sends QSO data to QSORDER, recordings will be created automatically.

### Check the UDP Port Settings

In N1MM+, the default QSO broadcast port is: UDP Port 12060

Other logging programs may use the same default or allow you to configure it manually. To ensure QSO-based recordings work correctly:

- Open your logger’s UDP broadcast settings
- Enable QSO broadcast or QSOs via UDP
- Verify the port matches the QSORDER command line, example: --port=12060

### Example for DXLog

DXLog allows enabling UDP Send QSO and specifying the broadcast port.
Just set this port to the same value used in your QSORDER script, and QSORDER will automatically create a new recording file every time a QSO is logged.


## Final Thoughts

Running QSORDER in the background is a simple but powerful technique that:

- Prevents accidental closure
- Produces contest-compliant continuous recordings
- Captures exactly what the operator hears (when correctly selecting the audio device)
- Works perfectly for SO1R, SO2R, multi-station, and advanced MicroHAM products setups
