# Experiment 1: ALU Design using Enumerated Data Types and Case Statements

---

## Aim  
To design and simulate a **4-bit Arithmetic Logic Unit (ALU)** using **SystemVerilog HDL** with **Enumerated Data Types and Case Statements**, and verify its functionality using **ModelSim 2020.1**.

---

## Apparatus Required  
- Computer with **Windows** OS  
- **MobaXterm** (for remote terminal access)
- **Synopsys VCS and DVE** (accessed via college server/license) 
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
- Compatible with **Synopsys VCS & DVE**  

---

## Procedure  

1. **Connect to the Server via MobaXterm**  
   - Open MobaXterm and log in using the college-provided license email ID and password.  
   - Run the `xdg-open` command to access the file system and navigate through the folders.  

2. **Create a Working Directory**  
   - Create a new folder for the project.  
   - Inside this folder, create the design file and the testbench file `ALU.sv`.  

3. **Set Up the Simulation Environment**  
   - Open a terminal session (bash).  
   - Source the Synopsys VCS environment setup script (e.g.,`source /synopsys/start.sh`).  

4. **Compile the Design and Testbench**  
   - Run the following command to compile the SystemVerilog files : `vcs -full64 -sverilog ALU.sv`
   - Ensure there are no syntax or compilation errors.  

5. **Run the Simulation**  
   - Execute the compiled simulation binary : `./simv`

6. **Launch DVE (Discovery Visualization Environment)**  
   - Open the waveform viewer : `dve -full64`
   - In the DVE window, go to `File → Open Database`.  
   - Select and open the generated `.vcd`/dump file.  

7. **Add Signals to the Waveform Window**  
   - Right-click on the file/module in the hierarchy.  
   - Select **Add Wave → Add New Wave to Window** to display the signals.  

8. **Analyze Waveforms**  
   - Verify the outputs of the ALU for each enumerated operation.  
   - Check that addition, subtraction, logical operations, and shifts are working correctly.  

9. **Save Results**  
   - Save the waveform for documentation. 

---

## SystemVerilog Code  

### ALU Design (`alu_enum.sv`)
```systemverilog
//========================================================
// ALU Design using Enumerated Data Types and Case Statements
//========================================================

// Define Enumerated Data Type for ALU Operations
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

// Module declaration
module alu_enum #(parameter WIDTH = 4) (
    input  logic [WIDTH-1:0] A, B,
    input  alu_ops_t operation, // Enumerated operation selector
    output logic [WIDTH-1:0] ALU_Out,
    output logic CarryOut
);

    // -----------------------------------------
    // Internal signals
    // -----------------------------------------
    logic [WIDTH:0] tmp;

    // -----------------------------------------
    // ALU operation using case statement
    // -----------------------------------------
    always_comb begin
        tmp = '0;

        case (operation)
            ADD: tmp = A + B;
            SUB: tmp = A - B;
            AND: tmp = {1'b0, (A & B)};
            OR : tmp = {1'b0, (A | B)};
            XOR: tmp = {1'b0, (A ^ B)};
            NOT: tmp = {1'b0, (~A)};
            SHL: tmp = {1'b0, (A << 1)};
            SHR: tmp = {1'b0, (A >> 1)};
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

//========================================================
// Testbench for ALU using Enumerated Data Types
//========================================================

module alu_enum_tb;

    // -----------------------------------------
    // Testbench signals
    // -----------------------------------------
    logic [3:0] A, B;
    alu_ops_t operation;   // Enumerated operation selector
    logic [3:0] ALU_Out;
    logic CarryOut;

    // -----------------------------------------
    // Instantiate ALU
    // -----------------------------------------
    alu_enum #(4) uut (
        .A(A),
        .B(B),
        .operation(operation),
        .ALU_Out(ALU_Out),
        .CarryOut(CarryOut)
    );

    // -----------------------------------------
    // Apply test vectors
    // -----------------------------------------
    initial begin

        A = 4'b0011; B = 4'b0001; operation = ADD; #10;
        A = 4'b0100; B = 4'b0001; operation = SUB; #10;
        A = 4'b1010; B = 4'b1100; operation = AND; #10;
        A = 4'b1010; B = 4'b1100; operation = OR;  #10;
        A = 4'b1010; B = 4'b1100; operation = XOR; #10;
        A = 4'b1010; B = 4'b0000; operation = NOT; #10;
        A = 4'b0011; B = 4'b0000; operation = SHL; #10;
        A = 4'b1000; B = 4'b0000; operation = SHR; #10;

        $stop; // End of simulation
    end

endmodule
```
---

### Simulation Output

<img width="1917" height="1143" alt="Screenshot 2026-07-28 132830" src="https://github.com/user-attachments/assets/13ba2fe4-2dbd-4cbf-adb8-9375cc8537b2" />


---

### Result

The design and simulation of a 4-bit ALU using Enumerated Data Types and Case Statements in SystemVerilog HDL was successfully carried out in Synopsys VCS and DVE. 
The ALU performed arithmetic, logical, and shift operations correctly as verified from the simulation outputs.

