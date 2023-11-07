# Block Ciphers
> Transform the input text directly, instead of generating a stream which needs to be combined

Examples: AES, DES
***
- Substitution ciphers on blocks
**P** - Plaintext block (n bits)
**C** - Ciphertext block (n bits)
**K** - Key (k bits)
$C=E(P, K)$ - Encryption function 
$P = D(C, K)$ - Decryption
***
**We want:**
- Infeasible to get plaintext without key (obvs)
- Knowledge of how it works doesn't matter
- each bit in C depends on all bits of P
- each bit in C depends on all bits of K
- e.g one bit change in P or K creates big change in C
***
## Confusion and Diffusion
**Confusion**
Substitution to make relationship as hard as possible
**Diffusion**
Transformations that dissipate statistical properties of the plaintext

# Iterator Ciphers
- Divide encryption into *rounds*
- Subfunctions are the same, *round function* $g$
- Rounds *may* use a different key $K_i$ derived from the input key $K$
- *round keys* internally derived through *Key Schedule*
$$
\begin{align}
W_{0}&= P \\
W_{1}&= g(W_{0}, K_{1}) \\
W_{2}&= g(W_{1}, K_{2}) \\
W_{r}&= g(W_{r-1}, K_{r}) \\
C &= W_{r}\\
\end{align}
$$
$g$ must have an inverse $g^{-1}$ so that decryption can happen.
## Substitution-Permutation
	e.g AES
**Substitution then transposition**
over and over again
## Feistel Network
	e.g DES

Algorithm:
```
Input: 2L-bit block W_in
Output: 2L-bit block W_out
Key: the function f(.)
Split W_in into W_1, W_2
W_1' = W_1
W_2' = W_2 XOR f(W_1)
W_out = W_2' || W_1'
go again
```

## DES
Feistel Network
**Brute Force**
> Test all possible $2^n$  keys

### Multiple DES
Encrypt, then encrypt that again
$$
C = E(E(P, K_{1}), K_{2})
$$
**Is it more secure?**: Unlike caesar, there is no $K_3$ that is equivalent to both $K_1$ and $K_2$. i.e you can't crack a single key to unlock both at once.  So yes
**How Much More Secure?**

**Meet In the Middle Attack**
*Known-plaintext* attack
Assuming we have a single P/C pair
1. For each key, store $C'=E(P, K)$
2. Check whether $D(C, K') = C'$ for any key $K'$
3. $K$ from step 1 gives candidate $K_1$
	$K$ from step 2 gives candidate $K_2$
4. Check if the key values work for any other pairs
In laymans terms: Encrypt P with all possible K ($2^{56}$) and store. Decrypt C with all possible K ($2^{56}$). Look up where the two match, you've found the middle with only $2^{57}$ calculations instead of $2^{112}$ 
Don't actually need to test two keys at once, just test one side ($P -> C'_1$) and the other ($C'_2$ -> C). If you get $C'_1 = C'_2$, you've cracked it.

**3-DES**
- much better security
- No better than using 2 keys? 
*MITM* -> still applies, but much more difficult. Have 2 possible places to 'meet'
At least has to do 2 keys at once, no way around that. $2^{112}$ strength, hence as strong as 2 keys. Don't even need to have $K_{1}\ne K_{3}$ , essentially same security when the same. 
## AES
**State**: 4x4 matrix of bytes (32x32 bits )
1. ByteSub (non-linear substitution)
2. ShiftRow (permutation)
3. MixColumn (permutation)
4. AddRoundKey 

**AES compared to DES**
- Longer key length 
- Longer data-block length (128 vs 64)
- Speed (faster in hardware and software)

# Modes of Operation
> A block cipher is a deterministic keyed permutation. Modes of operation introduce randomness

Do this through an **Initialisation Vector**

Modes can be for:
- Parallel processing
- Error (non)-propagation: a bit error in the ciphertext makes a lot of plaintext errors (not always bad)
- (Avoiding) Padding - some modes need full blocks, others avoid this
### Comparisons

|-|ECB|CBC|CFB|OFB|CTR|
|-|-|-|-|-|-|
|Randomised|$\times$| $\checkmark$|$\checkmark$|$\checkmark$|$\checkmark$|
|Padding|No| Required|No|No|No|
|Error Propagation| Within blocks | Across Blocks (specific bits)| Into this and Next Block|Current Block|Current Block|
| IV | None | Random| Random| Unique| Unique |
| Parallel Encryption | $\checkmark$| $\times$ |$\times$| $\times$| $\checkmark$|
| Parallel Decryption | $\checkmark$| $\checkmark$ |$\checkmark$|$\times$| $\checkmark$|
| Notes | Deterministic, not used for more than a block.| Commonly used for bulk encryption| Self-synchronised. Very similar to CBC| Dumb, just turns block into stream| Brittle but secure with unique nonce|

***
## Electronic Codebook (ECB)
*Encryption*: 
- $C_t=E(P_{t}, K)$
- Plaintext block is encrypted with a key to produce ciphertext block
*Decryption*:
- $P_{t}=D(C_{t}, K)$
- Ciphertext block is decrypted to produce Plaintext 

## Cipher Block Chaining (CBC)
*Encryption*
- $C_{t}= E(P_{t}\oplus C_{t-1}, K)$
- XOR $P_t$ with the previous ciphertext (or IV for t=1), and then encrypt
*Decryption*
- $P_{t}= D(C_{t}, K)\oplus C_{t-1}$
- Decrypt, then XOR with previous (or IV)

## Cipher Feedback (CFB)
> Feed last ciphertext back into the next, chaining the blocks together

*Encryption*
- $C_{t}=E(C_{t-1}, K)\oplus P_{t}$
*Decryption*
- $P_{t}=D(C_{t-1}, K)\oplus C_{t}$
Errors propagate crazy into next block, then fixed.
Don't actually encrypt the plaintext, encrypt the IV (or prev C), and xor with plaintext. Basically a keystream
> Effectively a *self-synchronising* stream cipher
> > After processing a correct ciphertext block, know everything you need to know. e.g if a block is missed, the next one is garbled, but then synced again for next block

## Output Feedback (OFB)
> In effect, a *synchronous* stream cipher. The keystream is:

$$
   O_{t}= E(O_{t-1}, K)
$$
With  $O_{0}$ is a *nonce*, or the $IV$
*Encryption*
- $C_{t}= O_{t}\oplus P_{t}$
*Decryption*
- $P_{t}= O_{t}\oplus C_t$

## Counter (CTR)
> Synchronous stream cipher, with keystream generated as 

$$
O_{t}= E(T_{t}, K)
$$
with $T_{t}=N||t$ , nonce concatenated with counter
*Encryption*
- $C_{t}=O_{t}\oplus P_t$
*Decryption*
- $P_{t}= O_{t}\oplus C_{t}$
Errors in ciphertext propagate within that block (at that location)



