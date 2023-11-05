# Fundemental questions of cryptography:
1. How much encrypted plaintext does the cryptanalyst need in order to obtain the correct key?
2. What does it mean to be an unbreakable cipher?
3. Does any such cipher exist?

# Information Theory
## Probability
**State**
Often denoted $\omega$, a hypothetical full characterisation of the world
***
**Event**
Often denoted by uppercase such as $X$, a **set of states** which a probability can be asigned.
- Can think of as the binary function of reaching a state
***
**Probability**
Denoted $P$ or $Pr$, a real function that assigns events a number from 0-1
### Random Variables
> Not random, nor variable
Function that maps world states $\omega$ to values $x$ in a domain $\chi$. 
$$
\begin{align}
X &: \Omega \rightarrow \mathcal{X} \\
&: \omega \rightarrow x = X(\Omega) \in \mathcal{X}
\end{align}
$$
> i.e, on an input state $\omega$, the real-value $X$ outputs value $x = X(\omega) \in \mathcal{X}$ . We observe X to be x.

### Multi-events
**Joint probability**: two or more things happen together
**Marginal Probability**: One thing happening neglecting everything else.
**Conditional Probability**: One thing happening with the knowledge something else happens. (given A happens, how likely is B)
$$
Pr[X, Y] = Pr[X|Y] \times Pr[Y]
$$
### Independence and Bayes' Theorem
Independent if the conditional probability is the same as the marginal probability.
$$
Pr[X,Y] = Pr[X|Y]Pr[Y] = Pr[X]Pr[Y]
$$
> I.e simply multiply independent events to get joint prob

Provided that $Pr[Y]\ne 0$ , 
$$
Pr[X|Y] = \frac{Pr[X]}{Pr[Y]}Pr[Y|X]
$$
## Information
Basically information is the degree of 'newness' that can be known from an event. E.g if $Pr[X]=1$, we don't learn anything from its occurrence, so information of that events probability is 0.

Negative logarithm of an event's probability does this. 

The information from an event with probability $p$ would be 
$$
\log_2{\frac{1}{p}} = -\log_{2}{p} \ge 0
$$
- Information in *bits*
- If something is certain, we learn nothing
- If something is unlikely, we learn a lot from it happening, but not much from it not happening
## Entropy
> How much information we expect from observing the outcome of a random variable

**Example**
Consider $M = {a, b, c}$ with
$$
Pr(a)=\frac{1}{2}, Pr(b)= \frac{1}{4}, Pr(c)=\frac{1}{4}
$$
$$
\begin{align}
H(M) &= \frac{1}{2}\log_{2}{2} \\
&=\frac{1}{2}+ \frac{1}{2} + \frac{1}{2} \\
&= \frac{3}{2} = 1.5 bits
\end{align}
$$
## Relation to Cryptography
- Entropy maximum when all values are equally likely
- In order to maximise the attackers uncertainty, choose keys uniformly
- In modern ciphers the key will be a bit string of $j$ bits so that $n=2^j$ and then there are $j$ bits of information. Maximized entropy  = the whole key is important, all bits are uncertain

# Information Theory In Cryptography
### Rate of a language
> average number of bits in each character.

- Analyse huge amount of text, english about $r\approx 1.5$ bit/letter
- No mathematical definition but approximated
***
### Redundancy
> difference between the information that is, and the information that could be carried with each character

We know that for $L$ characters, $H_{max}=\log_{2}{L}$ 

## Unicity Distance
How much plaintext can be encrypted before there is enough information to uniquely determine the key just from ciphertext
- about being possible, not feasible
- a bad cipher can be broken before the unicity distance if powers are greater than a ciphertext-only attack
$$
N \approx \frac{H(K)}{D}
$$
Consider a random substitution cipher. Entropy is 93.1 bits, redundency is about 3.25 bits/letter
Unicity distance is then about 29.1 letters

> This says that after about 30 characters, there is a unique key that the ciphertext will decrypt to english (ish) plaintext.

N can be increased by decreasing redundancy. 
## Perfect Secrecy
> No information gained from observing the ciphertext

Probability comes in here, if the conditional probability of seeing a message turn into that ciphertext is the same as the probability of just seeing that ciphertext or message

