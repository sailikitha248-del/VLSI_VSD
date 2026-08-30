Module 1 — Introduction to Verilog, Design & Testbench

1. Verilog Simulation

Verilog simulation is used to verify an RTL design according to its specification.

- Simulator: Tool used to simulate a Verilog design.
- Icarus Verilog ("iverilog"): Example Verilog simulator.
- Design: Verilog code describing the required hardware.
- Testbench: Code used to apply inputs and verify the design outputs.

---

2. Working of Simulator

A simulator monitors changes in signals.

- Input changes cause the design to be evaluated.
- Outputs change according to the design logic.
- Signal changes are recorded with respect to simulation time.

---

3. Testbench

A testbench provides inputs to the DUT (Design Under Test) and observes its outputs.

Stimulus Generator → DUT/Design → Stimulus Observer
      Inputs                       Outputs

- Design has primary inputs and outputs.
- Testbench generates inputs.
- Testbench observes outputs.
- Testbench normally has no primary inputs or outputs.

---

4. Simulation Flow

Design + Testbench
       ↓
    iverilog
       ↓
   Simulation
       ↓
      VCD
       ↓
   GTKWave
       ↓
   Waveforms
       ↓
Verify Functionality

Main Steps

1. Write the design.
2. Write the testbench.
3. Compile using "iverilog".
4. Run the simulation.
5. Generate the VCD file.
6. Open VCD in GTKWave.
7. Check waveforms and verify functionality.

---

5. Important Commands

iverilog good-mux.v tb-good-mux.v
./a.out
gtkwave tb-good-mux.vcd
gvim tb-good-mux.v

- "iverilog" → Compiles Verilog design and testbench.
- "./a.out" → Runs the simulation.
- "gtkwave" → Displays waveforms.
- "gvim" → Opens/edits Verilog files.

---

6. Testbench Structure

A testbench instantiates the DUT/UUT.

module tb_good_mux;

    reg sel, i0, i1;
    wire y;

    good_mux uut (
        .sel(sel),
        .i0(i0),
        .i1(i1),
        .y(y)
    );

endmodule

- "reg" → Used for testbench-driven inputs.
- "wire" → Used for DUT outputs.
- "uut" → Unit Under Test.
- "dut" → Design Under Test.

---
<img width="1919" height="978" alt="Screenshot 2026-08-22 221902" src="https://github.com/user-attachments/assets/63bac529-b1da-4182-ad94-6725d5868923" />

7. VCD — Value Change Dump

VCD stores signal value changes during simulation.

initial begin
    $dumpfile("tb_good_mux.vcd");
    $dumpvars(0, tb_good_mux);
end

Command| Function
"$dumpfile"| Specifies VCD file name
"$dumpvars"| Records selected signals
"$finish"| Ends simulation

The VCD file is viewed using GTKWave.

---

8. Initial Stimulus

Initial values are assigned using an "initial" block.

initial begin
    sel = 0;
    i0  = 0;
    i1  = 0;

    #300 $finish;
end

- "#300" → Delay of 300 simulation time units.
- "$finish" → Terminates simulation.

---

9. Periodic Stimulus

The "always" block can continuously change inputs.

always #75 sel = ~sel;
always #10 i0  = ~i0;
always #15 i1  = ~i1;

- "sel" toggles every 75 time units.
- "i0" toggles every 10 time units.
- "i1" toggles every 15 time units.
- These blocks act as the stimulus generator.

---

10. Important Terms

Term| Meaning
Design| Verilog RTL hardware description
Testbench| Code used to test the design
DUT/UUT| Design/Unit Under Test
Stimulus Generator| Generates input signals
Stimulus Observer| Observes output signals
iverilog| Verilog simulator/compiler
VCD| Stores signal value changes
GTKWave| Displays simulation waveforms
"initial"| Executes a block once
"always"| Repeatedly executes a block
"#delay"| Simulation time delay
"$dumpfile"| Creates/specifies VCD file
"$dumpvars"| Specifies signals to record
"$finish"| Stops simulation

