# Lab 4 – Reverse Engineering Challenge

## Goal
Reverse engineer a Linux ELF 64-bit binary (`level8`) to recover the hidden flag by rerouting execution so the real flag (not just the user input) is processed through the encryption

---

## Tools Used
- **Ghidra** – to analyze functions, stack variables, and the `check_password` routine  
- **Cutter** – to patch the assembly instruction that controlled which buffer went to `encrypt_decrypt`  
- **EDB** – to attach to the running binary, set breakpoints, and inspect memory  

---

## Process
1. **Static analysis in Ghidra**  
   - Located the `check_password` function and identified where the real flag was stored (`local_68`).  
   - Noted stack offsets for both the user input copy (`rbp-0x30`) and the real flag buffer (`rbp-0x60`):contentReference[oaicite:0]{index=0}.  

2. **Patching with Cutter**  
   - The program originally sent the user input (`rbp-0x30`) into the `encrypt_decrypt` function.  
   - Patched the instruction so it instead passed the real flag (`rbp-0x60`):  
     ```asm
     ; original
     lea rax, [rbp - 0x30]

     ; patched
     lea rax, [rbp - 0x60]
     ```

3. **Debugging the binary**  
   - Set a breakpoint inside the loop to catch execution.  
   - Ran `./level8_copy` with a fake flag to trigger the loop.  
   - Attached to the process in EDB and inspected memory while execution was paused.  

4. **Extracting the flag**  
   - Found the decrypted flag in the memory dump.  
   - Ran the binary again with the recovered flag and confirmed success with the congrats message.  

---

## Result
- Successfully patched the operand so the real flag passed through `encrypt_decrypt`.  
- Recovered the decrypted flag from memory and verified it worked in the binary.  

---

## Takeaways
- Gained practice mapping stack variables to see how user input vs. the real flag were handled.  
- Learned how a single operand change (`rbp-0x30` → `rbp-0x60`) can redirect program logic.  
- Combined static analysis, binary patching, and live debugging to solve the challenge.  

> Full write-up with screenshots and details: `Lab4_reverse_engineering.docx`

