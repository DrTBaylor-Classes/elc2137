# Composite Logic Functions Truth Table

Based on the simulation results, here's the complete truth table for the composite logic system:

## **Composite Logic Functions Truth Table**

| Input Vector | A | B | C | D | F3 | F2 | F1 | F0 | Hex Output |
|--------------|---|---|---|---|----|----|----|----|-----------|
| 0000         | 0 | 0 | 0 | 0 | 1  | 0  | 0  | 0  | **8**     |
| 0001         | 0 | 0 | 0 | 1 | 1  | 0  | 1  | 0  | **A**     |
| 0010         | 0 | 0 | 1 | 0 | 1  | 0  | 1  | 0  | **A**     |
| 0011         | 0 | 0 | 1 | 1 | 1  | 1  | 0  | 1  | **D**     |
| 0100         | 0 | 1 | 0 | 0 | 1  | 0  | 1  | 0  | **A**     |
| 0101         | 0 | 1 | 0 | 1 | 1  | 1  | 0  | 0  | **C**     |
| 0110         | 0 | 1 | 1 | 0 | 1  | 1  | 0  | 0  | **C**     |
| 0111         | 0 | 1 | 1 | 1 | 1  | 1  | 1  | 1  | **F**     |
| 1000         | 1 | 0 | 0 | 0 | 1  | 0  | 1  | 0  | **A**     |
| 1001         | 1 | 0 | 0 | 1 | 1  | 1  | 0  | 0  | **C**     |
| 1010         | 1 | 0 | 1 | 0 | 1  | 1  | 0  | 0  | **C**     |
| 1011         | 1 | 0 | 1 | 1 | 1  | 1  | 1  | 1  | **F**     |
| 1100         | 1 | 1 | 0 | 0 | 1  | 1  | 0  | 1  | **D**     |
| 1101         | 1 | 1 | 0 | 1 | 0  | 1  | 1  | 1  | **7**     |
| 1110         | 1 | 1 | 1 | 0 | 0  | 1  | 1  | 1  | **7**     |
| 1111         | 1 | 1 | 1 | 1 | 0  | 1  | 0  | 1  | **5**     |

## **Individual Function Definitions:**

- **F0**: `(A & B) | (C & D)` - AND-OR logic
- **F1**: `A ^ B ^ C ^ D` - XOR parity (odd number of 1s)
- **F2**: Majority function - outputs 1 when ≥2 inputs are 1
- **F3**: `~((A & B) & (C | D))` - NAND-based function

## **Key Observations:**

### **Hex Pattern Distribution:**
- **8**: 1 occurrence (all inputs low except F3)
- **A**: 4 occurrences (most common pattern)
- **C**: 4 occurrences 
- **D**: 2 occurrences
- **F**: 2 occurrences (all functions high)
- **7**: 2 occurrences (F3 low, others high)
- **5**: 1 occurrence (F1 low, others mixed)

### **Notable Patterns:**
- **F3 is LOW only for**: 1101, 1110, 1111 (when A=1, B=1, and C|D=1)
- **All functions HIGH (F)**: 0111, 1011 (both have exactly 3 inputs high)
- **F1 parity**: Always 1 when odd number of inputs (1,3) are high
- **F2 majority**: Always 1 when 2 or more inputs are high

This truth table demonstrates the educational value of combining different logic function types in a single system.