---
<img width="1919" height="979" alt="Screenshot 2026-08-22 231927" src="https://github.com/user-attachments/assets/3b199412-a3ce-49a6-a68a-18afb0b119df" />


Verilog simulation is used to verify the functionality of an RTL design. A testbench generates input stimulus and observes the outputs of the DUT. The design and testbench are compiled using Icarus Verilog, and the simulation produces a VCD file containing signal changes. The VCD file is opened in GTKWave to observe waveforms and verify whether the design works correctly.

Key Flow

Write Design → Write Testbench → Generate Stimulus → Compile → Simulate → Generate VCD → View in GTKWave → Verify Functionality

  Introduction to Yosys, Logic Synthesis & Standard Cell Selection
1. Introduction to Yosys

Yosys is an open-source RTL synthesis framework.

It converts:

RTL Verilog → Gate-Level Netlist

The netlist is created using cells from a standard cell library.

---

 Synthesizer

A synthesizer converts RTL code into a gate-level netlist.

Inputs

- RTL Verilog
- Standard cell library (".lib")

Output

- Synthesized netlist

Yosys is the synthesis tool used here.

---
Yosys Synthesis Flow

RTL Design
    ↓
read_verilog
    ↓
Yosys
    ↓
Synthesis + Optimization
    ↓
Technology Mapping
    ↓
Gate-Level Netlist
    ↓
write_verilog

The ".lib" file provides the available standard cells.

---

 Important Yosys Commands

Command| Purpose
"read_verilog design.v"| Read RTL Verilog
"read_liberty library.lib"| Read standard cell library
"synth"| Perform synthesis
Technology mapping| Map logic to library cells
"write_verilog netlist.v"| Generate netlist

---
 Netlist

A netlist is a gate-level representation of a circuit.

RTL → Synthesis → Gate-Level Netlist

Example:

Y = A & B

A ──┐
    AND ── Y
B ──┘

---
Verification of Synthesis

The synthesized netlist must be verified to ensure that it performs the same function as the RTL.

Netlist + Testbench
        ↓
   Icarus Verilog
        ↓
       VCD
        ↓
    GTKWave
        ↓
    Waveforms

Icarus Verilog

Icarus Verilog is a Verilog simulator.

It is used to simulate:

Synthesized Netlist + Testbench

The simulation produces a VCD file.

8. VCD File

VCD = Value Change Dump

It stores signal value changes during simulation.

Netlist + Testbench
        ↓
Icarus Verilog
        ↓
VCD File

 GTKWave

GTKWave is a waveform viewer.

It displays the signals stored in the VCD file.

VCD → GTKWave → Waveforms

The waveform is used to verify circuit functionality.


---
 Logic Synthesis

Logic synthesis converts RTL into a gate-level implementation.

RTL
 ↓
Optimization
 ↓
Gate-Level Logic
 ↓
Technology Mapping
 ↓
Standard Cell Netlist

Synthesis considers:

- Area
- Timing
- Power
- Logic optimization

---
RTL Design

RTL = Register Transfer Level

RTL describes the behavior and data transfer of a digital circuit.

It can contain:

- Registers
- Combinational logic
- Sequential logic
- Data operations

RTL is commonly written in Verilog/SystemVerilog.

---
RTL Code Example

module sample (
    input clk,
    input rst,
    output reg result
);

always @(posedge clk) begin
    if (rst)
        result <= 1'b0;
    else
        result <= 1'b1;
end

endmodule

This describes sequential logic controlled by a clock and reset.

 Digital Logic Circuit

A digital circuit may contain:

- Primary inputs
- Primary outputs
- Combinational logic
- Sequential elements

Synthesis converts this RTL description into standard-cell-based hardware.

 What Happens During Synthesis?

Synthesis performs:

1. RTL analysis
2. Logic optimization
3. Gate-level conversion
4. Technology mapping
5. Cell selection
6. Netlist generation

RTL → Optimized Logic → Standard Cells → Netlist

 RTL-to-Gate-Level Translation

The behavioral RTL is converted into physical logic structures.

