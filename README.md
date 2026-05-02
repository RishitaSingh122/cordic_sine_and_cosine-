 Overview
 
This project implements hardware-efficient sine and cosine generators using the CORDIC (Coordinate Rotation Digital Computer) algorithm in Verilog. The design uses iterative shift-add operations and fixed-point arithmetic, making it highly suitable for FPGA and ASIC implementations where multipliers are expensive.
Both sine and cosine are computed using separate modules built on a common CORDIC rotation principle and controlled via a finite state machine (FSM).
________________________________________
 Features
 
•	Multiplier-free architecture (shift-add only) 
•	Fixed-point arithmetic for high precision 
•	FSM-controlled deterministic computation 
•	Parameterized iteration count (default: 15) 
•	Synthesizable and FPGA-friendly design 
•	Separate modules for sine and cosine 
________________________________________
 Modules Included

 
1. sine
•	Computes sine of a given angle using CORDIC 
•	Output derived from the y-component of rotated vector 
2. cordic_cosine
•	Computes cosine of a given angle using CORDIC 
•	Output derived from the x-component of rotated vector 
________________________________________
 Common Interface

 
Signal Name	Width	Direction	Description
clk	1	Input	System clock
rstn	1	Input	Active-low reset
start	1	Input	Starts computation
angle	10	Input	Input angle (scaled representation of 0°–90°)
done	1	Output	Indicates computation completion
________________________________________
 Module-Specific Outputs

 
Module	Output	Description
sine	sine_out [9:0]	Computed sine value
cordic_cosine	cos_out [9:0]	Computed cosine value
________________________________________
 Algorithm Overview

 
The design uses CORDIC in rotation mode, which computes trigonometric functions using only:
•	Add/Subtract operations 
•	Bit shifting 
•	Lookup table (arctangent values) 
At each iteration:
•	A vector is rotated toward the target angle 
•	The residual angle (z) is reduced toward zero 
•	Final vector components give sine and cosine 
________________________________________
 FSM Architecture


 
Both modules follow a 4-state FSM:
State	Description
IDLE	Waits for start signal
LOAD	Loads initial angle and values
CALCULATE	Performs iterative CORDIC rotations
COMPLETE	Outputs result and asserts done
________________________________________
 Fixed-Point Representation
 
Parameter	Description
Internal width	32-bit (sine), 36-bit (cosine)
Iterations	15
Gain constant (K)	Compensates CORDIC scaling
Angle scaling	Converts input angle to fixed-point
________________________________________
 Lookup Table (atan values)
 
•	Precomputed arctangent values 
•	Stored in ROM-style array 
•	Used to update angle at each iteration 
•	Scaled appropriately for fixed-point domain 
________________________________________
 Output Scaling
 
Sine Module
•	Output derived from y 
•	Right-shift scaling applied 
•	Offset added for normalization 
Cosine Module
•	Output derived from x 
•	Right-shift scaling applied 
•	Offset subtraction used for alignment 
________________________________________
 Design Considerations
 
•	Input angle is scaled, not direct degrees 
•	Currently supports first quadrant (0°–90°) only 
•	Scaling constants are tuned for output range 
•	Accuracy depends on: 
o	Number of iterations 
o	Fixed-point precision 
________________________________________
 Possible Improvements
 
•	Extend to full 360° using quadrant mapping 
•	Pipeline architecture for higher throughput 
•	Unified sine-cosine module (shared datapath) 
•	Parameterized bit-width for scalability 
•	Add testbench and waveform validation 
________________________________________
 Suggested Verification
 
•	Compare outputs with MATLAB/Python reference 
•	Sweep full input range (0–1023) 
•	Measure error vs floating-point implementation 
________________________________________
 Conclusion
This project demonstrates an efficient RTL implementation of trigonometric functions using the CORDIC algorithm. It is well-suited for digital design applications requiring low-area and deterministic computation without relying on multipliers.

