## [[Lecture 01]]
**Cryptography**
Hidden writing. Turning messages into unintelligible
Protected writing
**Cryptanalysis**
Studying weakness in cryptography
**Plaintext (P)** - Information to be protected
**Ciphertext (C)** - Garbled message
**Encryption (E)** - The process to take P to C
**Decryption (D)** - The process to recover P from C
**Cryptographic Key (K)** - The special input of algorithms to unlock their operation
**Modular Inverse**
The modular inverse of $a$ is $a^{-1}$, and exists if $a\cdot a^{-1}\equiv 1 (\mod n)$ 
Only numbers that are coprime to $n$ have modular inverses, e.g $g.c.d (a, n) = 1$
## [[Lecture 02]]
**Polyalphabetic Substitution**
Use different mappings depending on location within the plaintext. Mix up frequency distributions
**Polygraphic Substitution**
Combine letters into blocks and alter the whole shebang
## [[Lecture 03]]
**Information** Is the degree of 'newness' that is obtained from an event. Measured in bits.
**Entropy** is how much information we expect, or the average information level from a probabilistic event
**Rate of Language** is average information per character. English only 1.5 bit/letter
**Redundancy** is the difference between Rate of Language and max entropy. 
**Unicity Distance** is the minimum amount of ciphertext before you can uniquely determine a key. Unrelated to feasibility, just means that there will only be one key that decrypts to english(ish) plaintext
**Perfect Secrecy** is when the likelihood of seeing a message turn into a ciphertext is the same likelihood as just seeing that ciphertext by random.
## [[Lecture 04]]
**Stream Cipher** encrypts/decrypts as the key is generated, a network of XOR streams and the message.
**KeyStream Generator** generates a stream, with a state, update, and output function. Setup with an IV and key. Hard to maintain synchronisation.
## [[Lecture 05]]
**Block Ciphers** transform the message directly in blocks.
**Confusion** is substitution that makes relationships hard to spot.
**Diffusion** is transposition that dissipates statistical properties
**AES** is a substitution-permutation encryption
**DES** is a fiestel network
Electronic-CodeBook, Cipher Block Chaining, Cipher Feedback, Output Feedback, Counter

|-|ECB|CBC|CFB|OFB|CTR|
|-|-|-|-|-|-|
|Randomised|$\times$| $\checkmark$|$\checkmark$|$\checkmark$|$\checkmark$|
|Padding|No| Required|No|No|No|
|Error Propagation| Within blocks | Across Blocks (specific bits)| Into this and Next Block|Current Block|Current Block|
| IV | None | Random| Random| Unique| Unique |
| Parallel Encryption | $\checkmark$| $\times$ |$\times$| $\times$| $\checkmark$|
| Parallel Decryption | $\checkmark$| $\checkmark$ |$\checkmark$|$\times$| $\checkmark$|
| Notes | Deterministic, not used for more than a block.| Commonly used for bulk encryption| Self-synchronised. Very similar to CBC| Dumb, just turns block into stream| Brittle but secure with unique nonce|
## [[Lecture 06]]
**Hash Functions** should be *collision resistant*, *second-preimage resistant*, *one-way*
**Collision resistance** implies **Second-Preimage** resistance
**Sponge Hash Construction** is a non-linear and can create large hashes. Secure
**MAC** is like an authenticated hash, using a secret key.
**E&M**: Encrypt and mac separately, **MtE**, mac, then encrypt all. **EtM**, safest.
## [[Lecture 07]]
**Euler's Totient**: Number of coprimes less than $n$
**Euler's Theorem**: Finding inverses $\bmod n$ 
**Fermat's Little Theorem**: If working in $\bmod p$ with prime $p$, $a^{p}\equiv p \bmod p$
**Test for Primality** With fermats, but **Carmichael Numbers** will always pass the Fermat test even though they are composite
**Miller-Rabin** test is better but not explained
**Hard Problem** is anything worse than exponential time
**Problem Complexity** is the minimum space and time to solve the hardest instance
**Discrete Logarithm** is the number of times to apply an operation to a generator to get a final value
**ElGamal** turns Diffie-Hellman into a PK encryption
## [[Lecture 08]]
**RSA** is a PK algorithm, where the input must be an integer less than the modulus
**Miller-Rabin** can be used to generate prime numbers (random bits (odd), test for prime, try again (or increment 2))
**Prime Number Theorem** says that $\pi(n):n/\ln{n}$  goes to 1. Primes become sparser
**Small Public Exponents** are much quicker, and with low hamming weight
**Private Exponents** should be much larger and $>\sqrt{n}$ 
**Chinese Remainder Theorem** can speed up decryption since $n=pq$ is known for the private key. $M_{p}=C^{d\bmod{p-1}} \mod p$, $M_{q}=C^{d\bmod{q-1}} \mod q$
**Hastad's Attack** builds a system of equations when one message is sent to lots of people with the same exponent
**Prevent Attacks** with randomised padding, or **OAEP**: an encoding scheme
**RSA** is secure as a factorisation problem, which we **think** is hard.
## [[Lecture 09]]
**Digital Signatures** are like MAC's for PK-crypto
**Correctness**: Verifying will always be correct, **Security**: infeasible to fake an s with only the public key
**Goals**: Key recovery, Universal Forgery (forge sig on any and all message), Existential Forgery (He gets to choose a message)
**Powers**: (public)Key-only, Known-message, chosen-message
**Gold**: Existential unforgeability under chosen-message attacks
**Redundency** is good for signatures, otherwise every sig would be meaningful
**Homomorphism** means given 2 sigs and 2 $M$, you can just multiply them and its valid. **Hash and Sign** much better
**ElGamal Sig** Randomised but still hash pls




