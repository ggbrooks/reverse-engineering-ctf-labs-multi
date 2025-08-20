# Lab 9 – l9_aarch64 (AArch64)

## Goal
Bypass the OTP (One-Time Password) check in the `l9_aarch64` binary so the program reveals the flag regardless of the input.

---

## Tools Used
- **Ghidra** — string search to locate the congratulatory message and label key variables (`OTP`, `user_input`, `decoy`) 
- **Cutter** — to inspect the OTP comparison and patch the binary

---

## Process
1. **Locate main and validation path**  
   - Loaded the binary in Ghidra and searched for the success message  
   - From there, identified the OTP, `user_input`, and a decoy variable

2. **Patch the comparison**  
   - Opened the binary in Cutter and navigated to the `if` statement where the OTP is compared to the user input  
   - At instruction `0x00400900`, changed the byte from `0x48` to `0x44`, forcing the check to always succeed

3. **Run test**  
   - Executed the patched binary. Entered `"password"` (though any input works).  
   - The program printed the flag successfully

---

## Result
- Patched the OTP check so that any input bypasses validation.  
- Running the binary with any string results in the flag being revealed

---

## Takeaways
- String searching for success messages is a reliable way to locate main validation logic in stripped binaries.  
- A single-byte patch at the right conditional check (`0x00400900`) can completely bypass input validation.  
- Cutter is especially useful for identifying and modifying conditional jumps directly at the assembly level.  

> Full write-up: `Lab9_aarch64.docx`
