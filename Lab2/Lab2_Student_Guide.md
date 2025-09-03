# Lab 2: Modular RTL Design - Student Guide

## Learning Objectives

By completing this lab, you will:
- **Master the development workflow**: GitHub → VS Code → Testing → Hardware  
- **Create modular Verilog designs**: Build individual modules and connect them hierarchically
- **Practice systematic testing**: Test individual modules, then integrated systems
- **Understand RTL design thinking**: Use block diagrams to guide code structure

---

## TL;DR - Quick Overview

**What you're building**: Four simple logic functions in separate modules, connected together to create a complete system that displays results on LEDs.

**What you need to do**:
**Pre-Lab**: Complete individual Verilog modules and test them
**Lab**: Connect modules together, integrate system, program Basys3 hardware



---

## Section A: Pre-Lab Preparation

### TL;DR: "Complete Before Coming to Lab"
✅ Watch Videos 1 & 2 (Lab overview and VS Code workflow)

✅ Clone repository to VS Code/Box (GitHub Desktop may solve authentication issues)

✅ Verify tools work with `make help` 

✅ Complete missing Verilog modules (`logic_func1.v` and `logic_func3.v`)

✅ Test individual modules work correctly

✅ Review expected results and understand the four functions  



### Step 1: Watch Preparation Videos

**Video 1: Lab2 Introduction** <a href="https://www.youtube.com/@MichaelT-ux5ux" target="_blank" rel="noopener noreferrer">
<a href="https://www.youtube.com/watch?v=SpQ7vx6j0cw" target="_blank" rel="noopener noreferrer">
  <img src="/images/Lab2_intro_thumbnail.jpg" alt="Link to Lab 2 Introduction Video" style="max-width:560px; width:100%;">
</a></a></a>



- Overview of the four logic functions you'll implement
- Understanding RTL diagrams and block-level thinking
- Expected results and verification approach
- Hardware vs programming mindset

**Video 2: RTL Modules and Testbenches** 
- VS Code workflow and terminal usage
- Module structure and syntax
- Individual module testing with Make commands
- Waveform analysis basics

### Step 2: Get Your Repository  

Follow the same workflow from Lab 1:
1. Accept assignment from GitHub Classroom  
2. Clone to your Box folder using VS Code
3. Open terminal in VS Code (Terminal → New Terminal)
4. Run `make help` to verify tools work

### Step 3: Understand What You're Building

You're implementing **four logic functions** that take the same 4 inputs (A, B, C, D) but calculate different outputs:

- **F0**: AND-OR function: `(A & B) | (C & D)` 
- **F1**: XOR parity: `A ^ B ^ C ^ D` (outputs 1 when odd number of inputs are 1)
- **F2**: Majority function: outputs 1 when 2+ inputs are 1
- **F3**: NAND function: `~((A & B) & (C | D))`

### Step 4: Expected Results Reference

**Complete Truth Table** (use this to verify your work):

| A B C D | F3 F2 F1 F0 | HEX |
|---------|-------------|-----|
| 0 0 0 0 |  1  0  0  0 |  8  |
| 0 0 0 1 |  1  0  1  0 |  a  |
| 0 0 1 0 |  1  0  1  0 |  a  |
| 0 0 1 1 |  1  1  0  1 |  d  |
| 0 1 0 0 |  1  0  1  0 |  a  |
| 0 1 0 1 |  1  1  0  0 |  c  |
| 0 1 1 0 |  1  1  0  0 |  c  |
| 0 1 1 1 |  1  1  1  1 |  f  |
| 1 0 0 0 |  1  0  1  0 |  a  |
| 1 0 0 1 |  1  1  0  0 |  c  |
| 1 0 1 0 |  1  1  0  0 |  c  |
| 1 0 1 1 |  1  1  1  1 |  f  |
| 1 1 0 0 |  1  1  0  1 |  d  |
| 1 1 0 1 |  0  1  1  1 |  7  |
| 1 1 1 0 |  0  1  1  1 |  7  |
| 1 1 1 1 |  0  1  0  1 |  5  |

**For detailed Verilog syntax and concepts**: See [Lab2_Background.md](Lab2_Background.md)

### Step 5: Complete Individual Modules (Pre-Lab)

#### Task 1: Complete the XOR Parity Function


**File to edit**: `rtl/logic_func1.v`

**What you'll find**: A module with missing assign statement
```verilog
// TODO: Implement F1 = w ^ x ^ y ^ z (XOR parity function)
// assign F1 = ???;
```

**What to do**: Replace the `???` with the correct XOR expression

**Test your work**: 
```bash
make test-func1
```

**Expected result**: Truth table showing F1=1 for odd number of 1's in inputs

#### Task 2: Write Complete NAND Function Module  

**File to edit**: `rtl/logic_func3.v`

