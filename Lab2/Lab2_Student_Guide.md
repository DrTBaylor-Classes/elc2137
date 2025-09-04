# Lab 2: Modular RTL Design – Student Guide (Updated for Two-Week Plan)

> **Schedule Update**
> - **Week 1 (today, in lab):** Start the activity in lab. Complete the two missing logic-function modules and get as far as you can with integration.
> - **Week 2 (next lab):** Finish integration (during the week and before lab) and hardware testing. The **first half of lab** is reserved for **instructor sign‑offs** on working circuits. Note that only the first hour of lab for next week is allocated for sign-offs.  

---

## Learning Objectives
By completing this lab, you will:
- **Master the development workflow**: GitHub → VS Code → Testing → Hardware  
- **Create modular Verilog designs**: Build individual modules and connect them hierarchically
- **Practice systematic testing**: Test individual modules, then integrated systems
- **Understand RTL design thinking**: Use block diagrams to guide code structure

---

## TL;DR – Quick Overview

**What you're building**: Four simple logic functions in separate modules, connected together to create a complete system that displays results on LEDs.

**What you need to do** (with the new schedule):
- **Week 1 (in lab):** Complete individual Verilog modules and test them; start top‑level integration.
- **Week 2 (in lab):** Finish integration, program the Basys3 hardware, and complete **sign‑off** during the **first half** of lab.

---

## Week 1 (In‑Lab) – Foundations and Individual Modules

### Step 1: Watch Preparation Videos (during lab if not already viewed)
**Video 1: Lab2 Introduction**      
<a href="https://www.youtube.com/watch?v=SpQ7vx6j0cw" target="_blank" rel="noopener noreferrer">Click here to watch the Intro video for Lab 2</a>

- Overview of the four logic functions you'll implement
- Understanding RTL diagrams and block‑level thinking
- Expected results and verification approach
- Hardware vs programming mindset

**Video 2: RTL Modules and Testbenches**  
<a href="https://www.youtube.com/watch?v=qEU6_KaPJPU" target="_blank" rel="noopener noreferrer">Click here to watch the RTL Modules and Testbenches Video for Lab 2</a>

- VS Code workflow and terminal usage
- Module structure and syntax
- Individual module testing with Make commands
- Waveform analysis basics

