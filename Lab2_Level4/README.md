# Lab 2 – Reverse Engineering Challenge

## Goal
Analyze a Linux ELF 64-bit binary (`level4`) and recover the hidden flag

---

## Tools Used
- **Ghidra** – static analysis and function labeling of the binary  
- **GDB** – dynamic analysis and debugging (breakpoints, disassembly, memory inspection)  

---

## Process (Summary)
1. **Static Analysis with Ghidra**  
   - Examined the binary in Ghidra and commented functions, focusing on `check_password`.  
   - Identified storage of the flag string and relevant memory copy/compare operations.  

2. **Dynamic Analysis with GDB**  
   - Ran the binary with fake input (`gdb --args ./level4 gianna_fake_flag`).  
   - Set a breakpoint at `main` to track execution and account for runtime address changes.  
   - Disassembled the `check_password` function to locate the memory comparison.  
   - Added a breakpoint at the memory copy instruction and resumed execution.  

3. **Flag Extraction**  
   - Inspected memory at the breakpoint, where the real flag was being compared.  
   - Extracted the flag from memory successfully.  

---

## Result
- Flag successfully recovered
- Program confirmed success with the congratulatory message.  

---

## Key Takeaways
- Analyzed an **ELF 64-bit binary** using both static and dynamic techniques.  
- Learned to account for runtime address shifts by setting breakpoints dynamically.  
- Demonstrated flag extraction through memory inspection and validation.  

> Full write-up with screenshots and notes: `lab2_reverse_engin.docx`
 
