
## Square and Multiply
> A way to compute $y=b^{x}\mod n$ , given known $b, x$, and $n$

1. Write $x$ in binary
2. let $y \leftarrow 1, l = \lfloor{\log_{2}e}\rfloor$  (number of bits of number + 1, zero index from MSB)
3. For $i=l,...,0$ (for each bit from MSB to LSB)
	1. If $i$th bit $x_{i}=0$, let $y\leftarrow y^{2} \mod n$
	2. If $i$th bit $x_{i}=1$, let $y\leftarrow y^{2}b \mod n$
 4. That's the answer
 **Example**
 find $y=7^{3} mod 10$
 1. `11`
 2. $y=1, l=1$
 3. $i\in {0, 1}$
	 0. $y\leftarrow 1\times 7 \mod 10$  
		 $y\leftarrow 7$
	 1. $y\leftarrow 7^{2}\times 7 \mod 10$
		 $y\leftarrow 343 \mod 10$
		 $y\leftarrow 3$
4. $y=3$
This is not a great example, just had to do the whole damn thing. Guess if the numbers are much bigger, mod brings it back down to a usable number when in reality $5248924^{213243234}$ would get too big to handle
## Chinese Remainder Theorem
Let $n=pq$ be the product of two distinct primes. Let $a, b$ be two givens. There exists a unique number $0\le m < n$ such that:
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


## Euler's Totient
> The *totient* of a number sounds made up, but is the number of $x\in[1,n]$ which are *relatively prime* , or *co-prime* with $n$ . To be co-prime means to have g.c.d 1

$$
n=\prod\limits_i{p_i}^{e_{i}}, \text{ The totient of n is } \phi(n)=\prod\limits_i{(p_{i}-1)p_{i}^{e_{i-1}}}
$$
Rule: *Write out the prime factors, and subtract 1 from the first prime in each group*
- If $n=pq$, is the product of two primes, then:
$$
\phi(n) = (q-1)(p-1)
$$
- $\phi(27) = \phi(3^{3}) = (2\cdot3\cdot3) = 18$
**Why?**
The *generators* of the *additive cycle* group $\mod n$   are exactly those $x$ such that $g.c.d(x, n) = 1$ 
	**In english:** The co-primes of $n$ are the numbers which, when added to any number $\mod n$ repeatedly, will go through all $1, ..., n$ (not in that order)

### Euler's Theorem
For $a > 1$ and $n > 1$, if $gcd(n, a) = 1$, then:
$$
\begin{align}
a^{\phi(n)} &\equiv 1 \mod n \\
a^{\phi(n)-1} &\equiv a^{-1} \mod n
\end{align}
$$
> In a group size $o$, if you apply the group operation $o$ times, you will get back to the identity element. e.g 4+4...+4 (10 times) = 40 = 0 mod 10
***
> *If you know $\phi(n)$, you can find inverses mod n with Euler's theorem

### Fermat's Little Theorem
> Tiny tiny theorem

Let $p$ be a prime and $a$ be a positive integer such that gcd(p, a) = 1. Then:
$$
a^{p-1} \equiv 1 \mod p
$$
- Because $\phi(p)=(p-1)$ when $p$ is prime (e.g every other number is coprime with a prime $p$)
- Special little case of Euler's therem

$a^{p-2}\equiv a^{-1} (\mod p)$  for any prime $p$, given $a^{p-2} \ge 0$
# Testing for Primality
> There are a number of deterministic tests (correct every time), but these are impractical for large numbers
## Fermat's Test
> Simple, fast, but wrong for a small set of numbers. Can also be wrong by chance but can retest cause so fast

If we test $a^{n-1}\mod n\ne 1$, then $n$ is not prime
- Reduce the probability of being wrong by repeatedly picking different values for $a$
That's it, decide on a number $k$ of times to test random $a$'s, and return whether the number is (probably) prime, or definitely not
**Always Wrong Sometimes?**
*Carmichael* numbers:
- e.g 561, 1105, 1729, 2465
- 'Pass' the Fermat test, but aren't strictly prime
## Miller-Rabin Test
> Improvement over Fermat's test, and widely used in practice

**Not actually explained here**

1. Choose a random odd integer $r$ with the same number of bits as the desired prime
	1. E.g for 1024 bits, set the bits randomly, then set both MSB to 1 (to ensure wanted length) and LSB to 1 (to ensure odd)
2. Use the Miller-Rabin Test, and if prime then done!
3. Add 2 to get next odd, and repeat 2.
> Note: step 3 could be replaced with *Go to step 1* for more random distribution of primes
# Factorisation
> Finding the prime factors of a number is not easy


# Hard problems and Other Fun Stuff
**A 'Hard' problem is considered anything worse than exponential time**
e.g Brute force key search is exponential

## Problem complexity
> Classified by the *minimum* time and space required to solve the *hardest* instance of the problem

 -  e.g matmul has been done fastest in $\mathcal{O}(m^{2.37..})$ time, and it takes *at least* $\mathcal{O}(m^{2})$ just to read the matrices! So it's definitely somewhere between the two

# Public Key Cryptography
Secret key: secret
Public Key: public
Encrypt with public, decrypt with secret

## Discrete log
the number of times to apply a group operation from g to y, where g is the (a) group generator, and y is the target element

When the group operator is multiplication, this is just the regular log

# Diffie-Hellman
$$
S = B^{a} = (g^{b})^{a}=g^{b\cdot a}
$$
Since $b\cdot a \equiv a\cdot b$, two parties with secret keys $b$ and $a$, and public keys $B$ and $A$, can independently compute a shared key $S$


# ElGamal
> Turn Diffie-Hellman into a public-key encryption algorithm
- Bob uses a fresh $b$ and sends $B=g^{b}$ along with the message
**Key Generation**
1. Alice picks her secret key $a$ in the range $1<a<q$ and publishes the public key $A=g^{a}\in \mathbb{G}$ 
**Encryption**
Encrypt $M\in \mathbb{G}$, Bob selects a random $b$ and sends $C=(C_{0}, C_{1})\in \mathbb{G}$ where:
$C_{0}=g^{b}\in \mathbb{G}$
$C_{1}=M\times A^{b}\in \mathbb{G}$
**Decryption**
Alice computes $M' = C'_{1}\times (C'_{0})^{-a}\in \mathbb{G}$


