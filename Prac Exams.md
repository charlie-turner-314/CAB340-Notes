1. C
2. A  *
3. D
4. C
5. C
6. 60 (D)
$n=99$ 
$\phi(99) = 3^{2}\cdot 11 = 2*3\cdot 10$
$=6\cdot 10=60$
7. D
8. D (only for $a$ coprime with $n$)
9. C
10. B
$p=11$ , $g=3$
$A = 9$, $b=2$
$S=B^{a} = 9^{2}\mod 11 = 81\mod 11 = 4$ 
11. B
12. A but could be B? Sig=1
13. A
14. A
15. B (MAC similar to hash)
16. B
17. A
18. D
19. D
20. D

21. 
a)
ZigZag Cipher.
$K = \text{number of rows}$
$E(M, K) =$ Write M on K rows diagonally, and read L2R
e.g for M = HELLO THERE, K=3
```
H   O   R
 E L T E E
  L   H   X
```
C = HORELTEELHX
$D(C, K) =$ Write C on K rows (L2R) and read diagonally
```
H   O   R
 E L T E E
  L   H   X
```
P = HelloThereX (drop the X)
b)
While the ciphertext is simply a transposition, there is no obvious relationship between the letters of the ciphertext, e.g no periodic behaviour 
22. a) let ${s_{0}, s_{1}, s_{2}} = {0, 1, 1}$
$s_{03}=s_{0}\oplus s_{1}=0\oplus 1 = 1 \quad 0111$
$s_{04}=s_{1}\oplus s_{2}=1\oplus 1 = 0 \quad 01110$
$s_{05}=s_{2}\oplus s_{3}=1\oplus 1 = 0 \quad 011100$
$s_{06}=s_{3}\oplus s_{4}=1\oplus 0 = 1 \quad 0111001$
$s_{07}=s_{4}\oplus s_{5}=0\oplus 0 = 0 \quad 01110010$
$s_{08}=s_{5}\oplus s_{6}=0\oplus 1 = 1 \quad 011100101$
$s_{09}=s_{6}\oplus s_{7}=1\oplus 0 = 1 \quad 0111001011$
$s_{10}=s_{7}\oplus s_{8}=0\oplus 1 = 1 \quad 01110010111$
Period is 7, i.e 0 1 1 1 0 0 1




