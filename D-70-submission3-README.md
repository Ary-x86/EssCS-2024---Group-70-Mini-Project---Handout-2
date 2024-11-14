# EssCS 2024 - Group 70 Mini Project - Handout 2
**Aryan Swami-Persaud (4490193)** | 
**Mathios Berhe (8356280)**  | 
**Michalis Piripitsis (4497384)** | 
**Sebastiaan Bruinzeel (3925285)**
 <p style=center>Group Practical Assignment (Mini Project)  Group 70 </p>
 <hr>
Our LSFR consists of 8 Registers,S7 to S0, with each latch connected to the next register.
The feedback values are calculated using XOR gates.
They are connected to the latch of the registers, as in the feedback polynomial.
The XOR operation starts with S0⊕S1. The result is passed to the next XOR gate.
This continues, eventually combining the outputs of registers up until and including S7.
The feedback value is calculated and sent to S7.
 A multiplexer is used with a manual SELECT control signal 
This is done to switch between loading the initial value and the feedback value.
The output sequence is the value from the latch of Register S0.
The circuit uses Reset and Chip Enable control signals:
   Reset: Allows the LSFR to reset and reinitialize.
   Chip Enable: Allows idle cycles for the LSFR
<hr>
**Analysis D-70-submission1.dig:**
Gates: 8x REG, 5x XOR-Gate, 1x MUX
Control Signals: SELECT, Chip-Enable, RESET
**Clock Cycles:**
Experiment 1: 1 RESET Cycle for known value + 8 cycles for initialization + 14 cycles for output = 23 cycles
Experiment 2: 1 RESET Cycle for known value + 8 cycles for initialization + 8 output cycles + 5 idle cycles + 8 init cycles + 5 output cycles = 35 cycles
   

**NOTE: We also designed a second circuit which automatically handles the initialization of the LFSR with a counter and comparator. It includes a work-around for bit overflow, and is functionally the same as the circuit we handed in with 1 control signal less. We figured we wouldn't hand this in as our final product since we're not sure if we were allowed to use the counter and comparator, but we figured we'd hand it in as well just in case. The name of this the file of this circuit is: D-70-submission1_AUTOMATIC.dig. More information, explanation and analysis can be found on our github repository: " https://github.com/Ary-x86/EssCS-2024---Group-70-Mini-Project---Handout-2 "**
