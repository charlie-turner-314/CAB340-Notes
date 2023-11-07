## [[Lecture 01]]
**RailFence/ZigZag**
K = number of rows. $C$ = write message on K rows, read diagonally
$P$ = write message on K rows, read L2R
**Simple Transposition**
$K=\pi=$ permutation over $len(\pi)$ characters e.g for length 3 could be 231
$E(M, K) = $ transpose blocks of $len(\pi)$ as per $\pi$
**Linear Cipher**
$K = (a, b)$
$C = (M_i\times a) + b \pmod{26}$
$P = a^{-1}\times (C_{i}-b)\pmod{26}$
**Random Substitution Cipher**
$K = \Pi =$ Permutation mapping over alphabet
$C =  \Pi(M_i)$
$P = \Pi^{-1}(C_i)$
**Lecture 1 Cryptanalyst**
Frequency analysis. Map the most common letters/digraphs to the ciphertext and go from there (informed guessing).  
## [[Lecture 02]]

**Vigenere** - Broken: Freq
$K =$ a word of length $d$ representing $d$ Caesar ciphers
$C = P_{i} + K_{i\bmod{d}}$ 
**Beaufort** - Broken: Freq
Same as Vigenere, except subtract message from key for both ops
**Autokey** - Broken: Autocorrellation
Same as Vigenere, but after $d$ letters, use the plaintext as the key.
**Playfair Cipher** - Broken: Freq Analysis Digraphs
Write the key as a 5x5 square of all the letters (i and J share a spot), encrypt digraphs as: Same row: change to letter on the right. Same col: Change to letter below. Otherwise: switch the columns but keep rows.
**Hill Cipher** - Broken: Linear system
Key is a $d\times d$ matrix. Group plaintext into $d$ length vectors. $C=K\cdot P \mod N$ (matrix-vector multiply). To decrypt, need the modular inverse matrix key.
## [[Lecture 03]]
Prob of X and Y: $Pr[X, Y] = Pr[X|Y] \times Pr[Y]$
Independently: $Pr[X,Y]=Pr[X]Pr[Y]$
Bayes: $Pr[X|Y] = \frac{Pr[X]}{Pr[Y]}Pr[Y|X]$
Information (bits) = $-log_{2}p \ge 0$
Entropy of Random Variable: $-\sum{p(x)\log_{2}p(x)}$ for each possible x
$H_{max} = log_{2}L$ for $L$ equally likely outcomes
Unicity Distance: $N\approx \frac{H(K)}{D}$ (entropy on redundancy)
## [[Lecture 04]]
Stream Generator (LFSR etc)
Autocorrelation periodic sequence: $C(j) = \frac{A-D}{p}$  for shift j, agg/dis A/D, period p
Random Sequence: Autocorrelation constant, 1/2 runs length 1, $\frac{1}{2n}$ length n
## [[Lecture 05]]
Iterator Cipher: $g =$ round function, do each $W_{r} = g(W_{r-1}, K_{r})$ with $W_{0}= P$
Fiestel Network: Take 2L-bit blocks, output 2L-bit blocks. Key is a function of L bits. $W_{1}' = W_{1}$, $W_{2}'=W_{2} \oplus f(W_{1})$ . Concat to get output, repeat.
Multiple DES: $C = E(E(P, K_{1}), K_{2})$
3-DES: Same security as if meet in the middle didn't work for 2 keys. 
AES: non-linear sub, permutation of rows, permutation of columns, add key, Repeat. Faster, Longer key length, longer data length than DES (128 vs 64)
Modes of Operation: **Include if Have Space**
## [[Lecture 06]]
Birthday Paradox: $2^{k/2}$ trials have around 0.5 prob of collision. $2^{128}$ trials still infeasible, so 256 bit hash considered collision resistant
MAC: $T = MAC(M, K) =$ fixed-length authenticated 'tag'
CBC-MAC: Block cipher: $C_{t}=E(M_{t}\oplus C_{t-1}, K)$ for $l$ blocks of message 
$\text{HMAC-H}(M, K) = H((K\oplus \text{opad}) || H((K\oplus \text{ipad})||m))$ , with opad and ipad fixed known strings.
## [[Lecture 07]]
Square and Multiply: $y=b^{x}\mod n$ 
let y=1, for bit in $bin(x)$ (MSB to LSB):  (square y, if bit == 1, multiply by b) modulo n
Chinese Remainder Theorem if $n=pq$ (two primes):
$$
\begin{align}
m &\equiv a \mod p\\
m &\equiv b \mod q\\
\end{align}
$$
$$
\begin{align}
M=((q\cdot (q^{-1}\mod p)\cdot a + p\cdot(p^{-1}\mod q)\cdot b)) \mod n
\end{align}
$$
$\phi(n)=$ write the number as product of prime factors, subtract 1 from each group: ie $n=p^{2}q$, $\phi(n)=(p-1)p\cdot(q-1)$
Euler's Theorem: for $a>1$ and $n > 1$, if $gcd(n, a)=1$ then: $a^{\phi(n)}\equiv 1 \mod n$ and $a^{\phi(n)-1}\equiv a^{-1}\mod n$
Fermat's Little Theorem: $a^{p-1}\equiv 1 \mod p$ for $gcd(p, a)=1$ and $p$ is prime
$a^{p-2}\equiv a^{-1}\mod p$ for any prime $p$
Use Fermats $a^{n-1}\mod n = 1 \mod n$  to test for primality. Carmichael numbers will pass even though they aren't prime.
Diffie-Hellman: $S = B^{a}=(g^{b})^{a}=g^{a\cdot b}$
ElGamal: Diffie-Hellman as encryption: $C_{0}=g^{b}$ $C_{1}=M\times A^{b}$ . Decryption then $M=C_{1}'\times(C_{0}')^{-a}$ All in the group $\mathbb{G}$
Euclidean Algorithm: $gcd(a, b)=d$. Find $a=q\cdot b + r$. Find $b=q\cdot r + r_{2}$. Continue.
## [[Lecture 08]]
RSA: use $n=pq$ with large primes
public exponent $e$ such that gcd$(e, n)$=1 (so that inverse exists)
$K_{p}=(n, e), k_{s}=(p, q, d)$
$RSA(M, K_{p}) = M^{e}\mod n$  $RSA^{-1}(C, K_{s})=C^{d}\mod n$
if $M = lq$: $M^{1+k\phi(n)}\mod p = M$ , $M^{1+k\phi(n)}\mod q = 0$. CRT to find M
CRT can be used to decrypt RSA anyway, quicker
## [[Lecture 09]]
RSA Signature: $K_{V}=(n, e), K_{s}=(p,q,d)$ 
$s = m^{d}\mod n$, $v = s^{e}\mod n \equiv m$
**Hash and sign better in practice**
ElGamal Signature:  $K_{p}= y := g^{x}\mod p$, $K_{s}= x \in \{1\ldots p-2\}$
$K_{s}= k \text{ s.t } gcd(n, k) = 1$
$s = (H(m)-xr)k^{-1}\mod(p-1)$ , $s := (r, s)$ 
$K_{V}= g^{H(m)}\equiv y^{r}r^{s}\mod p$ 
## [[Lecture 10]]
Elliptic curve: $y^{2}=x^{3}+ax +b$
No separate circle if $y^{2}=x^{3}+73$ e.g a=0, b=73
$$
\begin{align} 
P+Q &= R, &\text{with P, Q, and $-R$ aligned on the curve} \\
P+ (-P) &= -O := O &\quad \text{Vertical line}\\
P + O &= -P := P &\quad \text{Operating on $O$}\\
O + O  &= O &\quad \text{Double $O$ is $O$}
\end{align} 
$$
$x_{R}= \lambda^{2}-x_{1}-x_{2}$
$y_{R}=-(\lambda x_{R}+v)$, with $\lambda =$ slope, $v =$ y-intercept of line
size of elliptic curve in finite field $|E| \in p+1\pm 2\sqrt{p}$
All the integers ($\bmod p$) that satisfy the elliptic curve equation
Slope: $s = (y_{Q}-y_{P}) \cdot (x_{Q}-x_{P})^{-1}\pmod p$
$x_{R}= s^{2}-x_{Q}-x_{P}\pmod p$
$y_{R}= -y_{Q}+s(x_{Q}-x_{R}) \mod p$
For doubling point: $s = (3x_{P}^{2} + a) \cdot (2y_{p})^{-1}\pmod p$
Double and add to multiply points: $nP = p+p+p...$
Write $n$ in binary, start with $r=O$. for each bit, double $r$, and add $P$ if bit is 1.
Elliptic Curve Diffie-Hellman: Take a curve from generator $G$, $K_{s}=a$, $K_{p}=A=aG$
$S = Ba\times G = Ab \times G$
Discrete logarithm problem without knowing $a$ or $b$. This is $\mathcal{O}(p)$, where $p$ is the order
## [[Lecture 11]]
Authenticated Diffie-Hellman: Use long-term keys to agree on a random session key.
$K_{AB} = (g^{a})^{b} = g^{ab}\mod p$ 
Shamir Secret Sharing: with prime $p$, and $f(x)$ a polynomial with coefficients in $\mathbb{Z}_p$.
For (t, n) scheme, use: $f(x) = a_{t-1}x^{t-1}+\ldots+a_{1}x+a_{0}\mod p$
$a_{0}=k=f(0)$ $a_{i}$ chosen at random for all $i$ distribute $f(i)\mod p$ to each member
As long as $n >= t$, $t$ members can come together and compute $k$




