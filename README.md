# Cracking Software Learning Guide

## ⚠️ Disclaimer

**This guide is intended for educational purposes only.**
Cracking software without permission is illegal and unethical. The purpose of this guide is to help individuals learn about reverse engineering techniques in a controlled and legal environment. Always ensure you have explicit permission before testing any software.

---

## 📌 Introduction

This guide will help you understand the basics of **reverse engineering** and software **cracking** using simple step-by-step exercises. We will use special test programs called `crackme_x.exe` (where `x` is a number) to practice different cracking techniques. The difficulty increases with the number.

For more examples and inspiration, visit: [https://crackmes.one](https://crackmes.one)

---

## 🔹 Tools Required

- **x64dbg Disassembler**: [Download Here](https://github.com/x64dbg/x64dbg/releases)
- Windows system to run `crackme_x.exe`
- **Hex Editor (e.g., HxD)** to analyze binary data
- **OllyDbg** as an alternative disassembler
- **PEiD** to determine file packing/protection

---

## 📖 Step-by-Step Guide

### 1️⃣ Cracking `crackme_1.exe`

#### 🔹 Step 1: Running the Program
1. Download `crackme_1.exe` from the resources.
2. Open and run the file.
3. Enter random values in the input fields and click **Check it!**.
4. You will get a message: **"Nope, that's not it! Try again."**

#### 🔹 Step 2: Understanding Software Authentication
- Software usually checks if the entered key matches a stored valid key.
- The verification process happens in **binary code** compiled from the source code.
- Using **reverse engineering**, we can convert this binary back into a readable **assembly code**.
- Authentication typically compares user input against a stored value using **strcmp** or similar functions.

#### 🔹 Step 3: Using x64dbg to Analyze the Software
1. Open **x64dbg** and load `crackme_1.exe`.
2. Select **x32** for 32-bit executables.
3. Drag and drop `crackme_1.exe` into x64dbg to decompile it.
4. Set breakpoints on key functions such as `strcmp`.

#### 🔹 Step 4: Finding the Failure Message
1. Right-click -> **Search for -> All Modules -> String References**.
2. Search for `"Nope"`.
3. Right-click on the found string -> **Follow in Disassembler**.

#### 🔹 Step 5: Identifying Key Comparisons
- Scroll up to locate **two `push` instructions** with memory locations, e.g.: 
  ```
  push crackme.406584
  push crackme.406B84
  ```
- The software compares these values to determine if access is granted.

#### 🔹 Step 6: Modifying the Jump Instruction
- Find the instruction:
  ```
  jne crackme.4012A8
  ```
- Change it to:
  ```
  jmp 0x0040128E
  ```
- Right-click on the instruction -> **Assemble** -> Edit and confirm.

#### 🔹 Step 7: Patching the Software
1. Save the modified file:
   - Go to **File -> Patch File -> Select All -> Patch File**.
   - Save it as a new `.exe` file.
2. Run the modified file and enter any random input.
3. If successful, it will display: **"Yep, that's the right code!"** 🎉

---

## 🔑 Creating a Keygen
A **keygen** generates valid registration codes for any username. 

### 1️⃣ Identifying the Correct Key Location
- Reopen the original `crackme_1.exe`.
- Locate the function comparing the correct key (`JMP.&lstrcmpA`).
- Find which memory location stores the correct key (`crackme.406B84` or `crackme.406584`).

### 2️⃣ Replacing the "Nope" Message with the Correct Key
1. Modify the push instruction:
   ```
   push 0x406B84  (or push 0x406584 if the first doesn’t work)
   ```
2. Save and run the modified program.
3. If the program returns the same value you entered, try the other memory location.
4. Once found, use that as the valid key.

### 🎯 Example Key Found:
```
JRIU-BPUQ-XHLN-PUEI  (This is an example, your key will be different)
```

### 3️⃣ Verifying the Key
- Reopen the **original (unmodified)** `crackme_1.exe`.
- Enter any username and the generated key.
- If successful, you have successfully **cracked the software**! 🎉

---

## 🔍 Advanced Techniques
- **Debugging with OllyDbg**: Alternative debugger for deeper analysis.
- **Checking File Protection with PEiD**: Identify packing or encryption.
- **Using a Hex Editor**: Modify key strings directly in the binary.
- **Brute-force or Fuzzing Techniques**: Generate valid keys using patterns.

---

## 🏆 Summary
- **Reverse Engineering** allows us to analyze how software verifies keys.
- **x64dbg** helps inspect software’s **assembly code**.
- **Patching** modifies how software behaves.
- **Keygens** generate valid keys for given inputs.
- **Advanced debugging** techniques help bypass security mechanisms.

---

## 🛑 Final Note
This guide is meant for educational use **only**. Always respect software licenses and use this knowledge responsibly. 💡