### Step 2: Get Your Repository
Follow the same workflow from Lab 1:
1. Accept the assignment from GitHub Classroom  [Lab 2 Link](https://classroom.github.com/a/zHIdkquL)
2. Clone to your Box folder using VS Code or GitHub Desktop.  If this fails, simply download the template files to your Box folder. 
3. Open the VS Code terminal (Terminal → New Terminal)
4. Run `make help` to verify tools work

### Step 3: Understand What You're Building
You’re implementing **four logic functions** that take the same 4 inputs (A, B, C, D) but calculate different outputs:

- **F0**: AND‑OR function: `(A & B) | (C & D)` 
- **F1**: XOR parity: `A ^ B ^ C ^ D` (outputs 1 when odd number of inputs are 1)
- **F2**: Majority function: outputs 1 when 2+ inputs are 1
- **F3**: NAND function: `~((A & B) & (C | D))`

#### Expected Results Reference (Truth Table)
Use this to verify your work.

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

*See also: `Lab2_Background.md` for detailed Verilog syntax and concepts.*

### Step 4: Complete Individual Modules (Week 1 Goals) in VS-Code

#### Task 1: Complete the XOR Parity Function
**File to edit**: `rtl/logic_func1.v`

You’ll find a module with a missing assign statement:
```verilog
// TODO: Implement F1 = w ^ x ^ y ^ z (XOR parity function)
// assign F1 = ???;
```
**Action**: Replace `???` with the correct XOR expression.

**Test your work**: 
```bash
make test-func1
```
**Expected result**: Truth table showing F1=1 for odd number of 1's in inputs.

#### Task 2: Write the NAND Function Module
**File to edit**: `rtl/logic_func3.v`

This file contains only comments—you’ll write the entire module implementing `F3 = ~((A & B) & (C | D))`.
- Module name: `logic_func3`  
- Inputs: `in_a, in_b, in_c, in_d`
- Output: `F3`

**Pattern to follow**: See `rtl/logic_func0.v` for structure.

**Test your work**:
```bash  
make test-func3
```
**Expected result**: F3=0 only when both (A & B)=1 AND (C | D)=1.

#### Week 1 Checkpoint (End of Lab)
- Both individual modules (`logic_func1.v`, `logic_func3.v`) compile and pass their truth‑table tests.  
- You’ve run:
  ```bash
  make test-func1
  make test-func3
  make test-func0
  make test-func2
  ```
- Begin reviewing and implementing the top‑level integration files so you’re ready to finish next week.

---

## Week 2 (In‑Lab) – Integration, System Test, and Hardware Sign‑Offs

### Task 1: Complete Top‑Level Integration (either VS-Code or Vivado)
**Files to edit**: 
- `rtl/logic_functions_top.v` (connect individual modules)
- `rtl/logic_functions_wrapper.v` (connect the top to hardware)

**What you’ll find**: Commented instantiation templates with `???` placeholders.  
**Action**: Uncomment and complete the instantiations, connecting the correct signals.

**Key insight**: Use the RTL diagram from the background reading and examples from Video 3.

**(Optional) Video 3: Instantiating the Top**
- Module instantiation patterns and syntax
- How RTL diagrams guide port connections
- Examples of connecting different port names

### Task 2: Test the Integrated System
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
make view  # Opens a waveform viewer (or open *.vcd files in /waveforms)
```

**Success criteria**: All tests show correct truth table outputs with no 'z' (undefined) values.

### Task 3: Hardware Implementation & **Sign‑Offs (First Half of Lab)**
**Import to Vivado**:
1. Create a new project
2. Add all files from `/rtl`
3. Add all files from `/testbench` (for in‑Vivado sim if desired)
4. Set `logic_functions_wrapper` as top (Vivado may auto‑detect)
5. Apply Basys3 constraints (switches and LEDs)

**Program the Basys3**:
1. Synthesize and implement
2. Generate bitstream
3. Program the FPGA

**Hardware Validation (for sign‑off)**:
1. Use switches **SW3–SW0** for inputs **A,B,C,D**
2. Observe LEDs **LD3–LD0** for outputs **F3,F2,F1,F0**
3. Verify the hardware matches the truth table
4. Demonstrate several input combinations during the sign‑off

---

## Deliverables & Grading

### Week 1 (End‑of‑Lab) – Checkpoint
- `rtl/logic_func1.v` – XOR function implemented and tested
- `rtl/logic_func3.v` – NAND function module implemented and tested
- Be prepared for a short quiz next week over writing the logic modules and the background reading

### Week 2 (By Sign‑Off Window) – Final Submission
- `rtl/logic_functions_top.v` – Completed instantiations
- `rtl/logic_functions_wrapper.v` – Completed top‑level connection
- Screenshot of `make sim` showing the integrated system truth table
- **Working Basys3 hardware demonstration** with instructor **sign‑off**


---

## Quick Reference Commands

```bash
make help              # Show all available commands
make test-func0        # Test function 0 only
make test-func1        # Test function 1 only  
make test-func2        # Test function 2 only
make test-func3        # Test function 3 only
make test-individual   # Test all individual functions
make sim               # Test complete integrated system
make view              # View waveforms in GTKWave
make clean             # Remove generated files
```

---

## Getting Help

**If you get stuck**:
1. **Read error messages carefully** – they often point to the exact problem
2. **Compare with working examples** – `logic_func0.v` and `logic_func2.v`
3. **Test incrementally** – complete and verify one task at a time
4. **Use the reference materials** – see `Lab2_Background.md`
5. **Check port names exactly** – each module uses different port naming

**Common issues**:
- “Unknown module type” → You need to write/instantiate the module correctly
- All outputs show ‘z’ → Missing or commented assign statement  
- Port connection errors → Port names don’t match the module definition


---

## Why This Lab Matters
This lab establishes the foundation for more complex digital systems. The modular design principles you’re practicing here scale to processors, communication interfaces, and other sophisticated hardware designs.

**Key concepts**:
- Hardware description vs. programming mindset
- Module definition and instantiation patterns  
- Systematic testing and verification workflow
- RTL design thinking and visual debugging
