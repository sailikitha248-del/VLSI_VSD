Module 1 — Introduction to Verilog, Design & Testbench

1. Introduction to Verilog Simulation

A simulator is used for checking a design.

- RTL design is checked for adherence to the specification by simulating the design.
- A simulator is a tool used for simulating a design.
- Example simulator: Icarus Verilog ("iverilog").

Design
<img width="1911" height="971" alt="Screenshot 2026-08-22 230311" src="https://github.com/user-attachments/assets/213c0025-5cf4-4499-9ee6-4c37c0e2633b" />

The design is the actual Verilog code or set of Verilog codes written to meet the required specifications.

Testbench

A testbench is set up to apply test vectors to the design to check its functionality.

---

2. Working of a Simulator

The simulator mainly looks for changes in input signals.

- Changes in input signals cause the design to be evaluated.
- If there is no change to the input, there is generally no change to the output.
- The simulator looks for changes in the values of inputs.

---

3. Testbench

Basic testbench structure:

+-------------------+       +----------+       +------------------+
| Stimulus Generator| ----> |  Design  | ----> | Stimulus Observer|
+-------------------+       +----------+       +------------------+
        Primary I/Ps             |                  Primary O/Ps
                                 |

Important points

- A design may have one or more primary inputs.
- A design may have one or more primary outputs.
- A testbench does not have primary inputs or primary outputs.
- The testbench generates inputs for the design and observes its outputs.

---

4. Verilog Simulation Flow

The basic simulation flow is:

        Design
           \
            \
             v
        +-----------+
        |  iverilog |
        +-----------+
             |
             v
        +-----------+
        |  VCD File |
        |  Dumpfile  |
        +-----------+
             |
             v
        +-----------+
        | GTKWave   |
        +-----------+
             |
             v
          Waveform


Flow

1. Write the Design.
2. Write the Testbench.
3. Compile both using "iverilog".
4. Generate a VCD (Value Change Dump) file.
5. Open the VCD file using GTKWave.
6. Observe the waveforms and verify functionality.

---

5. Simulation Commands

Compile the design and testbench

iverilog good-mux.v tb-good-mux.v

This generates the simulation executable, commonly "a.out".

Run the simulation

./a.out

Open the waveform

gtkwave tb-good-mux.vcd

Open/edit Verilog files

gvim tb-good-mux.v

---

6. Testbench Structure

A typical testbench instantiates the design under test (DUT).

module tb_good_mux;

    // Inputs
    reg sel;
    reg i0;
    reg i1;

    // Output
    wire y;

    // Instantiate Unit Under Test (UUT)
    good_mux uut (
        .sel(sel),
        .i0(i0),
        .i1(i1),
        .y(y)
    );

endmodule

The notes refer to this as:

Instantiate Unit Under Test (UUT)

---
<img width="1919" height="978" alt="Screenshot 2026-08-22 221902" src="https://github.com/user-attachments/assets/8f0f3e1a-39e7-48f8-9226-e3087692050f" />

7. Value Change Dump (VCD)

A VCD file stores signal changes during simulation.

Example:

initial begin
    $dumpfile("tb_good_mux.vcd");
    $dumpvars(0, tb_good_mux);
end

- "$dumpfile" specifies the VCD file name.
- "$dumpvars" specifies which signals should be recorded.
- GTKWave can then be used to view these signal changes as waveforms.

---

8. Inputs / Stimulus Generator

Example initial input conditions:

initial begin
    sel = 0;
    i0  = 0;
    i1  = 0;

    #300 $finish;
end

Here:

- "sel = 0"
- "i0 = 0"
- "i1 = 0"
- "#300" means wait for 300 simulation time units.
- "$finish" terminates the simulation.

---

9. Generating Periodic Stimulus

The "always" block can be used to continuously toggle signals.

always #75 sel = ~sel;
always #10 i0  = ~i0;
always #15 i1  = ~i1;

Meaning

always #75 sel = ~sel;

Toggles "sel" after every 75 time units.

always #10 i0 = ~i0;

Toggles "i0" after every 10 time units.

always #15 i1 = ~i1;

Toggles "i1" after every 15 time units.

This code forms a stimulus generator, because it automatically changes the inputs after specified durations.

---

10. Complete Example Testbench

module tb_good_mux;

    reg sel;
    reg i0;
    reg i1;
    wire y;

    // Instantiate Unit Under Test
    good_mux uut (
        .sel(sel),
        .i0(i0),
        .i1(i1),
        .y(y)
    );

    // Value Change Dump
    initial begin
        $dumpfile("tb_good_mux.vcd");
        $dumpvars(0, tb_good_mux);
    end

    // Initial inputs
    initial begin
        sel = 0;
        i0  = 0;
        i1  = 0;

        #300 $finish;
    end

    // Stimulus generator
    always #75 sel = ~sel;
    always #10 i0  = ~i0;
    always #15 i1  = ~i1;

endmodule

---

11. Key Concepts

Concept| Meaning
Design| Verilog RTL code being tested
Testbench| Code that applies inputs and checks the design
Stimulus Generator| Generates/changing input signals
Stimulus Observer| Observes design outputs
iverilog| Verilog simulator/compiler
VCD| Value Change Dump file containing signal changes
GTKWave| Tool used to view simulation waveforms
UUT/DUT| Unit/Design Under Test
"$dumpfile"| Specifies the VCD output file
"$dumpvars"| Specifies signals to dump
"$finish"| Ends the simulation
"always"| Continuously executes a procedural block

---

12. Overall Process

Write Design
     |
     v
Write Testbench
     |
     v
Generate Stimulus
     |
     v
Compile using iverilog
     |
     v
Run ./a.out
     |
     v
Generate VCD
     |
     v
Open VCD in GTKWave
     |
     v
Observe Waveforms
     |
     v
Verify Design Functionality
