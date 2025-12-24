
🧠 Multi Retro DRAM Tester

Software for V1.02 PCB

See the bottom of this page for the latest software release.

📘 Multi Retro DRAM Commander — User Manual

Last Updated: 24-12-2025

⚠️ IMPORTANT POWER WARNING

DO NOT connect the Multi Retro DRAM Tester to USB while it is powered from the 2.1 mm DC jack.
Use one power source only at any time.

Maximum input voltage via DC jack: 12 V

Failure to follow this may permanently damage the tester.

🧩 Overview

The Multi Retro DRAM Tester is a comprehensive diagnostic platform for vintage Dynamic RAM (DRAM) chips ranging from the 1970s (4116) through to 1990s CMOS parts (71C4400).

It is powered by a Raspberry Pi Pico (RP2040), generating precise timing signals to validate memory integrity, timing margins, addressing logic, and data retention characteristics.

🔌 Socket Selection (CRITICAL)

The tester features two ZIF sockets.
Using the wrong socket can damage the chip or the tester.

🔹 ZIF Socket 1 — SK1 (Standard 5 V Only)

✔️ Use for single +5 V DRAM devices

Supported Packages

16-Pin: 3732, 4532, 4164, 41256, HM4816

18-Pin: 4416, 4464, 411000

20-Pin: 44256, 71C4400

🔸 ZIF Socket 2 — SK2 (Multi-Voltage)

⚠️ Required for –5 V / +5 V / +12 V DRAM

Supported Devices

4116 (16K × 1)

MK4027 (4K × 1)

TMS4108 (8K × 1)

🧪 Main DRAM Test Algorithms
March B

Type: Standard industry FSM algorithm

Complexity: Low–Medium

Description:
Writes zeros, then sequentially reads/writes 1 and 0 patterns.

Best For:

Stuck-at faults

Simple coupling faults

Notes: Faster than March C-

March C- (Recommended Default)

Type: Industry gold-standard algorithm

Complexity: High

Description:
Multi-pass read/write sequences walking both up and down memory.

Detects:

Stuck-at faults

Transition faults

Coupling faults

Address decoder faults

March C- Mix

Type: Timing stress test

Description:

Pass 1: Standard Page Mode

Pass 2: Fast Page Mode (FPM)

Best For:
Detecting chips that fail at speed despite passing logic tests.

Note:
FPM only runs on supported chips (4164, 41256, etc.)

Checkerboard + Retention

Type: Pattern + leakage test

Description:
Writes alternating bit patterns (0101 / 1010), then verifies after a delay.

Best For:

Cell-to-cell interference

Weak charge retention

Aging silicon detection

Extreme Mode

Type: Composite stress suite

Default Composition:

March C- Mix

Checkerboard + Retention

Fully configurable

⚙️ Configuration Options
Loops

0 → Infinite

1, 5, 10, … → Fixed pass count

Power Mode

ON: Power remains applied after a successful test

CYCLE: Power is removed and reapplied between loops

On Error

STOP: Halt immediately on failure

RESTART: Log error and continue

🧩 Pre-Tests (Highly Recommended)

⚠️ Disabling these is not recommended — they exist for safety and diagnostics.

Pin Check

Detects shorts, stuck-high, or stuck-low pins

Aborts before power is applied if a fault is found

Wake-Up Refresh

Issues RAS-only pulses

Stabilises internal DRAM charge pumps

Presence Check

Confirms a real chip is driving the data bus

Differentiates empty socket vs dead chip

Address Sweep

Verifies each address bit independently

Detects broken or shorted address lines

Data / WE Check

Confirms data I/O and Write Enable operation

🧭 Main Menu Functions

Select Chip Type
Automatically configures voltages and pinouts

Start Test
Begins the selected algorithm

Settings
Opens the configuration menu

🧪 Test Configuration Settings
Test Algorithm

Select March B, March C-, Checkerboard, Extreme, etc.

Access Mode

Standard Page Mode: Compatible with all DRAM

Fast Page Mode: Supported devices only

Loop Count

Single pass

Infinite

Fixed count (e.g. 10, 100)

End-of-Test Power

Power Cycle

Power On

Retention Time

Manufacturer-recommended minimum retention delay

Cycle Delay

Pause between loops (e.g. 1 s)

🖥️ Visual & UI Settings

💡 Disable these for maximum test speed

Bargraph: Progress indicator

Phase Messages: Sub-step display

Result Size:

Small: Detailed statistics

Large: Large PASS / FAIL display

🛠️ Advanced & Diagnostic Tools
Extreme Configuration

Enable or disable individual sub-tests

Pre-Test Controls

Toggle individual safety checks (recommended ON)

Test Hardware

Manual or automatic pin toggling for PCB diagnostics

Reset Defaults

Restores factory configuration
(Encoder direction preserved)

🎛️ Controls

Rotary Encoder: Navigate menus

Encoder Button: Select / toggle

Long Press (during test): Abort and power-cut

Encoder Direction: Reversible via PC command

Display Driver: SSD1306 / SH1106 selectable

🧯 Troubleshooting
Symptom	Action
OFFLINE	Check USB cable & COM port
Pass=0, Fail=0	Verify chip insertion
Chip gets hot	STOP IMMEDIATELY — incorrect socket or orientation
📌 Notes

This tester is designed for serious retro-computing diagnostics.
Incorrect socket selection or power misuse can permanently damage vintage DRAM.

Use carefully — these chips are no longer made.
