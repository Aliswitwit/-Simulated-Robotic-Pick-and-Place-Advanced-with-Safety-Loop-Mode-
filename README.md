# Simulated-Robotic-Pick-and-Place-Advanced-with-Safety-Loop-Mode-
This ladder logic project simulates a robotic pick-and-place sequence with integrated safety interlocks, emergency stop, and an automatic loop mode. The cycle performs arm extension, gripper closure, and arm retraction in a timed sequence.

🧠 Key Concepts Demonstrated
Sequenced Motion via TON Timers

Emergency Stop Latching Logic

Loop Mode Automation

Start Button & Auto Mode Parallel Triggering

Structured Subroutines (JSR for Safety & Modes)

🛠 System Description
🟢 Inputs
Address	Description
B3:0/0	Start Button
B3:0/7	E-Stop Button
B3:0/10	E-Stop Off Button
B3:0/12	Loop Mode Enable Button
B3:0/15	Loop Mode Disable Button
B3:1/0	System Reset Button

🔴 Outputs / Memory Bits
Address	Description
B3:0/3	Extend Motor
B3:0/5	Gripper Motor
B3:0/4	Retract Motor
B3:0/6	Sequence Complete
B3:0/9	E-Stop Active
B3:0/14	Loop Mode OFF Bit
B3:1/6	Loop Mode Active (NEW - Branch Bit)

🔧 Ladder Logic Flow
### 🔁 Rung 0 – Start Trigger (Manual or Loop Mode)

[ B3:0/0 ] Start Button
[ B3:1/6 ] Loop Mode Active
----[ONS B3:0/2]----( B3:0/1 ) Start Trigger
### 🔁 Rung 1 – Arm Extend (3s)

[ B3:0/1 ] Start Trigger
[ B3:0/3 ] Extend Output
[ T4:0/DN ] Done Bit
[ B3:0/9 ] E-Stop OFF
----[TON T4:0 3s]----( B3:0/3 )
### 🔁 Rung 2 – Gripper Close (1s)

[ T4:0/DN ] Extend Done
[ B3:0/5 ] Gripper Output
[ T4:1/DN ] Done Bit
[ B3:0/9 ] E-Stop OFF
----[TON T4:1 1s]----( B3:0/5 )
### 🔁 Rung 3 – Arm Retract (3s)

[ T4:1/DN ] Gripper Done
[ B3:0/4 ] Retract Output
[ T4:2/DN ] Done Bit
[ B3:0/9 ] E-Stop OFF
----[TON T4:2 3s]----( B3:0/4 )
### 🔁 Rung 4 – Loop Mode Logic

[ B3:0/12 ] Loop Mode Enable
[ B3:0/13 ] Loop Mode Trigger
[ B3:0/14 ] Loop Mode OFF
----( B3:0/13 )  ; Triggers Loop Mode ON

[ B3:0/13 ] AND [ B3:0/6 ] (Sequence Complete)
----( B3:1/6 ) ; NEW: Loop Mode Active bit (used in Rung 0)
🛑 E-Stop Logic (JSR 3) – Subroutine
B3:0/7 = E-Stop Button → Activates B3:0/8

B3:0/8 Latches into B3:0/9 (E-Stop Active)

B3:0/10 = E-Stop Off → Resets B3:0/9

### 🔁 Reset Logic

[ B3:1/0 ] Reset Button → Clears All Timers
Resets: T4:0, T4:1, T4:2

### 💡 Real-World Applications
Pneumatic pick-and-place

Compact robotic cell simulation

Interlocked motion control with auto-repeat

E-Stop-driven safety resets

### 🛠 Tools Used
RSLogix Micro Starter Lite

RSLinx Classic

RSLogix Emulate 500

