# Logic Optimization

## Module 3 – Logic Optimizations

This module covers **combinational and sequential logic optimization techniques** used in digital logic design and VLSI synthesis.

---

# 1. Combinational Logic Optimization

Combinational logic optimization is the process of simplifying a combinational circuit while maintaining the same logical functionality.

### Objectives

* Reduce area
* Reduce power consumption
* Reduce propagation delay
* Reduce the number of logic gates
* Improve circuit performance
* Obtain an optimized design

### Major Techniques

* Boolean simplification
* K-map reduction
* Constant propagation
* Logic restructuring
* Redundant logic removal
* Multiplexer-based optimization

---

# 2. Constant Propagation

Constant propagation is an optimization technique in which a signal having a constant value (`0` or `1`) is propagated through the logic circuit.

## Example

Consider:

```text
Y = (A.B) + C
```

If:

```text
A = 0
```

then:

```text
Y = (0.B) + C
  = 0 + C
  = C
```

Therefore:

```text
Y = C
```

The unnecessary AND gate can be removed.

---

## Example: Inverter

If:

```text
Y = C'
```

the circuit can be implemented using only an inverter.

```text
C ───► NOT ───► Y
```

---

# 3. Boolean Logic Optimization

Boolean algebra can be used to simplify logic expressions and reduce the number of gates.

## Example

Consider:

```text
Y = A' + AB + ABC
```

Factor `A`:

```text
Y = A' + A(B + BC)
```

Using:

```text
B + BC = B
```

we get:

```text
Y = A' + AB
```

Using:

```text
A' + AB = A' + B
```

Therefore:

```text
Y = A' + B
```

The optimized expression requires fewer logic gates.

---

# 4. Sequential Logic Optimization

Sequential logic optimization is the process of optimizing circuits that contain memory elements.

Examples include:

* Flip-flops
* Registers
* Counters
* Finite State Machines (FSMs)

Sequential circuits depend on:

* Present inputs
* Previous state
* Clock
* Reset/set conditions

---

# 5. Types of Sequential Logic Optimization

## 5.1 Basic Sequential Constant Propagation

Sequential constant propagation identifies registers or signals whose values become constant because of the circuit conditions.

## 5.2 Advanced Sequential Optimization

Advanced techniques include:

* State optimization
* Retiming
* Sequential logic cloning
* Floorplan-aware sequential optimization

---


# 6. Sequential Constant Propagation



Consider a D flip-flop:

```text
             ┌─────────┐
D ──────────►│    D    │────► Q
             │   FF    │
CLK ────────►│         │
             └─────────┘
```

If:

```text
D = 0
```

then after the required clock condition:

```text
Q = 0
```

Therefore, `Q` can be treated as a constant.

---



# 7. Sequential Constant Propagation with Reset

A reset can force the output of a flip-flop to a known value.

For an active reset:

```text
RST → Q = 0
```

Therefore:

```text
Q = 0
```

regardless of the previous state.

### Example

Consider:

```text
D = 0
```

with a resettable D flip-flop.

When reset is applied:

```text
Q = 0
```

When the clock is applied and reset is inactive:

```text
Q = D
```

Since:

```text
D = 0
```

we obtain:

```text
Q = 0
```

Thus the sequential logic can potentially be optimized to a constant.

---

# 8. Set-Based Sequential Optimization

A set input can force a flip-flop output to logic `1`.

When set is applied:

```text
Q = 1
```

When set is not applied, the flip-flop operates according to its normal clock/data behavior.

Therefore, if the output is always forced to `1` under the required operating conditions, the synthesis tool can propagate this constant value.

---

# 9. Sequential Constant Propagation Example

Consider:

```text
          ┌─────────┐
D = 0 ───►│ D     Q │──────┐
          │   DFF   │      │
CLK ─────►│         │      │
RST ─────►│         │      ▼
          └─────────┘    ┌─────┐
                         │NAND │───► Y
A ──────────────────────►│     │
                         └─────┘
```

Since:

```text
Q = 0
```

we get:

```text
Y = NAND(Q,A)
```

Therefore:

```text
Y = NAND(0,A)
```

Using the NAND operation:

```text
Y = 1
```

Hence:

```text
Y = 1
```

The output is independent of `A`.

Therefore, the entire sequential path may be replaced by a constant `1`.

---

# 10. State Optimization

State optimization is the optimization of states in a sequential circuit or finite state machine.

### Main Objective

* Remove unused states
* Merge equivalent states
* Reduce the number of flip-flops
* Reduce combinational logic
* Reduce area
* Improve power and performance
  


---

## 10.1 Optimization of Unused States

An FSM may contain states that can never be reached during normal operation.

For example:

```text
S0 → S1 → S2 → S3
```

If the FSM also contains:

```text
S4
S5
S6
S7
```

but these states are never reached, they are unused states.

The synthesis tool can optimize the corresponding logic.

---

# 11. State Optimization Flow

```text
RTL FSM
   │
   ▼
State Analysis
   │
   ▼
Identify Unused/Equivalent States
   │
   ▼
State Optimization
   │
   ▼
Reduced FSM
   │
   ▼
Technology Mapping
```

---

# 12. Retiming

Retiming is a sequential optimization technique in which flip-flops/registers are moved across combinational logic while preserving the functional behavior of the design.

### Main Purpose

Retiming is mainly used to:

* Reduce critical path delay
* Improve maximum operating frequency
* Balance combinational logic
* Improve timing performance

---

## Example

Original circuit:

```text
FF1 ───► Logic ───► FF2
```

If the combinational logic has a large delay, retiming can redistribute the registers:

```text
FF1 ───► Logic A ───► FF ───► Logic B ───► FF2
```

This divides a long combinational path into smaller paths.

---

# 13. Retiming Example

Consider a pipeline:

```text
          50 MHz
FF1 ───────────────► FF2
```

Suppose the logic between the flip-flops has a large delay.

By redistributing the registers, the circuit can be divided into smaller timing paths.

The objective is to increase the maximum operating frequency.

### Important Point

Retiming changes the location of registers but preserves the intended sequential functionality.

---

# 14. Sequential Logic Cloning

Sequential logic cloning means creating additional copies of sequential elements such as flip-flops to reduce fanout and routing delay.

### Example

Suppose one flip-flop drives two distant registers:

```text
             ┌──► FF B
FF A ────────┤
             └──► FF C
```

If `B` and `C` are physically far apart, routing from `A` to both destinations can introduce significant delay.

The synthesis tool can clone `FF A`.

```text
              ┌──► FF B
FF A ───► Clone A
              └──► FF C
```

The cloned registers can be placed closer to their destinations.

---

# 15. Floorplan-Aware Sequential Logic Cloning

Sequential cloning can consider the physical placement of the registers.

For example:

```text
                    FF B
                   /
Input ───► FF A ──
                   \
                    FF C
```

If `FF B` and `FF C` are located in different physical regions, cloning can reduce routing distance.

### Benefits

* Reduced routing delay
* Reduced fanout
* Improved timing
* Better physical implementation

---

# 16. Counter Optimization

A counter is a sequential circuit that changes its state according to the clock.

For example, a 3-bit up counter has:

```text
count[2:0]
```

and produces:

```text
000
001
010
011
100
101
110
111
```

Then the sequence repeats.

---
<img width="940" height="376" alt="image" src="https://github.com/user-attachments/assets/d4c8e26c-900c-4cb6-895f-73c4b8d51787" />
<img width="940" height="452" alt="image" src="https://github.com/user-attachments/assets/4448f0a1-fd05-4172-9e28-a3a7473ebc68" />



# 17. 3-Bit Up Counter

A 3-bit up counter can be represented as:

```text
              ┌────────────────┐
Clock ───────►│                │
Reset ───────►│  3-Bit Up      │
              │    Counter     │
              └───────┬────────┘
                      │
                  count[2:0]
                      │
              ┌───────┼───────┐
              │       │       │
             [2]     [1]     [0]
              │       │       │
              └───────┴───────┘
                      │
                      Q
```