Behavioral RTL
      ↓
Logic Optimization
      ↓
Gate-Level Logic
      ↓
Technology Mapping
      ↓
Standard Cell Netlist

 lib File

A ".lib" file is a standard cell library file.

It contains information such as:

- Cell names
- Logic function
- Area
- Timing
- Power
- Input capacitance
- Output capacitance
- Rise/fall delay
- Drive strength

It is read using:

read_liberty library.lib

---

18. Standard Cell Library

A standard cell library is a collection of predefined and characterized cells.

Examples:

- AND
- OR
- NAND
- NOR
- NOT
- XOR
- MUX
- Buffer
- Flip-flop

The library provides different versions of cells based on speed, area and drive strength.

---

 Different Types of the Same Gate

The library may contain different input and drive-strength versions.

Example:

2-input AND
3-input AND
4-input AND

Each may have:

Slow → Medium → Fast

 Combinational Delay and Maximum Speed

Combinational delay determines how quickly data travels between sequential elements.

DFF → Combinational Logic → DFF

Lower combinational delay:

Lower Delay → Higher Maximum Frequency

Higher delay reduces the maximum operating speed.

---

 Propagation Delay

Propagation delay is the time taken for a signal to travel from a cell input to its output.

Lower Delay → Higher Speed
Higher Delay → Lower Speed

 Setup Time

Setup time is the minimum time data must be stable before the active clock edge.

Simplified condition:

Tclk ≥ Tcq + Tcomb + Tsetup

Where:

- "Tclk" = Clock period
- "Tcq" = Clock-to-Q delay
- "Tcomb" = Combinational delay
- "Tsetup" = Setup time

 Hold Time

Hold time is the minimum time data must remain stable after the clock edge.

Simplified condition:

Tcq(min) + Tcomb(min) ≥ Thold

 Faster Cells vs Slower Cells

Feature| Faster Cell| Slower Cell
Delay| Low| High
Speed| High| Low
Drive Strength| High| Low
Area| Usually higher| Usually lower
Power| Usually higher| Usually lower
Main Use| Setup-critical paths| Hold-critical paths

Faster cells trade higher area/power for lower delay.



Cell Selection Guidelines

General guidelines:

- Use faster cells for setup-critical paths.
- Use slower cells when additional delay is required.
- Use smaller cells where timing allows.
- Consider load and capacitance.
- Balance timing, area and power.
- Avoid unnecessarily using fast cells everywhere.

---

 Technology Mapping

Technology mapping converts optimized logic into cells available in the selected standard cell library.

Optimized Logic
      ↓
Technology Mapping
      ↓
Library Cells
      ↓
Mapped Netlist

The final circuit must use cells that actually exist in the target ".lib".

---
 Complete RTL-to-Netlist Flow

RTL Verilog
    ↓
read_verilog
    ↓
RTL Analysis
    ↓
Logic Optimization
    ↓
Technology Mapping
    ↓
Standard Cell Selection
    ↓
Gate-Level Netlist
    ↓
write_verilog

---

 Complete Netlist Verification Flow

Synthesized Netlist
        +
    Testbench
        ↓
Icarus Verilog
        ↓
Simulation
        ↓
VCD File
        ↓
GTKWave
        ↓
Waveform Analysis
        ↓
Functional Verification

The synthesized netlist should produce the expected functionality.

---

 Yosys Command Summary

read_verilog design.v
read_liberty library.lib
synth
write_verilog netlist.v

Meaning

Command| Function
"read_verilog"| Reads RTL
"read_liberty"| Reads standard cell library
"synth"| Performs synthesis
Technology mapping| Maps logic to library cells
"write_verilog"| Writes synthesized netlist

---

 Important Timing Equations

Setup

Tclk ≥ Tcq + Tcomb + Tsetup

Hold

Tcq(min) + Tcomb(min) ≥ Thold

Maximum Frequency

Approximately:

Fmax ≈ 1 / Tclk(min)




 Final Concept

