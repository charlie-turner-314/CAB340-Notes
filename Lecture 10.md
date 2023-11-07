# Elliptic-curve cryptography
**Why?**
- Public-key crypto super useful
- DLog-based and factoring-based uses very large integers (1000s of bits), takes time to compute and transmit
- Hardness of Factoring and DLog isn't 'tight' - hardness decreases over time, and required length increases dramatically with target security
## Arms Race
### Defense
Attack complexity to grow exponentially with the 'security parameter' $\lambda$. e.g key size for symmetric ciphers
### Offence
Moore's law, computing power getting better and better all the time

# Mathematical Refresher
- Set: group of distinct objects
	e.g All integers, all positive integerrs, all real numbers (infinite)
- Finite Set: set where we can list all the elements
- Binary operation $\odot$ combines two elements in a set
**Groups**
$(S, \odot)$ is a group if it has:
1. Closure - using the operator on any two elements in S is also an element in S
2. Associativity - no-one cares which way you use the operator between 3 elements ($a\odot b \odot c = a \odot (b\odot c) = (a\odot b)\odot c$)
3. Identity Element - There exists an element $e$ such that $a\odot e = a$
4. Inverse Elements - For each element $a$, there is an element $\bar{a}$ such that $a \odot \bar{a} = e$ 
**Abelian and Cyclic Groups**
Abelian group is also commutative:
$a\odot b = b \odot a$
Cyclic if the group has a generator - one element that, if you apply the operation to it repeatedly will go through all elements of the group

**Ring**
Set with *two* operations, usually addition and multiplication
Has all the group things, is commutative of $+$ , has an additive and multiplicative identity, and there is always an Additive inverse for everything
**Field**
A set with two operations such that:
$(S, +)$ is an abelian group with identity 0
$(S\backslash \{0\}, \times)$   is an abelian group under $\times$ (e.g S without 0)
$(S, +, \times)$ is distributive (e. g $a \times (b+c) = ab + ac$)
> A field is a ring whose multiplication operation forms a commutative group without 0

## Relations and Functions
**Relation**
A (can be non-exhaustive) set of ordered pairs (permutations) of the elements of two sets
**Function**
A relation in which no two pairs have the same first element
**Injection**
Function in which no two pairs have the same first *or second* element 

# Elliptic Curves
## With Real Numbers
Using degree-3 relations:
$$
y^{2}=x^{3}+ax+b
$$
![[Pasted image 20231105215359.png]]
The set of points on an elliptic curve *almost* forms a group.
If you connect 2 points on a curve of degree 3, there must be another point if you extend the straight line. (**Unless the line is vertical**). e.g Taking two points, you can find a (unique) other point on the curve.
> Use this to define the operation for the group:

$$
P_{1}\odot P_{2}=P_{3}
$$
Where 
- $P_1$ and $P_{2}$ are two points on the curve, 
- $\odot$ is the binary operator, extending the straight line that joins the two points
- $P_{3}$ is the result (also provably on the curve).
**But, what if your point is vertical? Then it might only go through 2 points!**
Fix the group by defining a point at infinity where all vertical lines intersect - $O$.  
	Made up point to fix the vertical line problem
	Maybe not quite made up, the elliptic curve goes vertical, so therefore any vertical line will intersect at $y=\infty$ infinity
 **But how is this a Field? $P_{1}\odot O \ne P_{1}$ , there isn't an identity!**
 > Because of the vertical symmetry, just change $\odot$ to be:
 > 1. Join the two points with a straight line
 > 2. Extend the line onto another (different) point
 > 3. Mirror it over the x axis (take the negative)
 
