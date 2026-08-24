# Module 4 – Gate Level Simulation (GLS)

## Introduction

Gate Level Simulation, commonly called **GLS**, is the process of simulating a synthesized digital design using its **gate-level netlist**.

In the digital VLSI design flow, a design is initially written as **RTL (Register Transfer Level) code**. The RTL code is then synthesized to generate a **gate-level netlist**. This netlist represents the design using logic gates or standard cells.

Gate Level Simulation is performed after synthesis to verify that the synthesized design behaves correctly.

The main objectives of GLS are:

* To verify the logical correctness of the synthesized design.
* To compare RTL simulation results with gate-level simulation results.
* To detect synthesis simulation mismatches.
* To verify the connectivity of the synthesized netlist.
* To verify timing behavior when delay information is included.
* To ensure that the synthesized hardware behaves as intended.

---

# 1. Synthesis

**Synthesis** is the process of converting an RTL design written in Verilog or SystemVerilog into a gate-level representation.

The synthesis tool analyzes the RTL code and converts the logic into gates or standard cells available in the target technology library.

The synthesis process may involve:

* Logic optimization.
* Boolean simplification.
* Technology mapping.
* Gate mapping.
* Area optimization.
* Timing optimization.
* Removal of redundant logic.

The general synthesis flow is:

```text
RTL Design
    |
    v
Synthesis
    |
    v
Gate-Level Netlist
```

For example, consider the following RTL expression:

```verilog
assign y = (a & b) | c;
```

This can be represented at the gate level as:

```text
a ----\
       AND ----\
b ----/         \
                OR ---- y
c --------------/
```

The synthesized netlist contains the gates and connections required to implement the original RTL functionality.

---

# 2. What is Gate Level Simulation?

Gate Level Simulation is the simulation of a **synthesized gate-level netlist** using a testbench.

In RTL simulation, the RTL code is directly simulated.

```text
Testbench
    |
    v
RTL Design
    |
    v
Output
```

In Gate Level Simulation, the synthesized netlist is simulated instead.

```text
Testbench
    |
    v
Gate-Level Netlist
    |
    v
Output
```

The testbench applies different input combinations to the design and observes the corresponding outputs.

The outputs obtained from GLS can be compared with the RTL simulation outputs to verify the correctness of synthesis.

---

# 3. Why Do We Run GLS?

Gate Level Simulation is mainly performed for two important reasons:

1. **To verify the logical correctness of the synthesized design.**
2. **To verify the timing behavior of the design.**

---

## 3.1 Verify Logical Correctness

After synthesis, the RTL design is converted into a gate-level netlist.

GLS checks whether this synthesized netlist produces the correct output for the given input combinations.

```text
RTL Design
    |
    v
Synthesis
    |
    v
Gate-Level Netlist
    |
    v
Gate-Level Simulation
    |
    v
Verify Logical Correctness
```

The output of the gate-level simulation should match the expected functionality of the original RTL design.

---

## 3.2 Verify Timing

RTL simulation mainly verifies functionality and generally does not represent the actual delays introduced by the synthesized gates.

After synthesis, timing information can be included in the simulation.

```text
Gate-Level Netlist
        +
Delay Information
        |
        v
Gate-Level Simulation
        |
        v
Timing Verification
```

Timing GLS helps in checking:

* Propagation delay.
* Delayed output transitions.
* Setup timing.
* Hold timing.
* Timing violations.

Therefore, GLS can be used for both:

* **Functional verification**
* **Timing verification**

---

# 4. Gate-Level Netlist

A **netlist** is a description of a digital circuit in terms of its components and their connections.

After synthesis, the RTL design is converted into a gate-level netlist.

For example:

```verilog
assign y = (a & b) | c;
```

The gate-level implementation can be represented as:

```verilog
module gate_level_design(
    input a,
    input b,
    input c,
    output y
);

wire w1;

and u1(w1, a, b);
or  u2(y, w1, c);

endmodule
```

Here:

```text
w1 = a & b

y = w1 | c
```

Therefore:

```text
y = (a & b) | c
```

---

# 5. Gate-Level Verilog Models

A gate-level Verilog model describes the circuit using gates or synthesized standard cells.

Common Verilog gate primitives include:

* `and`
* `or`
* `not`
* `nand`
* `nor`
* `xor`
* `xnor`
* `buf`

For example:

```verilog
and u1(w1, a, b);
or  u2(y, w1, c);
```