The complete concept can be remembered as:

       RTL DESIGN
           ↓
       YOSYS
           ↓
   Logic Optimization
           ↓
   Technology Mapping
           ↓
   Standard Cell Selection
           ↓
   GATE-LEVEL NETLIST
           ↓
   Icarus Verilog
           ↓
        VCD
           ↓
      GTKWave
           ↓
     Verification

Yosys converts RTL Verilog into a gate-level netlist using cells from a standard cell ".lib". During synthesis, logic is optimized and mapped to suitable cells considering timing, area, power and load. The synthesized netlist is then simulated using Icarus Verilog, and its VCD waveform is viewed in GTKWave to verify that the synthesized design preserves the original RTL functionality.

# MODULE 3 – LOGIC OPTIMIZATION AND SYNTHESIS

## 1. Introduction to Logic Optimization

Logic optimization is the process of simplifying a digital circuit while maintaining its original functionality.

The main objectives of logic optimization are:

- Reduce circuit area
- Reduce power consumption
- Reduce propagation delay
- Improve timing performance
- Improve circuit speed
- Remove redundant logic
- Reduce the number of gates and cells
- Improve overall hardware efficiency

The optimized circuit should produce the same logical output as the original RTL design.

---

# 2. Types of Logic Optimization

Logic optimization can be broadly classified into:

1. Combinational Logic Optimization
2. Sequential Logic Optimization
3. Technology Mapping
4. Physical/Floorplan-Aware Optimization

---

# 3. Combinational Logic Optimization

Combinational logic optimization simplifies logic circuits without changing their functionality.

Examples include:

- Boolean simplification
- Constant propagation
- Removal of redundant logic
- Logic restructuring
- MUX optimization
- Gate minimization

### Example

Original:

```verilog
assign y = a & 1'b1;
```

After optimization:

```verilog
assign y = a;
```

The AND gate is unnecessary because:

```text
A AND 1 = A
```

---

# 4. Boolean Logic Optimization

Boolean identities can be used to simplify logic expressions.

Important Boolean identities include:

```text
A + 0 = A
A + 1 = 1
A · 0 = 0
A · 1 = A

A + A = A
A · A = A

A + A' = 1
A · A' = 0

(A')' = A
```

### Example

Consider:

```text
Y = A' + AB + ABC
```

The expression can be simplified using Boolean algebra.

The optimized logic requires fewer gates while producing the same output.

---

# 5. Constant Propagation

Constant propagation is an optimization technique where signals with known constant values are replaced by those values.

Examples:

```text
A AND 1 = A
A AND 0 = 0
A OR 1  = 1
A OR 0  = A
A XOR 0 = A
A XOR 1 = A'
```

### Advantages

- Reduces gate count
- Reduces area
- Reduces power
- Improves timing
- Removes unnecessary logic

---

# 6. Redundant Logic Removal

Redundant logic is logic that does not affect the final output.

The synthesis tool identifies and removes unnecessary:

- Gates
- Wires
- Logic expressions
- Registers
- MUXes

This produces a smaller and more efficient circuit.

---

# 7. MUX Optimization

A multiplexer selects one input according to a select signal.

For a 2:1 MUX:

```text
sel = 0 → y = i0
sel = 1 → y = i1
```

Verilog:

```verilog
assign y = sel ? i1 : i0;
```

The synthesis tool can simplify a MUX when:

- Select signal is constant
- One input is constant
- One input is redundant
- Logic around the MUX can be simplified

---

# 8. Sequential Logic Optimization

Sequential logic contains storage elements such as:

- Flip-flops
- Registers
- Counters
- FSMs

Sequential optimization attempts to reduce hardware while maintaining the same sequential behavior.

Techniques include:

- Constant propagation
- Register optimization
- State optimization
- Retiming
- Sequential cloning
- Reset/set optimization

---

# 9. Sequential Constant Propagation

Constant values can propagate through sequential logic when the behavior of the register is known.

For example:

```verilog
always @(posedge clk)
    q <= 1'b0;
```

The output of the register is always zero.

Therefore, unnecessary logic associated with the register may be removed during optimization.

---