---

# 18. Counter Optimization – Case 1

If only the least significant bit is required:

```text
Q = count[0]
```

then the remaining counter bits may be unnecessary for the required output.

The synthesis tool can identify unused logic and optimize it.

---

# 19. Counter Optimization – Case 2

If the complete counter output is required:

```text
Q = count[2:0]
```

then all three counter bits are required.

The sequence is:

```text
count[2:0]

000
001
010
011
100
101
110
111
```

---

# 20. D Flip-Flop Constant Optimization

A D flip-flop stores the value present at its D input at the active clock edge.

Basic D flip-flop:

```text
             ┌─────────┐
D ──────────►│ D     Q │────► Q
             │  DFF    │
CLK ────────►│         │
             └─────────┘
```

---

## DFF Optimization – Case 1
<img width="940" height="444" alt="image" src="https://github.com/user-attachments/assets/f2dbd164-dc4c-4c07-bc72-0c1929128e6c" />
<img width="1917" height="932" alt="Screenshot 2026-08-24 144321" src="https://github.com/user-attachments/assets/70a193bb-58fe-4b24-93a6-b6b298c9f8a6" />

If:

```text
D = 1
```

then at the active clock edge:

```text
Q = 1
```

The circuit can potentially be optimized by propagating the constant.

---

## DFF Optimization – Case 2

If:

```text
D = 0
```

then at the active clock edge:

```text
Q = 0
```

The output can potentially be optimized to a constant.

---
<img width="940" height="457" alt="image" src="https://github.com/user-attachments/assets/1d01ef8f-a5a1-4b9a-826e-a6aeae72286a" />
<img width="1917" height="948" alt="Screenshot 2026-08-24 155941" src="https://github.com/user-attachments/assets/8e13d6b0-d5e0-4f25-9f24-0d2a3ee30114" />

## DFF Optimization – Case 3

If a D flip-flop is connected to another D flip-flop and reset/set conditions are present, the synthesis tool can analyze the sequential behavior and optimize constant portions.

Example:

```text
          ┌───────┐          ┌───────┐
D ───────►│ DFF1  │──► Q1 ──►│ DFF2  │──► Q
          └───────┘          └───────┘
             ▲                  ▲
             │                  │
            CLK                CLK
```

If `Q1` becomes constant due to reset or constant data, the following sequential logic can also be simplified.

---
<img width="940" height="447" alt="image" src="https://github.com/user-attachments/assets/20c3dc52-347b-486c-8495-2872d74bb361" />
<img width="940" height="474" alt="image" src="https://github.com/user-attachments/assets/33dbfb13-5ec5-4fc2-a786-bb866c20bc7e" />

# 21. Optimization Checks

After performing optimization, the optimized design must be checked to ensure that the required functionality has not changed.

The general flow is:

```text
Original Design
      │
      ▼
Optimization
      │
      ▼
Optimized Design
      │
      ▼
Functional Verification
```

---

# 22. Opt-Check 1 – MUX Optimization

A 2:1 MUX has the equation:

```text
Y = S'I0 + SI1
```

Consider:

```text
I0 = 0
I1 = B
S  = A
```

Therefore:

```text
Y = A'·0 + A·B
```

Since:

```text
A'·0 = 0
```

we get:

```text
Y = AB
```

Therefore:

```text
Y = AB
```

The MUX implements an **AND gate**.

---
<img width="1917" height="960" alt="Screenshot 2026-08-24 142715" src="https://github.com/user-attachments/assets/c5b13ef1-a204-41a2-ab15-bfc6f0e0762f" />

# 23. Opt-Check 2 – MUX Optimization

Consider:

```text
I0 = B'
I1 = 1
S  = A
```

Using the MUX equation:

```text
Y = A'B' + A(1)
```

Therefore:

```text
Y = A'B' + A
```

Using Boolean simplification:

```text
Y = A + B'
```

Thus the optimized expression is:

```text
Y = A + B'
```

