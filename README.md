The Multi Retro Dram Tester Software for V1.02 PCB See bottom of the page for the latest release. MULTI RETRO DRAM COMMANDER - USER Manual Updated 24.12.2025 Do Not Connect The Multi Retro Dram Tester to a USB port whilst powered from the 2.1mm DC Jack Socket! - Onlu use one or the other to power the DRAM tester. Note:- Maximum input voltage via the 2.1mm DC Jack Socket is 12V!

OVERVIEW
This device is a comprehensive tester for vintage Dynamic RAM (DRAM) chips ranging from the 1970s (4116) to the 1990s (71C4400). It uses a Raspberry Pi Pico (RP2040) to generate precise timing signals and verify memory integrity.

SOCKET SELECTION (IMPORTANT!)
The tester has 2 distinct ZIF sockets. You MUST use the correct one to avoid damaging the chip or the tester.

ZIF SOCKET 1 (SK1) - STANDARD 5V << Use this for all other chips (single +5V supply).

16-Pin: 3732, 4532, 4164, 41256, HM4816, 4532, 3732
18-Pin: 4464, 4416, 411000
20-Pin: 44256, 71C4400
ZIF SOCKET 2 (SK2) - MULTI-VOLTAGE << Use this for chips requiring -5V, +5V, and +12V.

4116 (16K x 1)
MK4027 (4K x 1)
TMS4108 (8K x 1)
The Main DRAM Test Algorithms
These are the selectable tests that stress the internal memory cells.

March B: +Retention
Type: Standard Industry Algorithm (Finite State Machine). Complexity: Low/Medium. What it does: Writes 0s to the whole chip. Then walks through reading 0/writing 1, then reading 1/writing 0. Best for: Detecting simple "Stuck-at" faults (a bit that is permanently 0 or 1) and some simple coupling faults. Faster than March C-.

March C- (Minus): +Retention
Type: Standard Industry Algorithm. Complexity: High (The "Gold Standard" for DRAM). What it does: A complex sequence that walks up and down the memory array, reading and writing inverted data multiple times (e.g., Read 0, Write 1, Read 1, Write 0). Best for: Detecting Stuck-at faults, Transition faults (cell fails to change from 0->1 or 1->0), Coupling faults (writing to cell A changes cell B), and Address Decoder faults. This is the recommended default test.

March C- Mix: (+Retention)
Type: Timing Stress Test. What it does: Runs the March C- algorithm twice. First run: Uses Standard Page Mode (slower, standard timing). Second run: Uses Fast Page Mode (FPM) (keeps RAS low, toggles CAS rapidly). Best for: Detecting chips that are logically functional but fail when accessed at high speeds (timing violations). Note: Only runs the second pass on chips that support FPM (e.g., 4164, 41256).

Checkerboard + Ret (Retention):
Pattern Test. What it does: Writes a logical checkerboard pattern (010101...) to the memory array, verifies it, then writes the inverse pattern (101010...) and verifies it. Best for: Testing Cell-to-Cell interference. Because a capacitor holding a '1' is physically next to a capacitor holding a '0', this tests for leakage between adjacent cells on the silicon die.

Type: Leakage Test. What it does: Runs the Checkerboard test, but pauses after writing the pattern for a specific duration (defined by Retention Time in settings), and then reads the data. Best for: Detecting Weak Bits. These are cells that work fine immediately, but lose their charge (turn from 1 to 0) faster than the standard refresh cycle allows. Crucial for verifying old/aging chips.

Extreme:
Type: Composite Suite. A selectable composition of all of the above algorithms - Default setting is MarchC- Mix & Checkerboard + Ret (Retention)

CONFIGURATION OPTIONS
LOOPS: Set to '0' for infinite running, or a specific number (e.g., 5).
POWER MODE:
ON: DRAM power remains ON after a successful test.
CYCLE: Power is turned OFF and then ON between every loop.
ON ERROR:
STOP: Halts immediately when a bad bit is found.
RESTART: Logs the error count and continues to the next loop.
PRE-TESTS - It is recommended not to turn any of these tests to the OFF state! - These are only really left in there for my debug!

These are quick checks run before the main algorithm to save time.

Pin Check:
What it does: Checks for electrical shorts between pins (e.g., Data shorted to Address) and pins stuck at Ground or VCC. Why: Prevents damage to the tester or the chip. If a short is found, the test aborts immediately, before applying power to the DRAM chip, thus protecting the DRAM tester!