# 10. State Optimization

State optimization is used in Finite State Machines (FSMs).

It can reduce:

- Number of states
- Number of flip-flops
- Combinational logic
- Area
- Power

Equivalent states can be merged when they produce the same required behavior.

---

# 11. Retiming

Retiming is an optimization technique in which registers are moved across combinational logic while maintaining the same overall functionality.

### Before

```text
FF → Logic → Logic → FF
```

### After

```text
FF → Logic → FF → Logic
```

The purpose of retiming is to balance combinational paths and improve timing.

### Advantages

- Reduces critical path delay
- Improves maximum operating frequency
- Balances logic paths
- Improves timing performance

---

# 12. Sequential Cloning

Sequential cloning duplicates sequential elements such as registers or flip-flops.

It is mainly used to reduce large fanout and improve timing.

### Advantages

- Reduces fanout
- Reduces routing delay
- Improves timing
- Helps physical implementation
- Can reduce congestion

---

# 13. Counter Optimization

Counters contain flip-flops and combinational next-state logic.

Optimization can reduce:

- Number of gates
- Combinational logic
- Power consumption
- Area
- Propagation delay

The optimized counter must maintain the same counting sequence as the original design.

---
<img width="1917" height="921" alt="Screenshot 2026-08-24 161940" src="https://github.com/user-attachments/assets/807fb4a1-9b00-4faf-a02f-16628304fbda" />



# 14. Power Optimization

Power consumption can be reduced by:

- Reducing switching activity
- Reducing capacitance
- Reducing the number of cells
- Reducing unnecessary transitions
- Using appropriate standard cells

Reducing unnecessary logic generally reduces dynamic power consumption.

---

# 15. Timing Optimization

Timing optimization attempts to reduce the delay between input and output.

Important timing parameters include:

- Propagation delay
- Setup time
- Hold time
- Clock period
- Critical path delay


# 16. Standard Cells

Standard cells are pre-designed logic cells used during technology mapping.

Examples include:

- INV
- BUF
- AND
- OR
- NAND
- NOR
- XOR
- XNOR
- MUX
- D Flip-Flop

A standard-cell library contains information about these cells.

---

# 17. Liberty `.lib` File

The Liberty file describes the characteristics of standard cells.

It contains information such as:

- Cell names
- Input pins
- Output pins
- Logic functions
- Timing information
- Power information
- Area
- Delay
- Setup time
- Hold time
- Drive strength
- Input capacitance

---

# 18. Standard Cell Variants

The same logic function can be available in different cell sizes or drive strengths.

For example:

```text
AND2_X1
AND2_X2
AND2_X4
```

Higher drive-strength cells can drive larger loads and may be faster, but they generally consume more area and power.

---

# 19. Fast and Slow Cells

### Fast Cells

Advantages:

- Lower delay
- Better timing
- Higher drive strength

Disadvantages:

- More area
- Higher power

### Slow/Small Cells

Advantages:

- Smaller area
- Lower power

Disadvantages:

- Higher delay
- Lower drive strength

Therefore, synthesis involves an area-power-timing trade-off.

---

# 20. Technology Mapping

Technology mapping converts an optimized Boolean network into cells available in a particular technology library.

```text
Optimized Logic
      ↓
Technology Mapping
      ↓
Standard Cells
      ↓
Gate-Level Netlist
```

---

# 21. ABC Technology Mapping

ABC is a logic synthesis and technology mapping tool.

It can perform:

- Boolean optimization
- Logic restructuring
- Technology mapping
- Gate optimization
- Network optimization

Typical flow:

```text
RTL
 ↓
Yosys
 ↓
Logic Optimization
 ↓
ABC
 ↓
Technology Mapping
 ↓
Gate-Level Netlist
```

---



# 22. Important Yosys Commands

## Read Verilog

```text
read_verilog design.v
```

Reads the Verilog RTL design.

---

## Read Liberty

```text
read_liberty -lib library.lib
```

Reads the standard-cell library information.

---

## Synthesis

```text
synth
```

Performs RTL synthesis.

