# Security Measures
## Confidentiality
Also known as privacy, keep some information secret from others

## Integrity
Ensure unaltered message. e.g use indelible ink, tamper evident. 
In cryptography -> message authentication codes, hashes

## Authenticity
Ensure coming from the correct source. 
**digital signatures** in cryptography, Authenticity implies integrity.

# Other side of security
## Availability
Need to actually be able to undo encryption for it to be valuable. Need **correctness** - should behave exactly as expected when used as intended. 

> **The main goal of cryptography is to remove the need for trust.**

## Unrelated fields
**encoding** - takes something to an alternate representation. Not cryptographic as does not use a key
**coding theory** - compression, error correction. Deep connections but NOT cryptography.


# Methods of Cryptanalysis
## Brute force
- just throw things at it until it breaks
## Mathematical
- algorithms may be imperfect
## Side-channel
- hardware characteristics
- observe timings, signals etc. On a slow computer you may be able to 'listen' to key generation to get an idea of what's going on
## Social Engineering
- Attack the host 
- Make your own weakness
- Trick the user into telling you all their secrets

# Security
## Kerckhoff's Principle
*The cryptanalyst has complete knowledge oif the cipher - the only unknown is the decryption key*
- A cipher can be complicated, impractical to change
- The key must be hard to guess
- The key must easy to share with a correspondent

## What is Security?
Security defined by a hypothetical adversery with...
**Goals**: What do they need to achieve to be successful
**Powers:** What access do they have? e.g known-plaintext
**Resources:** How much time, computation, storage do they have

### Goals
1. Crack the key
2. Recover message (but not the key)
3. Figure out part of the message
4. Tell if the ciphertext decrypts to a particular plaintext
5. Determine if the ciphertext is a ciphertext at all, and not just random string

These are in order of difficulty, if you can do 1 you can do all, etc.

### Powers
0. Cipher algorithm? **yes** - assumed known
1. Challenge ciphertext? **yes** - without it what are they attacking? **Ciphertext-only**
2. Additional examples of ciphertext? uncommon but possible
3. P/C pairs? **Known plaintext**
4. P/C pairs where the cryptanalyst can choose P? **chosen plaintext**
5. P/C pairs where the cryptanalyst can choose C? **chosen-ciphertext**

> **Gold Standard**: "Ciphertext indistinguishability in an adaptice chosen-ciphertext attack"

If the attacher cannot even distinguish between ciphertexts *they* chose, you have done very well. **Highest powers**, **lowest goals**

> **Modern Standard**: security against chosen plaintext and chosen ciphertext attacks

## Proving Security
Pretty much impossible, (other than one-time pad), but can show that a problem is as hard as some other problem


# Elementary Ciphers

All *symmetric-key* ciphers

## Basic Operations
### Transposition:
Move around the positions of the plaintext (anagramed)
### Substitution:
Each character or set of characters is replaced by others
### Alphabets
Need to have a defined alphabet. Can be A-Z with or without space. Could have punctuation, lower/upper case,. special characters.

### Statistical Analysis
With transposition and substitution, can sometimes work things out just by looking at distribution of ciphertext vs distribution of the alphabet.

# Transposition Ciphers
## Rail-fence or zig-zag
**Key**: Number K of rows
**Encryption**: Write out P on K rows, with a diagonal zig-zag
```
M = THESKYISBLUE
K = 3
T   K   B   Z
 H S Y S L E
  E   I   U
Read left to right without spaces:
TKBZHSYSLEEIU
Decrypt by reversing, write left-to right on 3 rows:
T   K   B   Z
 H S Y S L E
  E   I   U
Read diagonally:
THESKYISBLUEZ
```
**Requirements**:
- Need to know length of P to do one operation (can be reversed)
	- with simple message this is easy, not so easy if recieving in chunks?
- Keyspace is not super large (can't be above length of P)
- Not secure, can bruteforce relatively easily. 
- Strength: With a relatively large message, the ciphertext does appear random, and there is variable distance between consecutive letters. 
## Simple Transpositional Cipher
**Key:** $\pi$ a permutation over groups of $d$ characters.  e.g for $d=3$, $\pi$ may be 312 
**Encryption**: Divide P into blocks of $d$, rearrange as per $\pi$
**Decryption:** Divide C into blocks of $d$, reorder as per inverse of $\pi$
- The key is the pair $(d, \pi)$, however $\pi$ implies $d$
- There are $d!$ possible permutations, key space grows quickly with $d$
**Cryptanalysis**
Frequency analysis works here.
Can just split and anagram, with heuristic from alphabet distributions
Or, split and treat each block as a row in a matrix. Just anagram rows with heuristic as alphabet distributions. *Which column is most likely to be next to this column?*
1. Choose the column with the most common characters
2. Find the column most likely to be after or before this column using digram frequencies
3. Repeat for that column

# Substitution Ciphers
Key is how the replacement is done.
Replace each letter (or blocks of letters) with another (or another block)
**Caesar cipher, linear cipher, random substitution**
Represent Key as a function of each letter in the alphabet and its mapping
## Caeser Cipher
Key can simply be a number from 0-25, each substitution is just the Kth next letter with wraparound

**Key:** $K \in \{0, ..., 25\}$
**Encryption**: $C_{i} \leftarrow M_{i}+ K \mod 26$ 
**Decryption**: $M_{i} \leftarrow C_{i} - K \mod 26$ 
Find out where the most common letters are shifted to, break the rest. If X is the most common C letter, its probably shifted from space or E. 
## Linear Cipher
Generalised Caeser Cipher
***
**Key:** $K = (a, b)$ two numbers from 0-25
**Encryption**: $C_{i} \leftarrow (a * M_{i}) + b \mod 26$ 
**Decryption**: $M_{i} \leftarrow a^{-1} * (C_{i} - b) \mod 26$ 
***
**Known-plaintext** attack
Suppose we know that C starts with VQ
and P starts with IT, and the alphabet is A=0 to space=26 (e.g mod 27)
$$
\begin{align}
21 &\equiv 8a + b \mod 27 \\
16 &\equiv 19a + b \mod 27
\end{align}
$$
Use matrices or basic simultaneous euqations.
**Ciphertext only attack**
Work by finding location of most frequent characters. Try permutations of a and b as per most common in alphabet. 
E.g most common in C -> space, second most -> E
then most common -> space, third most -> E
## Random Substitution Cipher
Simple, but stronger.
***
**Key:** $K$ = permutation $\Pi$ over the alphabet
**Encryption**: $C_{i}\leftarrow \Pi(M_i)$
**Decryption**: $M_{i}\leftarrow \Pi^{-1}(C_i)$
***
Much larger keyspace.
Still exploitable through statistical analysis. 
Using most common letters in C, start to build approximation for $\Pi$, looking for common trigrams or words e.g THE

# Useful Frequency Stats
- Most common letters:
	- E, T, R, S, I, A, N
	- end of word: E 
	- start: T
- Single letters:
	- A, I
- Short words
	- OF, TO, IN
	- THE, AND
	- THAT
- Common doubles
	- LL, EE, SS, OO, TT, FF
Q is before U, if not then what is going on







