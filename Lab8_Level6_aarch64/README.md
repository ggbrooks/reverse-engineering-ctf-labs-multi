# Lab 8 – l6_aarch_stat (AArch64)

## Goal
Reverse engineer the `l6_aarch_stat` binary and recover the hidden flag by tracing the password-check path and inspecting the stack at runtime.

---

## Tools Used
- **Ghidra** — string search, cross-references, function identification, and stack local analysis
- **QEMU (`qemu-aarch64`)** — to run the ARM64 binary on my x86 Kali VM with remote debugging enabled
- **GDB multiarch (`gdb-multiarch`)** — breakpoints, manual `call`, and stack/register inspection with AArch64 support

---

## Process
1. **Locate success path and key functions in Ghidra**  
   - Searched for the string `"you achieved Level 6"` and jumped to the referencing code path  
   - Identified the `check_password` function as `FUN_00400ae8`  
   - Found `memcmp` (`FUN_0040f780`) at `0x00400ccc` and the `encrypt_decrypt` routine just above it   
   - Relabeled helper routines (`signal`, `raise`, `CheckForDebugger`, `IsThereADebugger`) for clarity

2. **Prep for a targeted manual call**  
   - Copied the relevant call/argument info into notes, planning to swap the fake flag argument for the stack buffer so the routine operates on the real flag  
   - Used Ghidra (`Ctrl+Shift+G`) to find `local_38` and the exact address needed for the call

3. **Run under debugger and break after setup**  
   - Launched with a fake argument and attached the debugger  
   - Set a breakpoint after the flag was written to the stack at `0x400c68`, continued to it, executed the prepared call, and examined the string at `SP + 0x58` to read the flag

---

## Result
- Successfully revealed the flag from the stack and confirmed it with the program’s congratulatory message

---

## Takeaways
- Searching success strings and following their references quickly led to the validation logic 
- Capturing exact call sites and stack-local offsets made the manual `call` straightforward 
- A precise breakpoint (at `0x400c68`) allowed reading the flag directly from memory (`SP + 0x58`) without patching the binary

> Full write-up: `Lab8_aarch_stat.docx`