The exact structure of a synthesized gate-level netlist depends on:

* The synthesis tool.
* The target technology.
* The standard-cell library.
* Timing constraints.
* Area constraints.

---

# 6. Functional Gate-Level Simulation

Functional GLS is used to verify the logical functionality of the synthesized gate-level netlist.

It checks:

* Logical correctness.
* Connectivity.
* Correct synthesis of the RTL design.
* Functional equivalence between RTL and netlist.

The flow is:

```text
Gate-Level Netlist
        +
Testbench
        |
        v
Functional GLS
        |
        v
Functional Verification
```

In functional GLS, the main objective is to check whether the output logic is correct.

---

# 7. Timing Gate-Level Simulation

Timing GLS includes delay information in the gate-level simulation.

It checks:

* Gate delays.
* Propagation delays.
* Timing relationships.
* Setup violations.
* Hold violations.
* Delayed output transitions.

The flow is:

```text
Gate-Level Netlist
        +
Delay Information
        +
Testbench
        |
        v
Timing GLS
        |
        v
Functional + Timing Verification
```

Thus:

```text
Functional GLS
       |
       v
Checks Logic


Timing GLS
       |
       v
Checks Logic + Timing
```

---

# 8. Propagation Delay

Propagation delay is the time required for a change at the input of a circuit to produce the corresponding change at the output.

Without delay:

```text
Input Change -----------------> Output Change
```

With delay:

```text
Input Change ----> Gate Delay ----> Output Change
```

For example:

```text
Input
  |
  v
+-------+
| Gate  |
+-------+
  |
  | Delay
  v
Output
```

Timing GLS allows these delays to be observed during simulation.

---

# 9. GLS Using Icarus Verilog

**Icarus Verilog** is a Verilog simulation tool that can be used to compile and simulate digital designs.

The general simulation flow is:

```text
Design
   +
Gate-Level Netlist
   +
Testbench
   |
   v
Icarus Verilog
   |
   v
Simulation
   |
   v
VCD File
   |
   v
GTKWave
   |
   v
Waveform Analysis
```

The basic process consists of:

1. Compiling the Verilog files.
2. Running the simulation.
3. Generating a VCD file.
4. Opening the VCD file using GTKWave.
5. Observing the input and output waveforms.

---

# 10. VCD File

**VCD** stands for **Value Change Dump**.

A VCD file stores the changes in signal values during simulation.

The VCD file can contain:

* Input signal transitions.
* Output signal transitions.
* Internal signal transitions.
* Timing information.

A VCD file can be generated using:

```verilog
$dumpfile("wave.vcd");
$dumpvars(0, tb);
```

Example:

```verilog
initial begin
    $dumpfile("wave.vcd");
    $dumpvars(0, tb);
end
```

After simulation, a file such as the following is generated:

```text
wave.vcd
```

---

# 11. GTKWave

**GTKWave** is a waveform viewer used to observe the simulation results stored in a VCD file.

GTKWave can display:

* Input signals.
* Output signals.
* Internal signals.
* Signal transitions.
* Timing relationships.
* Propagation delays.

The flow is:

```text
Verilog Simulation
        |
        v
      VCD File
        |
        v
      GTKWave
        |
        v
     Waveform
```

By observing the waveform, the user can verify whether the output changes according to the expected functionality.

---

# 12. Testbench for Gate Level Simulation

A testbench is used to apply different input combinations to the Design Under Test (DUT).

Example:

```verilog
module tb;

reg a;
reg b;
reg c;

wire y;

gate_level_design dut(
    .a(a),
    .b(b),
    .c(c),
    .y(y)
);

initial begin

    $dumpfile("wave.vcd");
    $dumpvars(0, tb);

    a = 0;
    b = 0;
    c = 0;
    #10;

    a = 0;
    b = 0;
    c = 1;
    #10;

    a = 0;
    b = 1;
    c = 0;
    #10;

    a = 0;
    b = 1;
    c = 1;
    #10;

    a = 1;
    b = 0;
    c = 0;
    #10;

    a = 1;
    b = 0;
    c = 1;
    #10;

    a = 1;
    b = 1;
    c = 0;
    #10;

    a = 1;
    b = 1;
    c = 1;
    #10;

    $finish;

end

endmodule
```

The testbench:

* Generates input values.
* Applies the inputs to the DUT.
* Generates a VCD file.
* Allows the output to be observed using GTKWave.