For a particular top module:

```text
synth -top top_module
```

---

## Process Optimization

```text
proc
```

Converts behavioral processes into logic.

---

## General Optimization

```text
opt
```

Performs general logic optimization.

---

## FSM Optimization

```text
fsm
```

Performs FSM extraction and optimization.

---

## Memory Optimization

```text
memory
```

Processes and optimizes memory structures.

---

## Technology Mapping

```text
techmap
```

Performs technology-independent or technology mapping transformations.

---

## DFF Mapping

```text
dfflibmap -liberty library.lib
```

Maps flip-flops to cells available in the Liberty library.

---

## ABC Mapping

```text
abc -liberty library.lib
```

Uses ABC for technology mapping using the specified Liberty library.

---

## Show Design

```text
show
```

Displays the synthesized logic graph.

---


# 23. Basic Yosys Synthesis Flow

```text
read_verilog design.v
hierarchy -top top_module
proc
opt
fsm
memory
techmap
opt
dfflibmap -liberty library.lib
abc -liberty library.lib
clean
stat
write_verilog netlist.v
```

---

# 27. RTL-to-Gate-Level Flow

```text
RTL Verilog
     ↓
Read RTL
     ↓
Elaboration
     ↓
Logic Optimization
     ↓
FSM / Memory Optimization
     ↓
Technology Mapping
     ↓
Standard Cell Mapping
     ↓
Gate-Level Netlist
```

---



# 24. Module 3 Overall Flow

```text
RTL Design
     ↓
Logic Optimization
     ↓
Combinational Optimization
     ↓
Sequential Optimization
     ↓
Constant Propagation
     ↓
State Optimization
     ↓
Retiming / Sequential Cloning
     ↓
ABC Technology Mapping
     ↓
Standard Cell Mapping
     ↓
Gate-Level Netlist
     ↓
Area / Power / Timing Analysis
```

---

# MODULE 4 – INTRODUCTION TO GATE LEVEL SIMULATION (GLS)

## 1. Introduction to Gate Level Simulation

Gate Level Simulation (GLS) is the simulation of a synthesized gate-level netlist.

Instead of simulating the original RTL description, GLS simulates the actual gate-level implementation produced after synthesis.

```text
RTL Design
     ↓
Synthesis
     ↓
Gate-Level Netlist
     ↓
Gate-Level Simulation
     ↓
Waveform Analysis
```

---

# 2. Why Do We Run GLS?

GLS is performed to:

- Verify logical correctness after synthesis
- Detect synthesis simulation mismatches
- Verify the synthesized netlist
- Check gate-level connectivity
- Compare RTL and synthesized behavior
- Verify timing behavior when delays are included
- Check whether synthesis has preserved the intended functionality

---

# 3. Synthesis Simulation Mismatch

A synthesis simulation mismatch occurs when the behavior observed in RTL simulation is different from the behavior observed after synthesis.

Ideally:

```text
RTL Simulation = Gate-Level Simulation
```

for functional behavior.

If they differ, the RTL coding style or synthesis assumptions must be investigated.

---

# 4. Common Causes of Synthesis Simulation Mismatch

Important causes include:

1. Missing sensitivity list
2. Blocking vs non-blocking assignments
3. Non-standard Verilog coding
4. Incomplete combinational assignments
5. Unintended latch inference
6. Incorrect reset behavior
7. Incorrect initialization
8. Unsupported or synthesis-dependent constructs
9. Simulation-only constructs
10. Differences between RTL simulation semantics and synthesized hardware

---

# 5. Gate-Level Verilog Model

A gate-level Verilog model describes the circuit using gates or standard cells.

Common gate primitives include:

```text
and
or
not
nand
nor
xor
xnor
buf
```

Example:

```verilog
wire w1;

and u1(w1, a, b);
or  u2(y, w1, c);
```

This represents:

```text
a ----\
       AND ----\
b ----/         \
                OR ---- y
c --------------/
```

---

# 6. Example Design

Consider the logic:

```verilog
assign y = (a & b) | c;
```

