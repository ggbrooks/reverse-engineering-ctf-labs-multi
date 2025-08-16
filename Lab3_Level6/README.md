# Lab 3 – Reverse Engineering Challenge

## Goal
Analyze a Linux ELF 64-bit binary (`level6`) and recover the hidden flag by tracing the password check and manipulating execution in GDB.

---

## Tools Used
- **Ghidra** – static analysis (string search, function references)  
- **GDB** – dynamic debugging, breakpoints, and manual function calls  

---

## Process (Summary)
1. **Initial Analysis**  
   - Searched strings in the binary and located the "congrats" success message.  
   - Traced references to the message, which led to the `check_password` function 

2. **Binary Execution & Debugging**  
   - Made the binary executable with `chmod +x level6`.  
   - Launched GDB with fake input:  
     ```bash
     gdb --args ./level6 ggb_fake_flag_
     ```  

3. **Breakpoint Setup**  
   - Set a breakpoint where the `memcpy` call copies the user input into the stack.  
   - This made sure the flag was present on the stack before comparison.  

4. **Function Call Manipulation**  
   - Used GDB’s `call` statement to manually invoke the `encrypt_decrypt` function with adjusted arguments:  
     ```gdb
     call (void*) 0x4016fe($rbp-0x50, 0x2d, 0x4d03226f0ddc17eb)
     ```  
   - Modified the operand `-0x30` → `-0x50` to point to the **real flag’s decrypted value**.  
   - Continued execution with the patched call.  

5. **Flag Extraction**  
   - Printed the string at the stack location with:  
     ```gdb
     x/s $rbp-0x50
     ```  
   - Extracted the real flag from memory successfully.  

---

## Result
- Flag was decrypted and extracted from memory at runtime.  
- Program validated the recovered flag and displayed the success message.  

---

## Key Takeaways
- Identified the success condition by searching for strings and cross-references.  
- Used GDB to manipulate function calls and adjust memory operands.  
- Combined **static string analysis** with **dynamic memory inspection** to recover the hidden flag.  

> Full detailed report with screenshots and notes: `Lab3_reverse_engineering.docx`

