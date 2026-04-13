# Weight Divider: FPGA and Arduino Based Smart Sorting System

This repository contains the documentation and source code architecture for the **Weight Divider** project, developed as the final term project for the **EEE 102: Introduction to Digital Circuit Design** course at Bilkent University.

The system is designed to measure the weight of an object and physically sort it (pushing it left or right) using servo motors, based on a user-defined threshold weight.

##  Author
* **Emre Oral** - Bilkent University

##  Project Overview
The project successfully bridges the gap between analog sensor reading and digital logic control. The system operates in two main phases:
1. **Measurement (Arduino):** An Arduino Uno reads analog signals from a load cell via an HX711 amplifier, calibrates the data, and converts it into a digital weight value.
2. **Classification & Actuation (FPGA):** A Basys3 FPGA receives this digital weight and compares it against a predetermined threshold using custom-written VHDL logic. Depending on the comparison result (greater than or less than), the FPGA generates specific PWM signals to trigger the corresponding servo motor, physically sorting the item.

##  Hardware Components
* **FPGA Board:** Digilent Basys3 (Xilinx Artix-7)
* **Microcontroller:** Arduino Uno
* **Sensor:** Load Cell + HX711 Weight Sensor Amplifier
* **Actuators:** 2x Micro Servo Motors
* **Miscellaneous:** Jumper wires, custom physical sorting platform

##  Software & VHDL Architecture
The digital logic was implemented entirely in VHDL without relying on pre-built IP cores. The architecture consists of several interconnected modules:

* `Fourbitcomparator.vhd`: A custom 4-bit binary comparator built from scratch using gate-level logic (`XNOR`, `AND`, `OR`). It compares the measured weight ($A$) with the threshold weight ($B$) and outputs signals for $A > B$, $A < B$, or $A = B$.
* `servoclock.vhd`: A frequency divider that steps down the Basys3 board's onboard clock to generate a specific 64kHz timing signal required for servo operation.
* `servosignal.vhd`: A PWM (Pulse Width Modulation) generator that dictates the angular position of the servo motors (e.g., 90-degree rotation) based on the comparator's output.
* `top.vhd`: The top-level module that maps the comparator and dual servo driver components together, routing the logic signals to the physical GPIO pins connected to the servos.

##  Key Accomplishments
* **Analog-to-Digital Integration:** Successfully established reliable communication between a microcontroller handling analog sensor data and an FPGA handling strict digital logic.
* **Custom PWM Generation:** Implemented precise servo motor control solely through VHDL state machines and counters.
* **Gate-Level Logic Design:** Applied theoretical digital design concepts to create a functioning 4-bit comparator using fundamental logic gates.

## 📂 Project Structure
* `Report/Emre_Oral_EE102_TermProject.pdf`: Detailed project report including block diagrams, logic schematics, and test results.
* `VHDL_Src/`: Contains all VHDL source files (`top.vhd`, comparator, and servo controllers).
* `Arduino_Src/`: Contains the C++ code for the HX711 sensor calibration and reading.
