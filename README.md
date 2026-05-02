Overview
This project implements a CORDIC (Coordinate Rotation Digital Computer)-based sine calculator in Verilog. The module computes the sine of an input angle using an iterative shift-add algorithm, making it highly efficient for FPGA/ASIC implementations where multipliers are expensive.
The design uses a finite state machine (FSM) to control the computation flow and performs fixed-point arithmetic for precision.
________________________________________
 Module Description
Module Name
sine
Functionality
•	Accepts a 10-bit angle input (mapped from 0° to 90°). 
•	Computes the sine of the given angle using the CORDIC rotation method. 
•	Outputs a 10-bit sine value. 
•	Signals completion via a done flag. 
________________________________________
 Interface
Signal Name	Width	Direction	Description
clk	1	Input	System clock
rstn	1	Input	Active-low reset
start	1	Input	Starts computation
angle	10	Input	Input angle (scaled representation)
sine_out	10	Output	Computed sine value
done	1	Output	Indicates computation completion
________________________________________
 Design Details
1. Algorithm
•	Uses CORDIC rotation mode 
•	Iteratively rotates a vector toward the target angle 
•	Uses only: 
o	Add/Subtract operations 
o	Bit shifts 
o	Lookup table (atan values) 
________________________________________
2. Fixed-Point Representation
•	Internal computations use 32-bit signed fixed-point 
•	Angle is scaled using a constant factor (ascale) 
•	Gain compensation is handled using constant K 
________________________________________
3. FSM Architecture
The module operates using a 4-state FSM:
State	Description
IDLE	Waits for start signal
LOAD	Loads initial values
CALCULATE	Performs iterative CORDIC rotations
COMPLETE	Outputs result and asserts done
________________________________________
4. Iterative Computation
•	Total iterations: 15 
•	At each iteration: 
o	Shift operations approximate multiplication/division 
o	Angle (z) is reduced toward zero 
o	Vector (x, y) rotates accordingly 
________________________________________
5. Lookup Table
•	Precomputed arctangent values 
•	Stored in atan_table 
•	Used to update the angle during each iteration 
________________________________________
 Output Scaling
•	Final sine value is derived from y 
•	Right-shift operation adjusts fixed-point precision 
•	Offset added to fit into 10-bit output range 
________________________________________
 Key Features
•	Multiplier-free implementation (hardware efficient) 
•	Fully synchronous design 
•	FSM-controlled operation 
•	Parameterized iteration count 
•	Suitable for FPGA deployment 
________________________________________
 Notes & Considerations
•	Input angle is not in degrees directly, but a scaled representation 
•	Accuracy depends on: 
o	Number of iterations 
o	Fixed-point scaling constants 
•	Currently supports first quadrant (0° to 90°) 
________________________________________
 Possible Improvements
•	Extend to full 360° support using quadrant mapping 
•	Pipeline the design for higher throughput 
•	Parameterize bit-widths for flexibility 
•	Add cosine output (already partially present in comments) 