---

# 13. Same Testbench for RTL and GLS

The same testbench can generally be used to verify both:

* RTL design.
* Gate-level netlist.

The only major difference is the Design Under Test.

```text
RTL Simulation

Testbench
    |
    v
RTL Design
    |
    v
RTL Output
```

```text
Gate-Level Simulation

Testbench
    |
    v
Gate-Level Netlist
    |
    v
GLS Output
```

The results can then be compared.

```text
                Same Testbench
                      |
             +--------+--------+
             |                 |
             v                 v
         RTL Design      Gate-Level Netlist
             |                 |
             v                 v
         RTL Output        GLS Output
             |                 |
             +--------+--------+
                      |
                      v
                 Comparison
```

---

# 14. Synthesis Simulation Mismatch

A **synthesis simulation mismatch** occurs when the RTL simulation behavior is different from the synthesized gate-level simulation behavior.

In other words:

```text
RTL Simulation Output
        !=
Gate-Level Simulation Output
```
<img width="940" height="477" alt="image" src="https://github.com/user-attachments/assets/697be24a-cab4-4a9a-b9eb-e48b97980f62" />
<img width="940" height="461" alt="image" src="https://github.com/user-attachments/assets/e225c3fb-fbe0-4f4f-862e-5464dc0160bc" />

This means that the behavior observed during RTL simulation does not match the behavior of the synthesized hardware.

Some common causes are:

* Missing sensitivity list.
* Blocking and non-blocking assignment issues.
* Non-standard Verilog coding.
* Incomplete combinational assignments.
* Unintended latch inference.
* Incorrect reset logic.
* Incorrect initialization.
* Unsupported synthesis constructs.

---

# 15. Missing Sensitivity List

A sensitivity list specifies the signals that can trigger an `always` block.

Consider the following code:

```verilog
always @(a)
begin
    y = a & b;
end
```

Here, the output `y` depends on both:

```text
a
b
```

However, only `a` is included in the sensitivity list.

If `b` changes while `a` remains unchanged, the `always` block may not execute during RTL simulation.

Therefore, the output may not update correctly.

The recommended method is:

```verilog
always @(*)
begin
    y = a & b;
end
```

The `@(*)` automatically includes all signals used inside the block.

---

# 16. Missing Sensitivity List Example

Consider:

```verilog
always @(sel)
begin
    if (sel)
        y = i1;
    else
        y = i0;
end
```

If `i0` or `i1` changes while `sel` remains unchanged, the `always` block will not execute.

Therefore, the simulation output may remain unchanged even though the actual combinational hardware should respond to changes in `i0` or `i1`.

The better approach is:

```verilog
always @(*)
begin
    if (sel)
        y = i1;
    else
        y = i0;
end
```

---

# 17. Why Missing Sensitivity Lists Cause Mismatch

The simulation flow is:

```text
Input Changes
      |
      v
Sensitivity List Checked
      |
      v
Always Block Executes
      |
      v
Output Updated
```

If an input signal is missing from the sensitivity list:

```text
Input Changes
      |
      v
Input Not Present in Sensitivity List
      |
      v
Always Block Does Not Execute
      |
      v
Output May Not Update
```

Therefore, the RTL simulation can behave differently from the synthesized hardware.

This is one of the important causes of synthesis simulation mismatch.

---

# 18. Blocking and Non-Blocking Assignments

Verilog provides two commonly used assignment operators:

## Blocking Assignment

```verilog
=
```

## Non-Blocking Assignment

```verilog
<=
```

These two assignments have different simulation behavior.

Improper use of blocking and non-blocking assignments can cause unexpected simulation results and may contribute to mismatches between the intended behavior and simulation behavior.

---

# 19. Blocking Assignment

Blocking assignment uses the `=` operator.

Example:

```verilog
always @(*) begin
    q0 = d;
    q  = q0;
end
```

Blocking assignments execute sequentially.

The first assignment is completed before the next assignment is executed.

```text
d
|
v
q0
|
v
q
```

The second statement uses the updated value of `q0`.

### Characteristics of Blocking Assignment

* Uses `=`.
* Executes statements sequentially.
* The assignment occurs immediately within the procedural flow.
* Commonly used for combinational logic.

---


# 20. Non-Blocking Assignment

Non-blocking assignment uses the `<=` operator.

Example:

```verilog
always @(posedge clk) begin
    q <= d;
end
```

