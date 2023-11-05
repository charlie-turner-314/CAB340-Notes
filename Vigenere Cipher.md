> A type of [[Polyalphabetic Substitution]] cipher. [[Lecture 2]]

- Use d different mappings depending on the position of the letter in the plaintext.
- Let the key specify
	1. The number d
	2. The order in which the mappings are invoked
	3. The substitution function defining each mapping - for a $d$-alphabetic cipher, there is $d$ of these
- Do this "compactly"

## What Is It
- Originally bellaso in 1553 - Really OLD
- Independently invented by Vigenere in 1585 - Also OLD
- Rotate through d instances of Caesar cipher.
- Each key letter indicates how much to shift a
- Each key is used in sequence, wrapping over at the end
$$
\begin{align*}
C_{i}&= (M_i+K_{i\mod{d}})\mod{26} \\
M_{i} &= (C_i-K_{i\mod{d}})\mod{26}
\end{align*}
$$
## Example
M = UNBREAKABLE
K  = BIG
C  = ?

We can say that shifts are $K_1=1, k_2 = 8, k_3 = 6$
Therefore
$$
\begin{align}
U &\rightarrow U+1 = V \\
N &\rightarrow N+8 = V \\
B &\rightarrow N+6 = H \\
R &\rightarrow R+1 = S \\
&\text{and so on} \ldots
\end{align}
$$
## Using the vigenere tableau
- If you ever use it (DONT)
- Theres a big table of letters plus letters. You can just add the key value + plain-text value and it shows you the thing. Not hard

|key/text|A|B|...|
|-|-|-|-|
|A|A|B||
|B|B|C||
|...||||

## Security
- Larger key space
- As $d$ increases, the number of possible keys increases as $26^d$, as opposed to simple caeser which is only 26 possible keys. 
- As $d \rightarrow \inf$  you get the Vernam cipher (Note this for later) 
- Better equalisation of letter histogram in the ciphertext:
	- Most frequent plaintext symbon (e) has not 1 but up to $d$ corresponding cipher symbols

## Breaking the Vigenere
**If you knew** $d$:
- View the vigenere cipher as $d$ interlaced caeser
- Attempt to break each caeser thread separately
> Doesn't have meaningful text, but have meaningful statistics. Given a large corpus and a relatively low $d$, could be helpful
**If you don't know** $d$:
1. Need to know d
2. Just brute force it and use ceaser threads.
	1. heuristic can be how 'ragged' distributions are in each thread
	2. the correct d will have the least mixed distributions, e.g each thread approaching known alphabet distributions

### Known plaintext attack
- From the known pairs, calculate the successive letters of K..
	- e.g $K_i = C_{i}-P_{i}$$
- At some point K will repeat as M repeats, if it occurs on a multiple of $d$
- For example, if there is 15 letters between repetitions, we now know that 
$$
d \in {1, 3, 5, 15}
$$
- Find another repetition with a different distance to narrow search

### Length Finding With Autocorrelation
- Repititions more than two cipher characters long are unlikely for pure chance
- Works because english plaintext and key both repeat

> Autocorrelation identifies correlations between a string and shifts of that same string

Can be seen as a more powerful generalisatoin of the method described above.
The correlation $Co$ beween two strings


