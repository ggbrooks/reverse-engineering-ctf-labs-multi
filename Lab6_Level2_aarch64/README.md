# Lab 6 – Reverse Engineering Challenge (AArch64)

## Goal
Reverse engineer a Linux ELF 64-bit AArch64 binary (`l2_aarch64`) in Kali Linux and recover the hidden flag by running it under QEMU and attaching with `gdb-multiarch`.

---

## Tools Used
- **Kali Linux VM** – environment where the lab was completed  
- **Ghidra** – to look at `main` and `check_password` functions  
- **QEMU (qemu-aarch64)** – to emulate an AArch64 CPU so the binary could run on x86 hardware  
- **GDB Multiarch (`gdb-multiarch`)** – to debug the binary with ARM64 support (breakpoints, disassembly, register inspection)  

---

## Process
1. **Static analysis in Ghidra**  
   - Opened the binary and identified `main` and `check_password`.  
   - Found the password check using the `strcmp` function:contentReference[oaicite:0]{index=0}.  

2. **Running under QEMU**  
   - Made the binary executable:  
     ```bash
     chmod +x ./l2_aarch64
     ```  
   - Launched the binary under QEMU with the GDB stub enabled:  
     ```bash
     qemu-aarch64 -L /usr/aarch64-linux-gnu/ -g 1234 ./l2_aarch64 fake_flag_gianna
     ```

3. **Attaching with GDB Multiarch**  
   - In another terminal, attached with `gdb-multiarch`:  
     ```bash
     gdb-multiarch -q \
       --nh \
       -ex 'set architecture aarch64' \
       -ex 'target remote localhost:1234'
     ```  
   - Set a breakpoint at `main`, continued execution, then disassembled to find `check_password`.  
   - Set another breakpoint at `check_password` and disassembled it to locate the `strcmp` call.  

4. **Register inspection**  
   - Set a breakpoint at the `strcmp` instruction.  
   - Continued execution until it hit the breakpoint.  
   - Examined registers `x0` and `x1` (where AArch64 passes function arguments).  
   - Saw both my fake input and the **real flag** stored there during the comparison.  

---

## Result
- Successfully recovered the hidden flag by examining the values in registers `x0` and `x1` at the `strcmp` breakpoint.  
- Confirmed the flag matched the program’s success condition.  

---

## Takeaways
- Learned how to set up cross-architecture debugging in Kali using QEMU and `gdb-multiarch`.  
- Reinforced AArch64 calling conventions (function arguments in registers instead of the stack).  
- Gained practice running and analyzing ARM64 binaries even on an x86 system.  

> Full detailed write-up with notes: `Lab6_aarch64.docx`