---
<img width="940" height="442" alt="image" src="https://github.com/user-attachments/assets/c466f66f-53f5-4130-a14b-500372b4a9b0" />


# 24. Opt-Check 3 – Cascaded MUX Optimization

Consider two cascaded MUXes.

For the first MUX:

```text
I0 = 0
I1 = B
S  = C
```

Therefore:

```text
Y1 = C'·0 + C·B
```

Hence:

```text
Y1 = BC
```

For the second MUX:

```text
I0 = Y1
I1 = 0
S  = A
```

Therefore:

```text
Y = A'Y1 + A·0
```

Substituting:

```text
Y = A'(BC)
```

Therefore:

```text
Y = A'BC
```

The optimized expression is:

```text
Y = A'BC
```

---
<img width="940" height="448" alt="image" src="https://github.com/user-attachments/assets/280c86fe-3e06-4344-966d-cd500db337f2" />

# 25. Multiplexer Equation

For a 2:1 multiplexer:

```text
Y = S'I0 + SI1
```

Where:

* `S` = Select input
* `I0` = Input selected when `S = 0`
* `I1` = Input selected when `S = 1`
* `Y` = Output

### Truth Table

| S | Y  |
| - | -- |
| 0 | I0 |
| 1 | I1 |

---

# 26. Synthesis and Optimization Flow

The general RTL-to-netlist synthesis flow is:

```text
RTL Design
    ↓
Read Technology Library
    ↓
Read Verilog
    ↓
Elaborate Design
    ↓
Synthesis
    ↓
Logic Optimization
    ↓
DFF Mapping
    ↓
Technology Mapping
    ↓
Netlist Generation
    ↓
Verification
```

---

# 27. Yosys Commands

Yosys is an open-source RTL synthesis framework used for synthesis and optimization.

## Start Yosys

```bash
yosys
```

---

## Read Liberty Library

```tcl
read_liberty -lib <library>.lib
```

Example:

```tcl
read_liberty -lib sky130_fd_sc_hd__tt_025C_1v80.lib
```

---

## Read Verilog

```tcl
read_verilog <design>.v
```

Example:

```tcl
read_verilog dff_const.v
```

---

## Synthesis

```tcl
synth -top <top_module>
```

Example:

```tcl
synth -top dff_const
```

---

## Write Verilog Netlist

```tcl
write_verilog <output>.v
```

Example:

```tcl
write_verilog dff_const_netlist.v
```

---

## Show the Design

```tcl
show
```

This can be used to visualize the synthesized design.

---

# 28. DFF Library Mapping

Flip-flop mapping can be performed using:

```tcl
dfflibmap -liberty <library>.lib
```

Example:

```tcl
dfflibmap -liberty sky130_fd_sc_hd__tt_025C_1v80.lib
```

This maps generic flip-flops to available flip-flop cells in the technology library.

---

# 29. ABC Optimization

ABC is used for logic optimization and technology mapping.

Command:

```tcl
abc -liberty <library>.lib
```

Example:

```tcl
abc -liberty sky130_fd_sc_hd__tt_025C_1v80.lib
```

ABC can perform:

* Boolean logic optimization
* Technology mapping
* Logic restructuring
* Gate minimization

---

# 30. Complete DFF Optimization Flow

```tcl
read_liberty -lib <library>.lib
read_verilog dff_const.v
synth -top dff_const
dfflibmap -liberty <library>.lib
abc -liberty <library>.lib
write_verilog dff_const_netlist.v
show
```

---

# 31. Complete Counter Optimization Flow

```tcl
read_liberty -lib <library>.lib
read_verilog counter_opt.v
synth -top counter_opt
dfflibmap -liberty <library>.lib
abc -liberty <library>.lib
write_verilog counter_opt_netlist.v
show
```

---

# 32. Important Optimization Commands

