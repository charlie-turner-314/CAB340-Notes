[[Lecture 2#Polygraphic Ciphers]] based on linear transformation
Operates on $d$-graphs viewed as vectors
key is a $d\times d$ matrix
Produces vectors of ciphertext
## Encryption
1. Encode each letter of the plaintext as a number. E.g A=0, B=1 etc.
2. Group those into $d$-vectors 
3. Calculate each ciphertext $d$-vector as $C=K\cdot P \mod N$
4. Decode this as a string (concat vectors into alphabet)
## Decryption
> Inverse of encryption
1. Encode the letters of the ciphertext as numbers
2. Group into $d$-vectors
3. Calculate the plaintext vectors $P=K^{-1}\cdot C \mod N$
4. Decode the vectors and convert to string

## Modulo Inverse Matrix
Same as normal algo, just modular
e.g for 2x2
$$
\begin{pmatrix}
a & b \\ c & d
\end{pmatrix}^{-1}
= (ad - bc)^{-1}\cdot 
\begin{pmatrix} 
d & -b  \\ -c & a 
\end{pmatrix}
$$
## Cryptanalysis
Pretty much broken, as with most linear ciphers
### Known Plaintext
If you have $d$ matching pairs of blocks
e.g $d^2$ characters
1. Let $C$ be the matrix of all known vectors of $C_i$
	e.g if you have $[1, 2]$ and $[3, 4]$, $C=\begin{pmatrix}1 & 3 \\ 2 & 4\end{pmatrix}$
2. Let P be the matrix of all known vectors of $P_i$
3. Solve C = KP for the key K
4. Verify and solve anything you like
> Note: need to have invertible matrix K at step 3, otherwise 4 won't be possible. May need to try with other sets or a different order
### Ciphertext Only
Use $d$-graph frequencies to guess the plaintext for some ciphertext blocks
and then do known plaintext.
Takes a while but automatable, not as tough as brute force