Non-blocking assignments schedule updates instead of immediately updating the left-hand side during procedural execution.

### Characteristics of Non-Blocking Assignment

* Uses `<=`.
* Commonly used for sequential logic.
* Used for flip-flops and registers.
* Updates are scheduled after the right-hand-side expressions are evaluated.

Example:

```verilog
always @(posedge clk) begin
    q0 <= d;
    q  <= q0;
end
```

This represents sequential behavior where the registers update together on the clock edge.

---

# 21. Blocking vs Non-Blocking Assignment

| Feature       | Blocking Assignment              | Non-Blocking Assignment |
| ------------- | -------------------------------- | ----------------------- |
| Operator      | `=`                              | `<=`                    |
| Common Usage  | Combinational Logic              | Sequential Logic        |
| Execution     | Sequential                       | Scheduled               |
| Update        | Immediate within procedural flow | Scheduled for update    |
| Typical Block | `always @(*)`                    | `always @(posedge clk)` |

The commonly recommended coding style is:

```text
Combinational Logic
        |
        v
Use Blocking Assignment (=)


Sequential Logic
        |
        v
Use Non-Blocking Assignment (<=)
```

---

# 22. Blocking Assignment Order

Consider:

```verilog
always @(posedge clk) begin
    q0 = d;
    q  = q0;
end
```

The execution occurs in the following order:

```text
q0 = d
   |
   v
q = q0
```

Since `q0` is immediately updated using blocking assignment, `q` receives the new value of `q0`.

Changing the order of blocking assignments can therefore change the simulation behavior.

For this reason, blocking assignments should generally be avoided for ordinary flip-flop modeling.

For sequential logic, non-blocking assignments are generally preferred.

---

# 23. Incomplete Combinational Logic

Incomplete assignments in a combinational block can cause unintended behavior.

Consider:

```verilog
always @(*) begin
    if (sel)
        y = a;
end
```

When:

```text
sel = 1
```

the output is assigned:

```text
y = a
```

But when:

```text
sel = 0
```

there is no assignment to `y`.

Therefore, `y` may retain its previous value.

This can cause an unintended latch to be inferred.

A better implementation is:

```verilog
always @(*) begin
    if (sel)
        y = a;
    else
        y = b;
end
```

Now `y` is assigned for all possible input conditions.

---

# 24. Latch Inference

A latch can be unintentionally inferred when a signal in a combinational block is not assigned in every possible condition.

Example:

```verilog
always @(*) begin
    if (sel)
        y = a;
end
```

The behavior is:

```text
sel = 1
    |
    v
y = a


sel = 0
    |
    v
y retains previous value
```

Since the output retains its previous value, storage behavior is implied.

Therefore, the synthesized hardware may infer a latch.

To avoid unintended latch inference, all outputs should normally be assigned for every possible condition.

---

# 25. Ternary Operator


The **ternary operator** is a conditional operator commonly used to describe a multiplexer.

The general syntax is:

```verilog
condition ? true_value : false_value
```

For a 2:1 multiplexer:

```verilog
assign y = sel ? i1 : i0;
```

The operation is:

```text
If sel = 0

y = i0
```

```text
If sel = 1

y = i1
```

---

# 26. Ternary Operator as a 2:1 MUX

The ternary operator:

```verilog
assign y = sel ? i1 : i0;
```

represents a 2:1 multiplexer.

```text
             +---------+
i0 --------->|         |
             |   MUX   |------> y
i1 --------->|         |
             +----+----+
                  |
                 sel
```

The truth table is:

| `sel` | Output `y` |
| ----- | ---------- |
| 0     | `i0`       |
| 1     | `i1`       |

---


# 27. Verilog Code for Ternary Operator MUX

```verilog
module ternary_operator_mux(
    input i0,
    input i1,
    input sel,
    output y
);

assign y = sel ? i1 : i0;

endmodule
```

The synthesis tool converts this RTL description into a suitable hardware implementation.

The synthesized implementation may use:

* Logic gates.
* A multiplexer cell from the standard-cell library.

---

# 28. Testbench for Ternary Operator MUX

