
# Stream Ciphers
Encrypt/decrypt as you go
Random number generator (key) combined (xOR) with message, bit by bit or word by word

**Random** Is important
For binary:
1. half 1 and half 0
2. 1/2 runs have length 1, 1/4 length 2, 1/8 length 3. $\frac{1}{2n}$ length $n$
3. Autocorrellation constant 
$$
C(j) = \frac{A-D}{p}
$$
$j$ is the shift
$A/D$ is agreements, disagreements
$p$ is period


## Keystream Generator
- State: Storage of internal values
- Update Function: stored changes at each cycle
- Output Function: Function of the current state
- Setup Function: determine initial state from the initialisation vector (IV) and key (K)
### Design Constraints
Efficiency,
Implementation Requirements
Maintaining Sync

### Example
```
key -> keystream generator -> keystream
								|
				plaintext -> combiner -> ciphertext
```

Opposite for decryption

Alternatively, key and IV to keystream generator
Pros: reuse key with different IV 
Cons: need to transmit IV with ciphertext

# Linear Feedback Shift Registers
- Common component in stream cipher design
- LFSRs easy too implement in software/hardware
- Infinite periodic sequence can be generated through recurrance
$$

\oplus = \text{XOR}
$$
> Keystream period must exceed the ciphertext length. *Remember* [[Vigenere Cipher]] - can attack redundency

Linear complexity of a sequence is the minimum state size LFSR that can produce that sequence

### Real World Usage
- Generally used not as is, and multiple are combined or take multiple outputs from one and combine them
# A5 Cipher
> Binary synchronous stream cipher

Input: Three LFSRs
	Irregularly clocked, so overall output is non-linear
Only update an LFSR if it's clocking bit tells it to

BROKEN


# Pseudorandom Generators (PRG)
Input: A random seed
Output: bits that are unpredictable

