## Motivation
- Simple substitution is vulnerable to statistical attack
- Even the large key space (26!) does not appear to offer much protection.
- The **problem**: discrepancies in plaintext character frequencies are preserved (just moved around)
- **Solution?:** Use multiple mappings in a certain order

## What is it
- Smooth out the total frequency distribution
- Periodic substitution based on a period **d**
- given **d** ciphertext alphabets $C_{0,}C_{1,},...,C_{d-1}$:
$$
f_{i}= A \rightarrow C_i
$$
## Process
Plaintext message
$$
M = m_{o}...m_{d-1}m_d...m_{2d-1}
$$
Is encrypted to
$$
E_{k(M)}= f_0(m_{0)}etc.
$$
Key Generation
1. Select the block length **d**
2. Generate $d$ random substitution tables
Encryption
1. Encrypt the $i$th character with the substitution table $i\equiv j \text{ mod } d$  