```verilog
module tb_ternary_operator_mux;

reg i0;
reg i1;
reg sel;

wire y;

ternary_operator_mux uut(
    .i0(i0),
    .i1(i1),
    .sel(sel),
    .y(y)
);

initial begin

    $dumpfile("tb_ternary_operator_mux.vcd");
    $dumpvars(0, tb_ternary_operator_mux);

    i0 = 0;
    i1 = 0;
    sel = 0;
    #10;

    i0 = 0;
    i1 = 1;
    sel = 0;
    #10;

    i0 = 1;
    i1 = 0;
    sel = 1;
    #10;

    i0 = 0;
    i1 = 1;
    sel = 1;
    #10;

    $finish;

end

endmodule
```

The waveform generated can be viewed using GTKWave.

---
<img width="940" height="466" alt="image" src="https://github.com/user-attachments/assets/6d444740-9b31-4b7a-9b95-8a35cc3fd9ea" />
<img width="940" height="464" alt="image" src="https://github.com/user-attachments/assets/79e60de8-c565-487c-a40b-5ea1e302c4b4" />
# 29. Functional Simulation of Ternary MUX

The RTL design is first simulated using the testbench.

```text
Ternary Operator MUX RTL
            +
        Testbench
            |
            v
      RTL Simulation
            |
            v
         Waveform
```

The functionality is verified by checking:

```text
sel = 0  ->  y = i0

sel = 1  ->  y = i1
```

---

# 30. Synthesis of Ternary MUX

The RTL MUX design is synthesized to generate a gate-level netlist.

```text
Ternary Operator MUX RTL
            |
            v
        Synthesis
            |
            v
     Gate-Level Netlist
```

The netlist represents the MUX using gates or technology-specific cells.

---
<img width="940" height="444" alt="image" src="https://github.com/user-attachments/assets/bd2f09e0-8fbe-4ca0-8ac3-f757ca4e1382" />

# 31. Gate-Level Simulation of Ternary MUX

After synthesis, the generated gate-level netlist is simulated.

The flow is:

```text
Ternary Operator MUX RTL
            |
            v
        Synthesis
            |
            v
     Gate-Level Netlist
            +
        Testbench
            |
            v
   Gate-Level Simulation
            |
            v
         VCD File
            |
            v
         GTKWave
            |
            v
         Waveform
```

The gate-level simulation output is compared with the expected MUX behavior.

---


# 32. General GLS Flow

The complete Gate Level Simulation flow is:

```text
                 RTL Design
                     |
                     v
                 Synthesis
                     |
                     v
           Gate-Level Netlist
                     |
         +-----------+-----------+
         |                       |
         v                       v
    Library Models        Delay Information
         |                       |
         +-----------+-----------+
                     |
                     v
                 Testbench
                     |
                     v
                Icarus Verilog
                     |
                     v
                   VCD File
                     |
                     v
                   GTKWave
                     |
                     v
             Waveform Analysis
                     |
                     v
        Functional / Timing Verification
```

---

# 33. Complete File Structure

A typical GLS project can have the following structure:

```text
GLS_Project/
│
├── rtl/
│   └── ternary_operator_mux.v
│
├── netlist/
│   └── ternary_operator_mux_net.v
│
├── tb/
│   └── tb_ternary_operator_mux.v
│
├── lib/
│   └── library_models.v
│
└── waveforms/
    └── tb_ternary_operator_mux.vcd
```

---

# 34. Icarus Verilog Commands

## Compile RTL Design

```bash
iverilog -o simulation design.v tb.v
```

Where:

* `iverilog` is the compiler.
* `-o simulation` specifies the output executable name.
* `design.v` is the design file.
* `tb.v` is the testbench file.

---

## Run the Simulation

```bash
vvp simulation
```

This runs the compiled simulation.

If `$dumpfile` and `$dumpvars` are present in the testbench, a VCD file will be generated.

---

## Open the Waveform

```bash
gtkwave wave.vcd
```

This opens the generated VCD waveform file in GTKWave.

---

# 35. Gate-Level Simulation Command Flow

For gate-level simulation, the gate-level netlist and required library models must be included.

A typical command is:

```bash
iverilog -o gls_sim \
gate_level_netlist.v \
library_models.v \
tb.v
```

Run the simulation using:

```bash
vvp gls_sim
```

Open the generated waveform:

```bash
gtkwave wave.vcd
```

The complete command flow is:

```text
iverilog
    |
    v
Compiled Simulation
    |
    v
vvp
    |
    v
VCD File
    |
    v
gtkwave
    |
    v
Waveform
```

---

# 36. RTL Simulation vs Gate-Level Simulation

