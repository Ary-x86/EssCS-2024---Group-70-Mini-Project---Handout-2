# EssCS 2024 - Group 70 Mini Project - Handout 2
**Information about D-70-submission1_AUTOMATIC.dig can be found down below**

**Aryan Swami-Persaud (4490193)** | 
**Mathios Berhe (8356280)**  | 
**Michalis Piripitsis (4497384)** | 
**Sebastiaan Bruinzeel (3925285)**

 <p style=center>Group Practical Assignment (Mini Project)  Group 70 </p>
 <hr>

Our LSFR consists of 8 Registers, from right to left respectively, S7 to S0, with each latch connected to the next registers’ data input.
The feedback values are calculated using XOR gates connected to the outputs of the registers in accordance with the feedback polynomial.

The XOR operation starts with S0⊕S1. The result is passed to the next XOR gate.
This continues, eventually combining the outputs of registers up until and including S7.
The feedback value is calculated and sent to S7.

   Manual Selection Using a Multiplexer:
   A multiplexer is used with a manual SELECT control signal to switch between loading the initial value and the feedback value.
   After every reset, the SELECT signal must be toggled manually.
   This design reduces the gate equivalence value, making it simpler but requires manual intervention.

The output sequence is the value from the latch of Register S0.

The circuit uses Reset and Chip Enable control signals:

   Reset: Allows the LSFR to reset and reinitialize.
   Chip Enable: Allows idle cycles for the LSFR
<hr>
Analysis D-70-submission1.dig:

Gates: 8x REG, 5x XOR-Gate, 1x MUX

Control Signals: SELECT, Chip-Enable, RESET

**Clock Cycles:**

Experiment 1: 1 RESET Cycle for known value + 8 cycles for initialization + 14 cycles for output = 23 cycles

Experiment 2: 1 RESET Cycle for known value + 8 cycles for initialization + 8 output cycles + 5 idle cycles + 8 init cycles + 5 output cycles = 35 cycles
   

**NOTE: We also designed a second circuit which automatically handles the initialization of the LFSR with a counter and comparator. It includes a work-around for bit overflow, and is functionally the same as the circuit we handed in with 1 control signal less. We figured we wouldn't hand this in as our final product since we're not sure if we were allowed to use the counter and comparator, but we figured we'd hand it in as well just in case. The name of this the file of this circuit is: D-70-submission1_AUTOMATIC.dig. More information, explanation and analysis can be found on our github repository: " https://github.com/Ary-x86/EssCS-2024---Group-70-Mini-Project---Handout-2 "**

<hr>

**Information about D-70-submission1_AUTOMATIC.dig**
Our more complex circuit, named D-70-submission1_AUTOMATIC.dig, automates the process by generating the SELECT signal at the appropriate times. From a functional standpoint the design is identical to the manual circuit, but operates with fewer control signals.

In our LSFR the initialization phase happens in 8 cycles consistently. So instead of controlling the RESET and SELECT control signals simultaneously with the SELECT signal for the right amount of bits, we figured it'd be more practical to automate this process, and only keep the RESET and Chip-Enable Signals.

The way this works is that once initialization start, a clock cycle counter outputs the count, a binary number into a comparator. This comparator checks if that value is less than the constant value 8 (because 8 Registers must be initialized). If so, the select signal will be 1, which means that the input signal will be the input to the first register. If the counter is 8 or higher, the signal will be 0, so the feedback value will be the input to the first registers.

Now an issue we encountered was bit overflow. Once the 4-bit counter reached it's max value 0xF, the counter would reset and the SELECT signaL would be 1 again. To mitigate this issue we increased the counter to a 32 bit counter, but this would still cause problems if we needed a big output sequence. 

We decided on handling the case by connecting the overflow wire of the counter to a new Register, which could store the overflow bit until a RESET was triggered. To make it work properly we added some conditional logic; the output of the comparator is connected to an AND gate, and the other input for the end gate is a negated (NOT gate) from this overflow register latch. Now once the latch in the Register is 1, after the NOT gate it's 0, so the comparator value doesn't matter anymore. **(Zero Element Identity Law)**

To ensure this value would be held in the Bit-Overflow Register, we connected the Latch output, through a NOT gate, to the Chip Enable, so it would be off once it holds the overflow bit. The next issue we encountered was that the RESET signal couldn't reset the bit-overflow register, because the CE was off. So we connected the RESET to too the CE input of the register ny using an OR gate. Now the chip enable would be 1 during a RESET, effectively resetting the register. The register works independentely from the rest, so we won't need the global CE signal for idle cycles in the circuit.

<hr>

**Analysis:**

Analysis D-70-submission1_AUTOMATIC.dig:

Gates: 9x REG, 5x XOR-Gate, 1x MUX, 1x AND-gate, 1x OR-gate, 1x NOT-gate, 1x Counter, 1x Comparator

Control Signals: Chip-Enable, RESET

**Clock Cycles:**

Experiment 1: 1 RESET Cycle for known value + 8 cycles for initialization + 14 cycles for output = 23 cycles

Experiment 2: 1 RESET Cycle for known value + 8 cycles for initialization + 8 output cycles + 5 idle cycles + 8 init cycles + 5 output cycles = 35 cycles

<hr>

Conclusion: This cycle trades 1 control signal less for a higher Gate Equivalence value. The assignment states to minimize control signals.
