# Lab 7 – Reverse Engineering Challenge (AArch64)

## Goal
Reverse engineer a Linux ELF 64-bit AArch64 binary (`l4_aarch64`) and recover the hidden flag by analyzing the password check and inspecting registers at the string comparison.

---

## Tools Used
- **Ghidra** – to locate the success message, identify the `check_password` function, and find the `strcmp` call  
- **QEMU (qemu-aarch64)** – to emulate an ARM64 environment and run the binary on x86 hardware  
- **GDB Multiarch (`gdb-multiarch`)** – to debug the binary, set breakpoints, and inspect registers  

---

## Process
1. **Static analysis in Ghidra**  
   - Opened the binary and searched for the string `"you have succeeded"`.  
   - Followed cross-references to the suspected `check_password` function (`FUN_00400ad`).  
   - Traced it to calls that looked like `encrypt_decrypt` followed by a `strcmp`:contentReference[oaicite:0]{index=0}.  

2. **Running and debugging**  
   - Ran the binary under QEMU (`qemu-aarch64`) in one terminal.  
   - In another terminal, attached with `gdb-multiarch` configured for AArch64.  

3. **Breakpoint at `strcmp`**  
   - Set a breakpoint at the string compare.  
   - Continued execution until the breakpoint hit.  

4. **Register inspection**  
   - Checked registers at the breakpoint.  
   - Found the flag stored in `x0` (first argument to `strcmp`).  

5. **Validation**  
   - Copied the flag and ran the binary again using it.  
   - Confirmed correctness with the success message.  

---

## Result
- Successfully recovered the hidden flag by examining the value in register `x0` at the `strcmp` breakpoint.  
- Verified the flag by running the program and receiving the success output.  

---

## Takeaways
- Used **string search in Ghidra** to quickly find success conditions.  
- Identified and confirmed the password check routine (`check_password` + `strcmp`).  
- Practiced using **QEMU + GDB multiarch** to debug ARM64 binaries and inspect registers.  
- Reinforced understanding of AArch64 calling conventions (function arguments passed in registers).  

> Full detailed write-up with notes: `Lab7_aarch64.docx`