| Feature             | RTL Simulation          | Gate-Level Simulation              |
| ------------------- | ----------------------- | ---------------------------------- |
| Design Used         | RTL Code                | Gate-Level Netlist                 |
| Main Purpose        | Functional Verification | Functional and Timing Verification |
| Gate Information    | Not Explicitly Visible  | Available                          |
| Delay Information   | Usually Not Included    | Can Be Included                    |
| Simulation Speed    | Faster                  | Slower                             |
| Synthesis Effects   | Not Visible             | Can Be Observed                    |
| Timing Verification | Limited                 | Possible with Delay Information    |

---

# 37. Functional GLS vs Timing GLS

| Feature            | Functional GLS       | Timing GLS              |
| ------------------ | -------------------- | ----------------------- |
| Main Purpose       | Verify Logic         | Verify Logic and Timing |
| Gate-Level Netlist | Yes                  | Yes                     |
| Testbench          | Yes                  | Yes                     |
| Delay Information  | Usually Not Included | Included                |
| Propagation Delay  | Not Main Focus       | Checked                 |
| Timing Behavior    | Limited              | Verified                |

---

# 38. Synthesis Simulation Mismatch Summary
<img width="1916" height="956" alt="Screenshot 2026-08-24 171416" src="https://github.com/user-attachments/assets/d4ce54e6-e16f-4338-b0f0-379ae7d62281" />

The following issues can cause mismatches between RTL and gate-level simulation:

## Missing Sensitivity List

```verilog
always @(a)
    y = a & b;
```

Better:

```verilog
always @(*)
    y = a & b;
```

---

## Incorrect Blocking and Non-Blocking Assignments

Combinational logic generally uses:

```verilog
=
```

Sequential logic generally uses:

```verilog
<=
```

---
<img width="1917" height="907" alt="Screenshot 2026-08-24 180513" src="https://github.com/user-attachments/assets/9f4a7026-d581-40c6-92de-f62f2831424d" />
## Incomplete Combinational Assignments

Incorrect:

```verilog
always @(*) begin
    if (sel)
        y = a;
end
```

Correct:

```verilog
always @(*) begin
    if (sel)
        y = a;
    else
        y = b;
end
```

---


## Non-Synthesizable or Non-Standard Coding

Using unsupported or simulation-only constructs can result in differences between simulation and synthesized hardware.

Therefore, RTL code should follow synthesizable coding practices.

---

# 39. Important Points

* Gate Level Simulation is performed after synthesis.
* GLS uses the synthesized gate-level netlist.
* A testbench is used to apply input combinations.
* The RTL and gate-level results can be compared.
* Functional GLS verifies logical correctness.
* Timing GLS verifies both functionality and timing.
* VCD stands for Value Change Dump.
* VCD files store signal transitions during simulation.
* GTKWave is used to view VCD waveforms.
* Icarus Verilog can be used to compile and simulate Verilog designs.
* Missing sensitivity lists can cause simulation mismatches.
* Blocking assignment uses `=`.
* Non-blocking assignment uses `<=`.
* Blocking assignments are generally used for combinational logic.
* Non-blocking assignments are generally used for sequential logic.
* The ternary operator can be used to describe a 2:1 MUX.
* Incomplete combinational assignments can infer latches.
* Proper RTL coding helps avoid synthesis simulation mismatches.

---

# 40. Quick Revision Flow

```text
RTL Design
    |
    v
Synthesis
    |
    v
Gate-Level Netlist
    |
    +----------------------+
    |                      |
    v                      v
Library Models      Delay Information
    |                      |
    +----------+-----------+
               |
               v
           Testbench
               |
               v
        Gate-Level Simulation
               |
               v
            VCD File
               |
               v
            GTKWave
               |
               v
       Waveform Analysis
               |
               v
Functional / Timing Verification
```

---

# Conclusion

Gate Level Simulation (GLS) is an important post-synthesis verification process in the digital VLSI design flow. It involves simulating the synthesized gate-level netlist using a testbench to verify that the synthesized hardware behaves correctly.

GLS is used to verify both logical correctness and timing behavior. It also helps identify synthesis simulation mismatches caused by issues such as missing sensitivity lists, incorrect use of blocking and non-blocking assignments, incomplete combinational logic, and unintended latch inference.

Using tools such as **Icarus Verilog** for simulation and **GTKWave** for waveform analysis, the designer can generate VCD files, observe signal transitions, compare RTL and gate-level outputs, and verify the functionality and timing behavior of the synthesized design.
