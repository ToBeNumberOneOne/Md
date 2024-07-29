## Basic

Maxspeed 起重机控制系统,Digital Sway Control provides a unique electronic solution to control sway on cranes.Our system minimizes the adverse effects of propagation delay times created by PLC or peripheral computer-based systems because the feedback for closed loop sway control is in the trolley drive regulator.



Maxview 起重机视觉系统

Maxview Smart Landing®系统——一种基于激光传感器的船形检测和操作员辅助着箱系统

Maxview Clear Path®系统——码头和堆场起重机防碰撞和区域保护系统

Maxview Chassis Guidance™系统——将卡车驾驶员引导至精确的目标位置以便快速转移集装箱，从而缩短周期时间，提高产量

Maxview® Bucket Unloader System enables the operator to become more efficient by eliminating tasks where the operator's skills are not required.  


## 程序梳理

Auto Get/Put  code actions:

- A Abort
- L	Landside
- W	Waterside
- R	ASC
- S	Spreader
- D	Done
- X	Done_New
- V	Move Done
- M	Move
- N	No action
- O	Override

触发大车防撞保护后的策略

DEADLOCK BAD DECISION PROTECTION.

If the two crane are moving towards each other and stop at the anticollsion distance then the deadlock has made a bad decision.

In this case the following logic decides which crane goes back to state 2 and recalculated the decision.
1) this crane  does  not have a container and the other crane does.
2) this crane has lowers priorty and both do not have containers or both have containers
3) this crane is landside and is not high priorty and both do not have containers or both have containers


## Abbr

- oth -- Other
- CrnDir -- Crane Director
- Prev --  Previous
- Pwr -- Power
- Stp -- Stop
- Ins -- Instruction
- Psnt -- Present
- Rem -- Remote
- Xtrn -- External
- Tm  -- Time


## Ladder instructions

- %AI - Analog input
- %AQ - Analog output
- %G  - Genius gloable
- %I  - Input
- %M  - Internal
- %Q  - Output
- %R  - Register
- %S  - System
- %T  - Temporaray

- **GT** Energizes output 'Q' if 'IN1' is greater than 'IN2'. Otherwise, clears 'Q'.

- **LT** Energizes output 'Q' if 'IN1' is less than 'IN2'. Otherwise, clears 'Q'.

- **NE** Compares two  operands and energizes output 'Q' if they are not equal. Otherwise, clears 'Q'.

- **OFDT_THOUS**  The off-delay timer increments every thousandth of a second (1 msec) while power flow is off, and resets to 0 when there is power flow. The timer passes power until the specified interval 'PV' has elapsed.