Wake-up Refresh:
What it does: Sends a series of RAS-only pulses to the chip without reading or writing data. Why: "Warms up" the internal charge pump of the DRAM. Essential for the chip to behave stably before testing begins.

Presence Check:
What it does: Writes a '0' and a '1' to specific cells and uses internal Pull-Up/Pull-Down resistors to verify the data bus is actually being driven by a chip. Why: Distinguishes between a "Dead Chip" and an "Empty Socket."

Address Sweep:
What it does: Walks through address bits (A0, A1, A2...) individually. It writes to a base address and ensures that toggling a specific address bit doesn't affect the original data. Why: Detects broken address pins (e.g., if A4 is broken, writing to address 16 might actually overwrite address 0).

Data/WE Check:
What it does: Performs a quick write and read verification on a specific cell. Why: Confirms the Write Enable (WE) and Data In/Out (DQ) lines are functioning correctly.

Here is a brief description of the functions and menus available on The Multi Retro Dram Tester.
Main Menu Functions
Select Chip Type: Allows you to scroll through the supported DRAM models (e.g., 4164, 4116, 514400). This configures the voltages and pinouts automatically. Start Test: Initiates the selected test sequence. Settings: Opens the detailed configuration menu.

Test Configuration Settings
These options change how the memory test is performed.

Test Algorithm: Selects the specific logic used (March B, March C-, Checkerboard, etc.).

Access Mode:
Std Page: Uses standard RAS/CAS timing (compatible with all chips). Fast Page: Uses Fast Page Mode (RAS stays low, CAS toggles). Only available for chips that support it (4164 onwards).

Loop Count:
1: Runs the test once and stops. Infinite (0): Runs forever until you abort. Specific (e.g., 10, 100): Runs for a set number of passes.

On Error:
Stop: The tester halts immediately when a bad bit is found. Restart: The tester logs the error and immediately restarts the test loop. EOT (End of Test) Power:

Power Cycle:
Turns the chip off and back on between loops (ensures cold-start capability).

Power On:
Keeps the chip powered between loops (faster testing).

Retention Time:
Presets how long the tester waits during all tests to detect weak bits that lose charge. (Manufacturers Recommended Minimum Retention Time)

Cycle Delay:
Sets the pause duration between test loops (e.g., wait 1 second before starting the next pass).

Visual & UI Settings (*** Turn off the Bargraph and the Phase Messages for faster testing!***)
Bargraph: Toggles the progress bar at the bottom of the screen. Phase Msgs: Toggles the display of specific sub-steps (e.g., "March C- (w0)"). Turning this off speeds up the test slightly. Result Size: Small: Shows detailed statistics (Address, Expected vs. Actual data). Large: Shows a giant "PASS" or "FAIL" for easy viewing from a distance.

Advanced & Diagnostic Functions
Extreme Config: A submenu where you can manually enable or disable specific algorithms included in the "Extreme" test suite. Pre-Tests: Allows you to toggle safety checks on/off: (highly recommended to keep ON). Pin Check: Checks for shorts (highly recommended to keep ON). Presence: Checks if a chip is inserted. (highly recommended to keep ON). Address Sweep: Checks for broken address pins. (highly recommended to keep ON).

Test Hardware: A diagnostic mode for the tester itself. It allows you to manually toggle specific socket pins High (3.3V) or Low to verify PCB traces with a multimeter. It has two modes: Manual: You scroll the knob to select a pin. Auto: The tester cycles through pins automatically every second.

Reset Defaults: Wipes all settings back to factory defaults (except for Encoder Direction).

Controls
Rotary Scroll: Navigates menus. Rotary Button: Depressing the Button Selects an option or toggles a setting. Long Press (during test): Aborts the current test immediately and cuts power to the socket. Encoder Direction: Via PC command, you can reverse the direction of the knob if it feels "backward" to you. Display Driver: Via PC command, toggle between SSD1306 and SH1106 drivers for the OLED display.

TROUBLESHOOTING:
"OFFLINE": Check USB cable and ensure correct COM port is selected.
"Pass=0, Fail=0" after start: Check connection.
Chip getting hot: STOP IMMEDIATELY. You may have inserted it backward or in the wrong socket.
