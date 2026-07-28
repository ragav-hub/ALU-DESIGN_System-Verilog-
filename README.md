# Experiment 1: ALU Design using Enumerated Data Types and Case Statements

---

## Aim  
To design and simulate a **4-bit Arithmetic Logic Unit (ALU)** using **SystemVerilog HDL** with **Enumerated Data Types and Case Statements**, and verify its functionality using **ModelSim 2020.1**.

---

## Apparatus Required  
- Computer with **Windows** OS  
- **ModelSim 2020.1** (or later) installed  
- SystemVerilog source code editor  

---

## Description about ALU  
An **Arithmetic Logic Unit (ALU)** is a combinational circuit that performs arithmetic and logical operations.  
In this experiment, the ALU is designed using:  
- **Enumerated Data Types** to represent ALU operations in a readable form.  
- **Case Statements** to implement the functionality of each operation.  

This approach improves code readability, maintainability, and reduces errors compared to using raw binary values for control signals.  

Common ALU operations included in this design are:  
- **Arithmetic:** Addition, Subtraction  
- **Logical:** AND, OR, XOR, NOT  
- **Shift Operations:** Left Shift, Right Shift  

---

## Features
- Uses **SystemVerilog Enumerated Types** for operation selection  
- Implements ALU operations with **Case Statements**  
- Parameterized design for scalability  
- Includes a **Testbench** for functional verification  
- Compatible with **ModelSim 2020.1**  

---

## Procedure  

1. **Open ModelSim 2020.1**  
   - Launch the ModelSim IDE from the Start Menu (Windows) or terminal (Linux).  

2. **Create a New Project**  
   - Go to `File → New → Project`.  
   - Enter a project name (e.g., `ALU_Enum_Project`).  
   - Set the project location.  
   - Click **OK**.  

3. **Add SystemVerilog Source Files**  
   - Create a new source file named `alu_enum.sv` and type the ALU design code.  
   - Create a new source file named `alu_enum_tb.sv` and type the testbench code.  

4. **Compile the Design and Testbench**  
   - Select both files (`alu_enum.sv` and `alu_enum_tb.sv`).  
   - Right-click → **Compile Selected**.  
   - Ensure there are no syntax errors.  

5. **Start Simulation**  
   - Go to `Simulate → Start Simulation`.  
   - In the **Library window**, expand **work**.  
   - Select the testbench module (`alu_enum_tb`).  
   - Click **OK**.  

6. **Add Signals to Waveform**  
   - In the simulation window, select all signals (A, B, Operation, ALU_Out, CarryOut).  
   - Right-click → **Add to → Wave → Selected Signals**.  

7. **Run Simulation**  
   - In the simulation console, type:  
     ```
     run 100ns
     ```  
   - Or use the **Run button** to observe waveforms.  

8. **Analyze Waveforms**  
   - Verify the outputs of the ALU for each enumerated operation.  
   - Check that addition, subtraction, logical operations, and shifts are working correctly.  

9. **Save Results**  
   - Save the waveform (`.wlf` file) for documentation.  

---

## SystemVerilog Code  

### ALU Design (`alu_enum.sv`)
```systemverilog
// Write your ALU design code here using
// - Enumerated Data Types for ALU operations
// - Case Statements for operation selection
// ========================================================
// ALU Design using Enumerated Data Types and Case Statements
// ========================================================

typedef enum logic [2:0] {
    ADD = 3'b000,
    SUB = 3'b001,
    AND = 3'b010,
    OR  = 3'b011,
    XOR = 3'b100,
    NOT = 3'b101,
    SHL = 3'b110,
    SHR = 3'b111
} alu_ops_t;

module alu_enum #(parameter WIDTH = 4)(
    input logic [WIDTH-1:0] A,
    input logic [WIDTH-1:0] B,
    input alu_ops_t operation,
    output logic [WIDTH-1:0] ALU_Out,
    output logic CarryOut
);

logic [WIDTH:0] tmp;

always_comb begin
    tmp = '0;

    case(operation)
        ADD: tmp = A + B;
        SUB: tmp = A - B;
        AND: tmp = {1'b0, A & B};
        OR : tmp = {1'b0, A | B};
        XOR: tmp = {1'b0, A ^ B};
        NOT: tmp = {1'b0, ~A};
        SHL: tmp = {1'b0, A << 1};
        SHR: tmp = {1'b0, A >> 1};
        default: tmp = '0;
    endcase
end

assign ALU_Out = tmp[WIDTH-1:0];
assign CarryOut = tmp[WIDTH];

endmodule
```
---

### ALU Testbench (`alu_tb.sv`)
```systemverilog

// Write your ALU testbench code here
// ========================================================
// Testbench for ALU using Enumerated Data Types
// ========================================================

module alu_enum_tb;

logic [3:0] A, B;
alu_ops_t operation;

logic [3:0] ALU_Out;
logic CarryOut;

alu_enum #(4) uut(
    .A(A),
    .B(B),
    .operation(operation),
    .ALU_Out(ALU_Out),
    .CarryOut(CarryOut)
);

initial begin

    A = 4'b0011; B = 4'b0001; operation = ADD; #10;
    A = 4'b0100; B = 4'b0001; operation = SUB; #10;
    A = 4'b1010; B = 4'b1100; operation = AND; #10;
    A = 4'b1010; B = 4'b1100; operation = OR;  #10;
    A = 4'b1010; B = 4'b1100; operation = XOR; #10;
    A = 4'b1010; B = 4'b0000; operation = NOT; #10;
    A = 4'b0011; B = 4'b0000; operation = SHL; #10;
    A = 4'b1000; B = 4'b0000; operation = SHR; #10;

    $stop;

end

endmodule
```
---

### Simulation Output

<img width="1917" height="1143" alt="Screenshot 2026-07-28 132830" src="https://github.com/user-attachments/assets/13ba2fe4-2dbd-4cbf-adb8-9375cc8537b2" />


---

### Result

The design and simulation of a 4-bit ALU using Enumerated Data Types and Case Statements in SystemVerilog HDL was successfully carried out in Synopsys VCS and DVE
The ALU performed arithmetic, logical, and shift operations correctly as verified from the simulation outputs.

