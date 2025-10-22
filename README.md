---
## 🧾 Daily Log — October 22, 2025
---
Rebuild the ALU from scratch after design issues in the previous version.
---
Progress Summary:
Designed and implemented a 1-bit full adder using two half adders and an OR gate.
Verified the adder against the full truth table all outputs matched perfectly.
Built a 2→4 decoder using F1 and F0 inputs with NOT and AND gates to generate operation select lines.
Connected the decoder outputs to enable logic and arithmetic units.  
Implemented the logic unit supporting:

00 → X AND Y  
01 → X OR Y  
10 → NOT X  
11 → ADD X and Y (+ Cin)  

Constructed and tested the full ALU with all 16 test cases (logic + arithmetic).

All tests passed 1-to-1 correct Result and Cout for every operation.

Outcome:
The ALU is fully functional and validated. Decoder, logic unit, and full adder all operate correctly together.  
   | Test # | F1F0 (Opcode) |  X  |  Y  | Cin | Expected Result | Cout | Status |
| :----: | :-----------: | :-: | :-: | :-: | :-------------- | :--: | :----: |
|    1   |       00      |  0  |  0  |  0  | 0               |   0  |    ✅   |
|    2   |       00      |  1  |  1  |  0  | 1               |   0  |    ✅   |
|    3   |       01      |  0  |  1  |  0  | 1               |   0  |    ✅   |
|    4   |       10      |  0  |  –  |  0  | 1               |   0  |    ✅   |
|    5   |       10      |  1  |  –  |  0  | 0               |   0  |    ✅   |
|    6   |       11      |  0  |  0  |  0  | 0               |   0  |    ✅   |
|    7   |       11      |  0  |  1  |  1  | 0               |   1  |    ✅   |
|    8   |       11      |  1  |  1  |  1  | 1               |   1  |    ✅   |
---



## 🧾 Daily Log — October 21 2025

---
8-bit CPU Mainboard / ALU Module Testing
---
🔧 Work Done Today
Verified the 2-to-4 decoder design and corrected wiring.
Each combination of F1 F0 ∈ {00, 01, 10, 11} activates exactly one output (D0–D3).
Confirmed one-hot behavior with LED probes.
Connected the decoder outputs to the logic unit and adder blocks.
Performed functional testing on the 1-bit ALU slice before chaining 8 bits.
Implemented output selection network:

(D0 · AND_out) + (D1 · OR_out) + (D2 · NOT_out) + (D3 · ADD_out)

Replaced incorrect XOR combiner with OR gate.
Traced and corrected multiple wiring mix-ups between decoder lines and logic functions.
Swapped D0/D1 enables to align 00 → AND, 01 → OR.
Discovered carry-out leakage from adder in logic modes → gated Cout with D3.
Refined the NOT path to use pure ¬B signal, independent of A or Cin.
Built and verified 8-bit version: A[7:0], B[7:0] buses, carry chain, shared F1 F0 control.

---

|  F1 |  F0 | Cin |  A  |  B  | Expected Output | Expected Cout | Notes               |
| :-: | :-: | :-: | :-: | :-: | :-------------: | :-----------: | :------------------ |
|  0  |  0  |  X  |  0  |  0  |        0        |       0       | AND mode            |
|  0  |  0  |  X  |  1  |  0  |        0        |       0       | AND                 |
|  0  |  0  |  X  |  0  |  1  |        0        |       0       | AND                 |
|  0  |  0  |  X  |  1  |  1  |        1        |       0       | AND                 |
|  0  |  1  |  X  |  0  |  0  |        0        |       0       | OR mode             |
|  0  |  1  |  X  |  1  |  0  |        1        |       0       | OR                  |
|  0  |  1  |  X  |  0  |  1  |        1        |       0       | OR                  |
|  0  |  1  |  X  |  1  |  1  |        1        |       0       | OR                  |
|  1  |  0  |  X  |  0  |  0  |        1        |       0       | NOT mode (¬B)       |
|  1  |  0  |  X  |  0  |  1  |        0        |       0       | NOT                 |
|  1  |  0  |  X  |  1  |  0  |        1        |       0       | NOT                 |
|  1  |  0  |  X  |  1  |  1  |        0        |       0       | NOT                 |
|  1  |  1  |  0  |  0  |  0  |        0        |       0       | ADD mode            |
|  1  |  1  |  0  |  0  |  1  |        1        |       0       | ADD                 |
|  1  |  1  |  0  |  1  |  0  |        1        |       0       | ADD                 |
|  1  |  1  |  0  |  1  |  1  |        0        |       1       | ADD                 |
|  1  |  1  |  1  |  0  |  0  |        1        |       0       | ADD+Cin=1 (0+0+1=1) |
|  1  |  1  |  1  |  0  |  1  |        0        |       1       | ADD+Cin=1           |
|  1  |  1  |  1  |  1  |  0  |        0        |       1       | ADD+Cin=1           |
|  1  |  1  |  1  |  1  |  1  |        1        |       1       | ADD+Cin=1           |

Errors at line;
4th output 1 cout 1.  
8th output 1 cout 1.  
9th output 0 cout 0.  
11th output 0 cout 0.  
12th output 0 cout 1.
