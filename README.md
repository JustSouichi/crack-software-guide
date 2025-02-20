# Crack Software Guide

## ⚠ Disclaimer
**This guide is strictly for educational purposes only!** The software used in this guide (CrackMe) is intentionally designed to be cracked for practice. Cracking or modifying other software without permission is illegal and may result in legal consequences. This guide aims to teach **reverse engineering concepts** for educational and ethical research purposes only.

---

## 📥 Downloading CrackMe
To begin, visit [CrackMe](https://crackmes.one) and download **crackme_1.exe** from the resources section.

Higher numbers indicate higher difficulty, so it's recommended to start with **crackme_1.exe**.

---

## 🔎 Understanding Software Execution
Before diving into cracking, it’s important to understand how software works:

1. **Source Code → Compiler → Binary Code**
   - Developers write programs in high-level languages like C or C++.
   - The **compiler** translates this into **binary code (0s and 1s)** that the computer understands.

2. **Login Mechanism in Software**
   - When logging into a program, the software compares the **input key** with the correct **registration key** stored inside it.
   - If they match, access is granted; otherwise, access is denied.

3. **Reverse Engineering for Cracking**
   - To bypass these checks, we analyze and modify the binary using a **disassembler**.
   - This allows us to convert the binary code back into **assembly code**, which is human-readable.

---

## 🛠️ Tools Required
- **x64dbg** (Free Open-Source Disassembler)
  - Download: [x64dbg GitHub Releases](https://github.com/x64dbg/x64dbg/releases)
  - Install and open it.

---

## 🏗️ Step-by-Step Guide to CrackMe_1.exe

### 1️⃣ Setting Up the Disassembler
1. Open **x64dbg**.
2. Select **x96** architecture.
3. Load **crackme_1.exe** by dragging and dropping it into the debugger.
4. The software will decompile the binary into **assembly code**.

### 2️⃣ Understanding the Program
- The **assembly instructions** control how the program executes.
- Registers on the right store data, and the bottom section shows program memory.

### 3️⃣ Finding the String for Incorrect Input
1. Run **crackme_1.exe**.
2. Enter any input in both fields and press **Check it!**.
3. A popup appears saying: _"Nope, that's not it! Try again"._
4. Right-click in **x64dbg** → **Search for** → **All Modules** → **String References**.
5. Use **CTRL + F** to search for the phrase: `Nope`.
6. Right-click it → **Follow in Disassembler**.

### 4️⃣ Locating the Registration Check
1. Scroll **above** the `Nope` string.
2. You'll see two `push` instructions:
   - `push crackme.406584`
   - `push crackme.406B84`
3. These store **memory locations** of the correct key and the user input.
4. The program compares these two values:
   ```cpp
   if (correct_mem == user_mem) {
       print("You are logged in!");
   } else {
       print("Nope, that's not it!");
   }
   ```
5. We need to **force** the program to always succeed.

### 5️⃣ Modifying the Jump Instruction
1. Locate the following line in assembly:
   ```assembly
   jne crackme.4012A8  ; Conditional jump (if not equal, go to error message)
   ```
2. Change it to:
   ```assembly
   jmp 0x0040128E  ; Unconditional jump (always succeed)
   ```
   - Right-click on the instruction → **Assemble** → Modify it.

### 6️⃣ Patching the Program
1. Save the modified program:
   - Go to **File → Patch File → Select All → Patch File**.
   - Save it with a different name (e.g., `crackme_1_patched.exe`).
2. Open the patched `.exe` and enter any input.
3. It should now display: **"Yep, that's the right code!"**.

---

## 🎯 Creating a Keygen
A **Keygen (Key Generator)** is a program that generates a valid registration key.

### 1️⃣ Understanding How the Key is Stored
1. Open the **original** (unmodified) `crackme_1.exe` in x64dbg.
2. Locate the comparison instruction (`JMP.&lstrcmpA`).
3. The correct key is stored at one of these locations:
   - `crackme.406584`
   - `crackme.406B84`
4. Modify the memory push instruction:
   - Replace `push 0x406584` with `push 0x406B84`.
5. Save and test the program.
6. If successful, it will display a valid key.

### 2️⃣ Generating a Registration Key
1. Reopen `crackme_1.exe`.
2. Enter any username, but use the discovered key (e.g., `JRIU-BPUQ-XHLN-PUEI`).
3. It should grant access!
4. This key changes each time, so your **Keygen must reproduce the algorithm**.

---

## 📌 Conclusion
Congratulations! You have successfully:
- **Disassembled a program**.
- **Found its key-checking mechanism**.
- **Modified it to always return success**.
- **Extracted the correct registration key**.
- **Generated a valid key for any username**.

This process introduces you to **reverse engineering** and **software security analysis**. However, always remember:
> **Using these skills for illegal purposes is strictly prohibited. This guide is only for ethical learning!**

Happy learning! 🚀

