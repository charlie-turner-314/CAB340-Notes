# Public Key Cryptosystem
**For Confidentiality**
1. Key generation algorithm
2. Encryption Algorithm
3. Decryption Algorithm
# RSA
## Key Generation
1. Get $p$ and $q$, which are distinct (large) prime numbers 
2. Compute $n=pq$
3. Select e with gcd$(e, \phi(n))=1$
	1. Remember: Needs to be co-prime to *have an inverse* [[Definitions]]
4. Compute $d=e^{-1}\mod \phi(n)$
5. The public key is the pair ($n, e$)
6. The private key is the tuple ($p, q, d$)
## Encryption
The public key is $K_{E}=(n, e)$
The message is any value M where 0 < M < n
1. Compute $C=E(M, K_{E})=M^{e} \mod n$
## Decryption
The private key for decryption is $K_D=(n,d)$
1. Compute $M'=D(C, K_{D}) = C^{d}\mod n$
> The message must be preprocessed, or encoded as an integer in the range $[1, n-1]$

## But Why?
**We need to prove that**
$$
(M^{e})^{d}\mod n = M
$$
Since $d$ is by definition $e^{-1}\mod \phi(n)$, we know that $ed\mod\phi(n)=1$, and so $ed=1+k\phi(n)$ for some integer k ($k\cdot\phi(n)=0\mod\phi(n)$ every time).
$$
\begin{align}
(M^{e})^{d}\mod n &= M^{ed}\mod n \\
\therefore M &= M^{1+k\phi(n)}\mod n
\end{align}
$$
> Still need to prove that last bit.

**In the case where gcd(M, n) = 1**
> Vast majority of the time, remember $p$ and $q$ are large primes, and landing on a multiple of one is extremely unlikely

$$
\begin{align}
M^{\phi(n)}\mod n &= 1 \\
&\therefore \\
M^{1=k\phi(n)}\mod n &= M \times (M^{\phi(n)})^{k}\mod n \quad \text{- Expanding} \\
&= M \times (1)^{k}\mod n \\
&= M
\end{align}
$$
> M can only be a multiple of $p$ **or** $q$, since we make sure that $M<n$

Therefore:
$$
M = lq
$$
> p and q are interchangeable in theory, so we go with q

Bring out Fermat's tiny little theorem:
*Using modulo p:*
$$
\begin{align}
M^{1+k\phi(n)}\mod n &= M\cdot M^{k(p-1)(q-1)}\mod p \\
&= M\cdot (M^{p-1})^{(q-1)k}\mod p \\
&= M\cdot 1^{(q-1)k}=M
\end{align}
$$
*Using modulo q:*
$$
M^{1+k\phi(n)}\mod q = (lq)^{1+k\phi(n)}\mod q = 0
$$
*Bring together:*
$$
\begin{align}
M^{1+k\phi(n)}\mod p &= M \\
M^{1+k\phi(n)}\mod q &= 0
\end{align}
$$
Now we can bring out the CRT, there is one unique that satisfies the above. It's M.


## Random Key Generation
Use the [[Lecture 7#Miller-Rabin Test]] algorithm to generate the random $p$ and $q$
### Prime Number Theorem
If $\pi(n)$ is the quantity of primes less than $n$, 
The ratio of $\pi(n)$ and $x/\ln(x)$ tends to 1 (primes thin out)
Theres still **a lot** of primes that are 1024 bits long. Brute force is not an option.
## Practical Problems
- Randomness is important! If people share keys, or even share prime factors of a key, we have a big issue. 

## Selecting $e$ for super-fast encryption

Take $e$ to be small and with low **Hamming weight** (not many 1 bits)
This way you can [[Lecture 7#Square and Multiply]] very quickly. $e$ doesn't need to be secret, so is just usually 65537 apparently.
> 65537 is a *Fermat Prime* - **of the form $2^{2n} + 1$** which is interesting, as it is prime and thus very unlikely to be a factor of $\phi(n)$ ($(p-1)(q-1)$), and is represented in binary as 10000000000000001, so can be used as an exponent fast!

*$d$, on the other hand, should be greater than $\sqrt{n}$, to avoid known attacks. Fixing $e$ to this small(ish) number means that $d$ is guaranteed to be very large*

## Fast(er) Decryption with The CRT
[[Lecture 7#Chinese Remainder Theorem]]
- $d$ must be secret, so very large
- We know that $n=pq$ though (on the decryption side)
- Use CRT to solve for M, knowing these secrets:
1. Compute with Square and Multiply:
$$
\begin{align}
M_{p}&= C^{d\mod p-1} \mod p \\
M_{q}&= C^{d\mod q-1} \mod q 
\end{align}
$$
2. Solve for M:
$$
M = q\cdot(q^{-1}\mod p) \cdot M_{p}+p\cdot(p^{-1}\mod q) \cdot M_{q}\mod n
$$
**Example**:
$n = 43 \cdot 59$
Ciphertext is $C=2488$, decryption $d=1949$
$$
\begin{align}
d\mod p-1 &= 1949 \mod 42 = 17 \\
d\mod q-1 &= 1949\mod 58 = 35 
\end{align}
$$
Using C again:
$$
\begin{align}
M_{p}&\equiv 2488^{17} \mod 43 = 37^{17}\mod 43 = 7\\
M_{q}&\equiv 2488^{35}\mod 59 = 16^{35}\mod 59 = 50
\end{align}
$$
Using the formula for M:
$$
M=50
$$
> Using the CRT can achieve up to an 8x speedup (computing $M_p$ and $M_q$ in parallel. So even though you can use private key $(n, d)$, its quite a bit faster if you store $(p, q, d)$

# Attacking PKC
## Chosen Plaintext Attack
1. Build a dictionary of P/C pairs, by knowing or guessing P
2. Loop up C in the dictionary
## Hastad's Attack
- If the same exponent is used by multiple recipients, and the same message is encrypted to each, you can build a system of equations to solve for M.

# Preventing Attacks
## Padding
> Introduce randomised padding so the same message doesn't encrypt to the same ciphertext (technically it does but you're introducing randomness *in the message*)

### PKCS
Encrypt into blocks:

| 00|02|PS|00|D |
|-|-|-|-|-|
With 00 and 02 as leading byte paddings, PS as a pseudo-random string of bytes, 
and D as the data to be encrypted. Can be undone after decryption.
### OAEP

> Basically a 2-round Fiestel network *encoding* scheme. Can be undone by anyone but if you do it prior to encryption then that doesn't matter

- $k_0$ bits of redundancy and $k_1$ bits of randomness
- If we have a $k+1$ bit modulus then we use OAEP to encrypt a message of size $k-k_{0}-k_{1}$
- Use two random hash functions.
1. Choose a random value $r$ of length $k_0$
2. Set $X=(m||0^{k_{0}})\oplus G(r)$
3. $Y=r\oplus H(X)$
4. Encrypt $(X, Y)$
> $X$ is $k-k_1$ bits in length, $Y$ is $k_1$  bits in length

## Security
RSA is only as secure as a factorisation problem, which we **think** is hard. 
- Determining $e$ from $d$ **IS** a factorisation problem
- Determining $M$ from $C$ is **AT MOST** a factorisation problem