The equivalent gate-level structure is:

```text
a ─────┐
       │
       AND ─────┐
b ─────┘        │
                OR ───── y
c ──────────────┘
```

Gate-level Verilog:

```verilog
module design (
    input a,
    input b,
    input c,
    output y
);

wire w1;

and u_and(w1, a, b);
or  u_or(y, w1, c);

endmodule
```

---

# 7. Gate-Level Verilog Model and Timing

Gate-level Verilog models can represent:

### Functional Behavior

Only the logical behavior of gates is considered.

```text
Input → Logic → Output
```

### Timing Behavior

Gate delays are also considered.

```text
Input
  ↓
Gate Delay
  ↓
Output
```

Therefore:

```text
Gate-Level Simulation
        ↓
Functional GLS
        +
Timing GLS
```

---

# 8. Functional GLS

Functional GLS verifies whether the synthesized gate-level netlist produces the correct logical outputs.

It mainly checks:

- Logic functionality
- Gate connectivity
- Synthesis correctness
- RTL-to-netlist equivalence by simulation

```text
Gate-Level Netlist
       +
Testbench
       ↓
Functional GLS
       ↓
Logical Verification
```

---

# 9. Timing GLS

Timing GLS includes delay information associated with gates or standard cells.

It is used to observe:

- Propagation delay
- Signal transition timing
- Setup violations
- Hold violations
- Timing-related behavior

```text
Gate-Level Netlist
       +
Cell Library
       +
Delay Information
       +
Testbench
       ↓
Timing GLS
```

---

# 10. Propagation Delay

Propagation delay is the time between a change at the input of a gate and the corresponding change at its output.

```text
Input changes
     ↓
Gate delay
     ↓
Output changes
```

In timing GLS, these delays can be observed in the waveform.

---

# 11. Missing Sensitivity List

A sensitivity list determines when an `always` block is executed.

Example:

```verilog
always @(a)
begin
    y = a & b;
end
```

Here only `a` is included.

If `b` changes while `a` remains unchanged, the `always` block may not execute in simulation.

This can produce a simulation mismatch.

---

# 12. Correct Sensitivity List

Use:

```verilog
always @(*)
begin
    y = a & b;
end
```

The `@(*)` automatically includes all signals read by the block.

This is preferred for combinational logic.

---

# 13. Blocking Assignment

Blocking assignment uses:

```verilog
=
```

Example:

```verilog
always @(*) begin
    q0 = d;
    q  = q0;
end
```

Blocking assignments execute in the order in which they are written.

```text
First statement
      ↓
Second statement
      ↓
Third statement
```

Therefore, the first statement is evaluated before the next statement.

Blocking assignments are generally used for combinational logic.

---

# 14. Non-Blocking Assignment

Non-blocking assignment uses:

```verilog
<=
```

Example:

```verilog
always @(posedge clk) begin
    q <= d;
end
```

Non-blocking assignments schedule updates rather than immediately changing the left-hand side.

They are generally used for sequential logic such as flip-flops.

---

# 15. Blocking vs Non-Blocking

| Feature | Blocking `=` | Non-Blocking `<=` |
|---|---|---|
| Common application | Combinational logic | Sequential logic |
| Execution | Sequential | Scheduled |
| Typical block | `always @(*)` | `always @(posedge clk)` |
| Typical use | Calculate intermediate values | Model flip-flops/registers |

---

# 16. Important Blocking Assignment Example

Consider:

```verilog
q0 = d;
q  = q0;
```

With blocking assignment:

```text
q0 gets d
    ↓
q gets new q0
    ↓
q gets d
```

The order of blocking assignments can affect the simulation result.

Therefore, blocking assignments must be used carefully in sequential logic.

---

# 17. Non-Blocking Sequential Example

```verilog
always @(posedge clk) begin
    q0 <= d;
    q  <= q0;
end
```

At the same clock edge:

```text
q0 gets old d
q gets old q0
```

The two assignments are scheduled together.

This behavior correctly models multiple flip-flops operating on the same clock edge.

---

# 18. Important Caveat