| Command         | Purpose                                           |
| --------------- | ------------------------------------------------- |
| `read_liberty`  | Read technology/library information               |
| `read_verilog`  | Read RTL Verilog design                           |
| `synth`         | Perform RTL synthesis                             |
| `dfflibmap`     | Map flip-flops to library cells                   |
| `abc`           | Perform logic optimization and technology mapping |
| `write_verilog` | Generate Verilog netlist                          |
| `show`          | Display the synthesized circuit                   |

---

# 33. Optimization Techniques Summary

| Technique                | Purpose                                |
| ------------------------ | -------------------------------------- |
| Constant Propagation     | Removes logic driven by constants      |
| Boolean Simplification   | Simplifies Boolean expressions         |
| K-map Reduction          | Minimizes logic expressions            |
| State Optimization       | Removes/merges unnecessary states      |
| Retiming                 | Improves timing by moving registers    |
| Sequential Logic Cloning | Reduces fanout and routing delay       |
| Counter Optimization     | Removes unnecessary counter logic      |
| DFF Mapping              | Maps flip-flops to technology cells    |
| ABC Optimization         | Optimizes and maps combinational logic |
| Technology Mapping       | Maps logic to available standard cells |

---

# 34. Area, Power and Timing Optimization

Logic optimization generally targets three important parameters:

## Area

The number and size of gates/cells used in the design.

Optimization aims to:

```text
Reduce number of cells
        ↓
Reduce chip area
```

---

## Power

Power consumption can be reduced by:

* Removing unnecessary logic
* Reducing switching activity
* Reducing capacitance
* Removing unused registers

---

## Timing

Timing optimization aims to reduce the critical path delay.

Techniques include:

* Retiming
* Logic restructuring
* Sequential cloning
* Buffering
* Gate optimization

---

# 35. Overall Optimization Flow

```text
                 RTL DESIGN
                     │
                     ▼
          ┌─────────────────────┐
          │ Combinational       │
          │ Optimization        │
          └──────────┬──────────┘
                     │
                     ▼
          ┌─────────────────────┐
          │ Sequential          │
          │ Optimization        │
          └──────────┬──────────┘
                     │
          ┌──────────┼──────────┐
          ▼          ▼          ▼
     Constant      State      Retiming
   Propagation   Optimization
          │          │          │
          └──────────┼──────────┘
                     ▼
          Sequential Logic
             Cloning
                     │
                     ▼
             DFF Mapping
                     │
                     ▼
             ABC Optimization
                     │
                     ▼
           Technology Mapping
                     │
                     ▼
            Optimized Netlist
                     │
                     ▼
              Verification
```

---

# 36. Key Points to Remember

### Combinational Optimization

* Does not contain memory elements.
* Boolean expressions can be simplified.
* Constant values can be propagated.
* Redundant gates can be removed.

### Sequential Optimization

* Deals with flip-flops, registers, counters and FSMs.
* Must preserve sequential behavior.
* Reset and set conditions are important.
* State optimization can remove unused states.

### Retiming

```text
Move registers
      ↓
Balance logic paths
      ↓
Reduce critical path
      ↓
Improve frequency
```

### Sequential Logic Cloning

```text
High fanout
     ↓
Clone register
     ↓
Place copies closer to loads
     ↓
Reduce routing delay
```

### Counter Optimization

```text
Identify unused counter outputs
            ↓
Remove unnecessary logic
            ↓
Reduce area and power
```

---

# 37. Conclusion

Logic optimization is an important stage in digital and VLSI design. It improves the implementation of a circuit without changing its intended functionality.

**Combinational logic optimization** focuses on Boolean simplification, constant propagation, K-map reduction and logic restructuring.

**Sequential logic optimization** focuses on sequential constant propagation, state optimization, retiming, sequential logic cloning and counter optimization.

Tools such as **Yosys and ABC** can automatically perform many of these optimizations and map the design to a target technology library.

The main goals of logic optimization are:

```text
         Optimization
              │
     ┌────────┼────────┐
     ▼        ▼        ▼
   Area     Power    Timing
    ↓        ↓        ↓
 Smaller   Lower    Faster
  Design   Power   Operation
```

A successful optimized design should maintain the required functionality while achieving improved **area, power and timing performance**.
