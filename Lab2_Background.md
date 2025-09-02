# Lab 2: Comprehensive Background Reading

## Table of Contents
- [Hardware vs Programming Mindset](#hardware-vs-programming-mindset)
- [Essential Verilog Concepts](#essential-verilog-concepts)  
- [RTL Design and Block Diagrams](#rtl-design-and-block-diagrams)
- [Development Workflow](#development-workflow)
- [Logic Function Details](#logic-function-details)
- [Testing and Debugging](#testing-and-debugging)
- [Tool Usage Guide](#tool-usage-guide)

---

## Hardware vs Programming Mindset

### Key Insight from Video Content

**"Verilog describes circuits, not programs."** This fundamental difference shapes how you think about digital design:

**Programming Mindset** (Sequential):
```
1. Execute instruction A
2. Then execute instruction B  
3. Then execute instruction C
```

**Hardware Mindset** (Parallel):
```
Circuit A, Circuit B, and Circuit C all operate simultaneously
Connections between circuits determine data flow
Everything happens in parallel unless explicitly controlled
```

### Why This Matters

In Verilog:
- All `assign` statements execute simultaneously
- Module instantiations create parallel hardware blocks
- Order of statements in code doesn't determine execution order
- Think in terms of "connections" not "sequences"

---

## Essential Verilog Concepts

### 1. Modules: The Building Blocks

A **module** is Verilog's fundamental unit - like a hardware function:

```verilog
module my_logic_gate (
    input  wire A,        // Input port
    input  wire B,        // Input port  
    output wire Y         // Output port
);

// Module functionality
assign Y = A & B;         // Combinational logic

endmodule
```

**Key Components**:
- `module` keyword starts the definition
- Port list defines inputs and outputs
- Logic implementation goes inside
- `endmodule` ends the definition

### 2. Wire vs Reg: Understanding Signal Types

**`wire`** - Physical connections:
- Represents actual wires in hardware
- Used for combinational logic outputs
- Driven by `assign` statements or module outputs
- Cannot store values (no memory)

**`reg`** - Variables for testbenches:
- Used in testbenches to drive test signals
- Can be assigned in `initial` and `always` blocks  
- Despite the name, doesn't always create physical registers
- Think "variable" not "register"

```verilog
wire connection;          // Physical wire in design
reg  test_input;          // Test variable in testbench
assign connection = A & B; // Drive wire with logic
```

### 3. Vectors: Multi-bit Signals

Understanding **when** and **how** to use brackets is crucial:

#### Declaration vs Indexing Rule
**🔑 KEY RULE**: Brackets BEFORE name = Declaration, Brackets AFTER name = Access

#### Vector Declaration (Brackets BEFORE name)
When you **define** a vector:
```verilog
input  wire [3:0] data_bus;     // 4-bit input vector
output wire [7:0] result_bus;   // 8-bit output vector  
wire [15:0] internal_signal;    // 16-bit internal wire
```

**What `[3:0]` means**:
- 4 bits wide (bits 3, 2, 1, 0)
- Bit 3 is MSB (Most Significant Bit)
- Bit 0 is LSB (Least Significant Bit)

#### Vector Indexing (Brackets AFTER name)
When you **access** parts of a vector:
```verilog
wire [3:0] data_bus;

// Access individual bits
wire msb_bit = data_bus[3];     // Get MSB
wire lsb_bit = data_bus[0];     // Get LSB

// Access bit ranges  
wire [1:0] upper = data_bus[3:2];   // Get bits 3,2
wire [1:0] lower = data_bus[1:0];   // Get bits 1,0
```

#### Memory Aid: "Dec-BEFORE, In-AFTER"
- **DEC**laration = Brackets **BEFORE** name
- **IN**dexing = Brackets **AFTER** name

### 4. Module Instantiation

Connecting modules creates hierarchical designs:

```verilog
// Pattern: module_name instance_name (port_connections);
my_logic_gate gate_inst (
    .A(switch_1),         // Connect port A to switch_1
    .B(switch_2),         // Connect port B to switch_2  
    .Y(led_output)        // Connect port Y to led_output
);
```

**Port Connection Syntax**:
- `.port_name(signal_name)` connects module port to external signal
- Each instance needs unique name (`gate_inst`)
- Order doesn't matter - connections are by name

---

## RTL Design and Block Diagrams

### From Video: "RTL Diagrams Tell You How to Code"

RTL (Register Transfer Level) diagrams show:
- **Modules as blocks** (functional level, not gate level)
- **Signal connections** between modules
- **Input/output relationships**

### Reading the Lab2 RTL Diagram

From the provided RTL diagram, you can see:

**Top Level Structure**:
```
logic_functions_top
├── logic_func3 (ports: in_a, in_b, in_c, in_d → F3)
├── logic_func2 (ports: inputs[3:0] → F2)  
├── logic_func1 (ports: w, x, y, z → F1)
└── logic_func0 (ports: p, q, r, s → F0)
```

**Signal Flow**:
- `in_vec[3:0]` feeds all four modules
- Each module has different port names (intentionally!)
- Outputs combine into `out_vec[3:0]`

### Color Coding Convention
- **Red**: Inputs
- **Blue**: Outputs  
- **Yellow/Gold**: Internal wires

### Using Diagrams to Write Code

The RTL diagram directly translates to instantiation code:
```verilog
// From diagram: func0 has ports p,q,r,s and output F0
logic_func0 func0_inst (
    .p(in_vec[3]),        // Connect p to MSB of input
    .q(in_vec[2]),        
    .r(in_vec[1]),
    .s(in_vec[0]),        // Connect s to LSB of input
    .F0(out_vec[0])       // Connect output to LSB of output
);
```

---

## Development Workflow

### From Videos: The Complete Process

**Phase 1: Setup** (from Lab1 workflow)
1. Accept GitHub Classroom assignment  
2. Clone repository to VS Code/Box
3. Open terminal in VS Code

**Phase 2: Individual Module Development**
1. Examine module templates
2. Complete missing assign statements  
3. Test individual modules: `make test-func1`
4. View waveforms if needed: `make view`

**Phase 3: Integration**
1. Complete module instantiations using RTL diagrams
2. Test integrated system: `make sim`
3. Debug any connection issues

**Phase 4: Hardware Validation** (optional)
1. Import to Vivado
2. Program Basys3
3. Validate with switches and LEDs

### Tool Integration: VS Code Workflow

**Terminal Commands** (from Video 2):
```bash
make help              # See available commands
make test-func0        # Test individual function
make sim              # Test complete system  
make view             # View waveforms
make clean            # Clean up files
```

**File Organization**:
- `rtl/` - Design files (synthesizable Verilog)
- `testbench/` - Test files (simulation only)
- `waveforms/` - Generated VCD files
- `Makefile` - Build automation

---

## Logic Function Details

### Function Descriptions and Examples

**F0: AND-OR Function** `(A & B) | (C & D)`
- Output 1 when: (both A,B are 1) OR (both C,D are 1)  
- Example: A=1, B=1, C=0, D=0 → F0=1 (because A&B=1)
- Example: A=0, B=0, C=1, D=1 → F0=1 (because C&D=1)

**F1: XOR Parity Function** `A ^ B ^ C ^ D`
- Output 1 when: odd number of inputs are 1
- Example: A=1, B=0, C=1, D=0 → F1=0 (2 ones = even)
- Example: A=1, B=0, C=1, D=1 → F1=1 (3 ones = odd)
- Use: Error detection in digital communications

**F2: Majority Function** 
- Output 1 when: 2 or more inputs are 1
- Implementation: `(A&B) | (A&C) | (A&D) | (B&C) | (B&D) | (C&D)`
- Example: A=1, B=1, C=0, D=0 → F2=1 (2 or more)
- Use: Fault-tolerant voting systems

**F3: NAND Function** `~((A & B) & (C | D))`  
- Output 0 only when: (A=1 AND B=1) AND (C=1 OR D=1)
- All other cases → F3=1
- Demonstrates: De Morgan's laws and NAND universality
- NAND gates can implement any Boolean function

### Truth Table Analysis

**Pattern Recognition**:
- F1 alternates based on parity (count of 1's)
- F2 increases with number of 1's in input
- F3 is mostly 1's (only three 0's in entire table)
- F0 follows clear AND-OR pattern

---

## Testing and Debugging

### Individual Module Testing

Each function has dedicated testbench:
```verilog
// Testbench pattern
module tb_logic_func0;
    reg A, B, C, D;           // Test inputs (regs for assignment)
    wire F0;                  // Test output (wire)
    
    // Instantiate module under test
    logic_func0 uut (
        .p(A), .q(B), .r(C), .s(D),  // Connect test signals
        .F0(F0)
    );
    
    // Test all combinations
    initial begin
        // VCD dump for waveforms
        $dumpfile("waveforms/tb_logic_func0.vcd");
        $dumpvars(0, tb_logic_func0);
        
        // Test vectors
        {A,B,C,D} = 4'b0000; #10;
        {A,B,C,D} = 4'b0001; #10;
        // ... all 16 combinations
    end
endmodule
```

### System Integration Testing

Complete system testbench tests:
- All 16 input combinations
- Vector-to-vector connections
- Hex output formatting
- Module hierarchy

### Waveform Analysis

VCD files show:
- **Input patterns**: All 16 combinations cycling
- **Individual outputs**: F0, F1, F2, F3 timing
- **Vector buses**: 4-bit input/output patterns
- **Propagation delays**: How signals flow through logic

**GTKWave Usage**:
1. `make view` opens waveform viewer
2. Add signals by double-clicking  
3. Zoom and scroll to analyze timing
4. Verify outputs match expected truth table

---

## Tool Usage Guide

### VS Code Integration

**Terminal Setup**:
- Mac: Terminal → New Terminal
- PC: Similar menu option
- Creates interactive command line at bottom of VS Code

**File Navigation**:
- Use sidebar file explorer
- RTL files in `rtl/` directory
- Testbenches in `testbench/` directory

### Makefile System

**Why Makefiles**: VS Code "Run" buttons don't work with Verilog - need custom build process

**Key Targets**:
```bash
make help              # Always start here
make test-func0        # Individual function tests
make test-individual   # All individual tests  
make sim              # Complete system test
make view             # Waveform viewer
make clean            # Remove generated files
```

**Build Process**:
1. `iverilog` compiles Verilog files
2. `./sim_out` runs simulation
3. VCD files saved to `waveforms/`
4. GTKWave opens for waveform viewing

### Common Error Patterns

**"Unknown module type"**:
- Module not defined or file missing
- Check spelling of module name
- Verify file is in correct directory

**"Multiple drivers"**:  
- Two `assign` statements driving same wire
- Check for duplicate assignments

**Port connection mismatches**:
- Port names must match exactly (case-sensitive)
- Use RTL diagram to verify connections
- Each module uses different port naming

**Undefined outputs ('z' values)**:
- Missing assign statement
- Commented code not uncommented
- Logic expression syntax error

### Debugging Strategy

1. **Start simple**: Test individual modules first
2. **Read error messages**: They usually point to exact problem
3. **Compare with working examples**: Use logic_func0.v as reference
4. **Use truth tables**: Verify outputs match expected values
5. **Check waveforms**: Visual debugging often reveals issues
6. **Incremental testing**: Fix one module before moving to next

---

## Integration with Hardware

### Wrapper Concept (from Video)

**Hardware-Agnostic Design**: `logic_functions_top`
- Works on any FPGA platform
- Focuses on logic functionality
- Uses generic input/output vectors

**Hardware-Specific Wrapper**: `basys3_wrapper`  
- Maps generic signals to specific pins
- Handles Basys3 switch and LED connections
- Contains display decoding logic

**Why Separate**:
- **Portability**: Same logic works on different boards
- **Testing**: Test logic separate from pin assignments  
- **Modularity**: Clean separation of concerns

### Physical Mapping

**Basys3 Connections**:
- `sw[3:0]` → Switches SW3-SW0 → A,B,C,D inputs
- `led[3:0]` → LEDs LD3-LD0 → F3,F2,F1,F0 outputs  
- Seven-segment displays show hex values

**Expected Hardware Behavior**:
- Flip switches to set input patterns
- LEDs show function outputs immediately  
- Hex display shows combined 4-bit result

---

This background reading provides comprehensive coverage of all concepts needed for Lab2. Refer back to specific sections as needed while working through the main student guide.