Blocking assignments and non-blocking assignments should be selected according to the type of hardware being described.

General rule:

```text
Combinational Logic → Blocking (=)

Sequential Logic → Non-Blocking (<=)
```

Incorrect usage can result in RTL simulation behavior that does not match the intended synthesized hardware behavior.

---

# 19. Incomplete Combinational Assignment

Example:

```verilog
always @(*) begin
    if (sel)
        y = a;
end
```

When:

```text
sel = 1 → y = a
sel = 0 → y is not assigned
```

The previous value of `y` may need to be retained.

This can cause latch inference.

---

# 20. Avoiding Unintended Latches

A complete combinational block should assign an output under every condition.


---

# 21. Ternary Operator

The ternary operator is commonly used to describe a multiplexer.

Syntax:

```verilog
condition ? true_value : false_value
```


# 22. 2:1 Multiplexer

A 2:1 MUX has:

- Two data inputs
- One select input
- One output

```text
             ┌─────────┐
i0 ─────────►│         │
             │   MUX   ├────► y
i1 ─────────►│         │
             └────┬────┘
                  │
                 sel
```

Truth table:

| Select | Output |
|---|---|
| 0 | i0 |
| 1 | i1 |

Boolean expression:

```text
Y = sel'·i0 + sel·i1
```

---

# 23. Ternary Operator MUX Verilog

```verilog
module ternary_operator_mux (
    input  i0,
    input  i1,
    input  sel,
    output y
);

assign y = sel ? i1 : i0;

endmodule
```

---

# 24. Testbench

A testbench is used to provide input stimulus to the Design Under Test (DUT).

A testbench generally contains:

- DUT instantiation
- Input generation
- Output observation
- Simulation control
- VCD generation


```

---
<img width="1917" height="946" alt="Screenshot 2026-08-24 174709" src="https://github.com/user-attachments/assets/9e403811-15b7-463f-b279-876163df552f" />


---

#  Icarus Verilog

Icarus Verilog is an open-source Verilog simulator.

Basic simulation flow:

```text
Verilog Design
      +
Testbench
      ↓
Icarus Verilog
      ↓
VCD File

# VSDBabySoC Pre-Synthesis Simulation

## Introduction

**VSDBabySoC** is a small System-on-Chip based on a **RISC-V processor**. It integrates digital processing with **PLL** and **DAC** blocks.

Pre-synthesis simulation is performed to verify the functionality of the RTL design before synthesis.

## Main Blocks

- **RISC-V CPU** – Executes instructions and generates digital data.
- **PLL** – Generates the required clock signal.
- **DAC** – Converts the 10-bit digital data into an analog output.

## Important Signals

| Signal | Description |
|---|---|
| `CLK` | Main clock |
| `reset` | Reset signal |
| `ENb_CP` | Charge pump enable |
| `ENb_VCO` | VCO enable |
| `VCO_IN` | VCO input |
| `VCO_OUT` | VCO output |
| `VREFH` | Reference voltage |
| `RV_TO_DAC[9:0]` | 10-bit DAC input |

## Waveform Observation

The GTKWave pre-synthesis waveform shows:

- Continuous clock switching.
- Proper reset and control signals.
- Changing `RV_TO_DAC[9:0]` values.
- Individual DAC bits switching at different rates.
- A periodic output waveform generated from the DAC data.

## Simulation Flow

```text
VSDBabySoC RTL
      ↓
Testbench
      ↓
Pre-Synthesis Simulation
      ↓
GTKWave
      ↓
Waveform Verification
      ↓
Logic Synthesis


      ↓
GTKWave
```
<img width="1919" height="1079" alt="Screenshot 2026-08-22 152115" src="https://github.com/user-attachments/assets/d6634ce1-8de2-4e32-8d81-b44457c9554d" />
## Conclusion

The pre-synthesis simulation verifies the functional behavior of **VSDBabySoC** at the RTL level. The observed clock, control signals, and `RV_TO_DAC[9:0]` waveform indicate that the design is functioning as expected before proceeding to synthesis.