$$
\begin{align} 
P+Q &= R, &\text{with P, Q, and $-R$ aligned on the curve} \\
P+ (-P) &= -O := O &\quad \text{Vertical line}\\
P + O &= -P := P &\quad \text{Operating on $O$}\\
O + O  &= O &\quad \text{Double $O$ is $O$}
\end{align} 
$$
### Calculating the Group Operation
'Draw a line' through $(x_{1}, y_{1})$ and $(x_{2}, y_{2})$:
$$
\begin{align}
y &= \lambda x +v \quad \text{ Standard form of a straight line} \\
&\text{complicated equation....}\\
&\text{calculating and proving the slope....}\\
x_{R}&= \lambda^{2}-x_{1}-x_{2}\\
y_{R}&= -(\lambda x_{R}+ v)
\end{align}
$$
Notice how for $y_R$, we don't need to use the cubic, because we know it's on a straight line from the other points.
## With Finite Fields
Same idea, but:
1. The elliptic curve E over the finite field $\mathbb{F}_p$ is the set of points $(x, y)\in\mathbb{F}\times\mathbb{F}$
$$
y^{2}=x^{3}+ax +b \pmod{p}
$$
2. Same definition/calculations as the reals, but with ints
3. Not a curve (disjoint cause only integers)
4. $|E|$ (number of points) close to $p$, in the range $p+1 \pm 2\sqrt{p}$ 
Basically, all the *integer $\bmod p$*   points that satisfy the same equation as before. 
**Example:**
For $a=3$, $b=2$, $p=7$:
$$
\begin{align}
y^{2}&=x^{3}+ax+b \mod p \\
&\text{Start at 0} \\
y^{2}&= 0 + 0 + 2 \mod 7 \\
y&= 4 \quad ( 4^{2} = 16 = 7*2 + 2)\\
y&= 3 \quad ( 3^{2} = 9 = 7 + 2) \\
&\text{For x=1} \\
y^{2}&= 1^{3}+3(1)+2 = 8 \equiv 1 \mod 7\\
y&= \text{nothing works $\bmod 7$}\\
&\text{For x=2} \\
y^{2}&= 2^{3}+3(2)+2 = 16 \equiv 2 \mod 7\\
y&= \text{same as for $x=0$}
\end{align}
$$
Continue this for all $x\in [0, p]$ To get points which both $x$ and $y$ satisfy the equation. 
### 'Add' two points in a finite field
1. Find the slope between $P$ and $Q$:
$$
s = (y_{Q}-y_{P}) \cdot (x_{Q}-x_{P})^{-1}\pmod p
$$
2. Find the x coordinate of the resulting point
$$
x_{R}= s^{2}-x_{Q}-x_{P}\pmod p
$$
3. Find the corresponding y coordinate, and mirror
$$
y_{R}= -y_{Q}+s(x_{Q}-x_{R}) \mod p
$$
> Notice how we had to use the inverse in step 1. This is a reason why we must work in the **finite field** $\mathbb{F}_p$, and not just any group/ring

### 'Add' a point to itself
1. Find the slope tangent to the point (this line 'intersects' P twice)
$$
s = (3x_{P}^{2} + a) \cdot (2y_{p})^{-1}\pmod p
$$
2. Find the x coordinate of the other point where the tangent intersect the 'curve'
$$
x_{R}= s^{2}-2x_P \pmod p
$$
3. Find the y coordinate, and mirror
$$
y_{R} = -y_{P}+s\cdot(x_{P}-x_{R}) \pmod p
$$
> This is the basis for multiplication! $2P = P+P = (x_{R}, y_{R})$

## Double and Add Point Multiplication
```
let bits = bit_representation(s) # the vector of bits (from LSB to MSB) representing s
   let res = O # point at infinity
   let temp = P # track doubled P val
   for bit in bits: 
       if bit == 1:            
           res = res + temp # point add
       temp = temp + temp # double
   return res
```
Very similar to square and multiply, where we find the shortest (binary) path from 
$P$ to $nP$, by just doubling it and adding it to itself a heap of times. 

## Elliptic Curve Diffie-Hellman
Using $\mathbb{G}$, an abelian cyclic group on an elliptic curve of large prime order $p$, with generator $G\in\mathbb{G}$.
1. Alice picks a secret key $a$ such that $1<a<p$ , and her public key as $A=aG$
2. Bob picks a secret key $b$ such that $1<b<p$, and his public key as $B=bG$
3. Alice and bob derive shared secrets:
$$
\begin{align}
S = Ba\cdot G \\
S' = Ab\cdot G \\
\end{align}
$$
> Since $B = bG$, $S=b\cdot a\cdot G \cdot G$
> Since $A=aG, S=a\cdot b\cdot G\cdot G$
> These are equal, yet without knowing $a$ or $b$, we have a **discrete logarithm problem** i.e. an attacker needs to figure out the (very large) secret number that was used to generate $A$ (or $B$) from $G$ (the secret key $a$ (or $b$)). This is hard