**What you'll find**: Only comments - you write the entire module!

**What to do**: Create complete module implementing `F3 = ~((A & B) & (C | D))`
- Module name: `logic_func3`  
- Input ports: `in_a, in_b, in_c, in_d`
- Output port: `F3`

**Pattern to follow**: Look at `rtl/logic_func0.v` for module structure example

**Test your work**:
```bash  
make test-func3
```

**Expected result**: F3=0 only when both (A & B)=1 AND (C | D)=1

#### Pre-Lab Verification

**Test individual modules**:
```bash
make test-func1  # Test your XOR function
make test-func3  # Test your NAND function
make test-func0  # Verify provided example works
make test-func2  # Verify provided example works
```

**Success criteria**: All individual tests show correct truth table outputs

---

## Section B: Lab Activities  

### TL;DR: "What to do during lab time"
✅ Watch Video 3 (Module instantiation guidance)
1. Complete module instantiations in two top-level files
2. Test integrated system works correctly
3. Import to Vivado and program Basys3 hardware
4. Validate hardware matches simulation results

### Task 1: Complete Top-Level Integration

**First: Watch Video 3 - Instantiating the Top**
- Learn module instantiation patterns and syntax
- Understand how RTL diagrams guide port connections
- See examples of connecting different port names

**Files to edit**: 
- `rtl/logic_functions_top.v` (connect individual modules)
- `rtl/logic_functions_wrapper.v` (connect top to hardware)

**What you'll find**: Commented instantiation templates with `???` placeholders

**What to do**: Uncomment and complete the module instantiations by connecting the right signals

**Key insight**: Use the RTL diagram (see Background Reading) and Video 3 examples to understand connections

### Task 2: Test Integrated System

**Test individual modules**:
```bash
make test-individual  # Tests all four functions
```

**Test complete system**:
```bash  
make sim  # Tests integrated system
```

**View waveforms**:

```bash
make view  # Opens a waveform viewer.  If this fails, directly click on *.vcd files in the waveforms directory
```

**Success criteria**: All tests show correct truth table outputs with no 'z' (undefined) values

### Task 3: Hardware Implementation

**Import to Vivado**:
1. Open Vivado and create new project
2. Add all files from `/rtl` directory
3. Add all files from the `/testbench` directory
4. Run simulations within 
5. Set `logic_functions_wrapper` as top-level module (Vivado might auto-detect this)
6. Apply Basys3 constraints (switches and LEDs)

**Program Basys3**:
1. Synthesize and implement design
2. Generate bitstream
3. Program FPGA board

**Hardware Validation**:
1. Use switches SW3-SW0 for inputs A,B,C,D
2. Observe LEDs LD3-LD0 for outputs F3,F2,F1,F0
3. Verify hardware results match simulation truth table
4. Test several input combinations to confirm operation

---

## Section C: Deliverables

### TL;DR: "What to submit"

**Pre-Lab Completion Evidence**:
- `rtl/logic_func1.v` - XOR function with correct assign statement
- `rtl/logic_func3.v` - Complete NAND function module  
- Come to lab prepared for a quiz over writing the logic modules and the background reading.

**Lab Session Deliverables**:

- `rtl/logic_functions_top.v` - Completed instantiations
- `rtl/logic_functions_wrapper.v` - Completed top-level connection
- Screenshot of `make sim` showing complete integrated system truth table
- Working Basys3 hardware demonstration with switches and LEDs
- Brief reflection (1-2 sentences) on what you learned about module instantiation

---

## Quick Reference Commands

```bash
make help              # Show all available commands
make test-func0        # Test function 0 only
make test-func1        # Test function 1 only  
make test-func2        # Test function 2 only
make test-func3        # Test function 3 only
make test-individual   # Test all individual functions
make sim              # Test complete integrated system
make view             # View waveforms in GTKWave
make clean            # Remove generated files
```

---

## Getting Help

**If you get stuck**:

1. **Check error messages carefully** - they usually point to the exact problem
2. **Compare with working examples** - `logic_func0.v` and `logic_func2.v` are complete
3. **Test incrementally** - complete one task at a time
4. **Use the reference materials** - see Lab2_Background.md for detailed explanations
5. **Check port names match exactly** - each module uses different port naming

**Common issues**:
- "Unknown module type" → You need to write the complete module
- All outputs show 'z' → Missing or commented assign statement  
- Port connection errors → Check port names match the module definition exactly

---

## What's Next

This lab establishes the foundation for more complex digital systems. The modular design principles you're learning here will scale up to processors, communication interfaces, and other sophisticated hardware designs.

**Key concepts mastered**:
- Hardware description vs. programming mindset
- Module definition and instantiation patterns  
- Systematic testing and verification workflow
- RTL design thinking and visual debugging

For complete technical details and extended explanations, see [Lab2_Background.md](Lab2_Background.md).