# Digital Signatures
> Up until now, confidentiality - hiding information

- MACS, only the right key can create or write a valid tag, but both need to have same secret
- Digital signatures use PK to do the same, but without requiring shared secrets. Anyone can verify
- Signature verification still requires a verification key, but it can be made public as only one key can sign it
- Non-repudiation (authentication and integrity), since only the signer knows the private signing key

## Key Generation
Outputs:
1. A private signing key, $K_s$
2. A public verification key $K_V$ 
## Signing
Suppose Siggy wants to sign a message m:
**Sign($m$, $K_s$)**
Input:
1. any message
2. signing key $K_s$
Output: $\rightarrow$ signature $s=\text{Sig}(m, K_s)$
> Can sign any message $m\in\{0,1\}^*$, e.g bit string of any length
> Typically signatures are of a fixed length
## Verification
Suppose victor wants to verify that Siggy was the one who sent the message:
**Verify$(m, s, K_v)$**
Input:
1. a message $m$
2. a claimed signature $s$
3. the public verification key $K_V$ 
Output $\rightarrow$ true or false
## Properties
1. Correctness (Completeness)
	1. For all $(K_{S}, K_{V})\leftarrow$KeyGen() and valid $m$, 
		if $s = \text{Sig}(m, K_{S})$ then $\text{Ver}(m, s, K_{V})=1$
2. Security (unforgeability)
	1. It is infeasible for an attacker, given the public $K_{V}$, to find a message $m$ and a signature $s$ such that $\text{Ver}(m, s, K_{V}) = 1$
## Goals Against Signatures
1. **Key Recovery**
	The attacker wants to recover $K_{S}$ from any available info
2. **Universal Forgery**
	The attacker wants to forge a message on *any* message. Given a message, they want to forge $s$
3. **Existential Forgery**
	The attacker wants to forge a signature on a chosen message. Doesn't have to be *any* message, just finding a single message that he can forge a valid $s$
**Hardest:** Key recovery
**Easiest:** Existential Forgery (protect this for maximum security)
## Powers Against Signatures
1. **Key-only attacks**
	Given only $K_{V} \rightarrow$  find $K_{S}$ OR forge any valid $s$ OR forge a single valid $s$
2. **Known-message attacks**
	Given signatures for any messages, but they can't choose the messages
3. **Chosen-message attacks**
	Signing oracle can give attacker signatures for any message

> **Gold Standard**: *existential unforgeability* under *chosen-message* attacks

# RSA Signatures

Using $n=p\times q$, and the public exponent $e$, and private exponent $d$
$\rightarrow$ $K_{V}=(n, e)$
$\rightarrow K_{S}=(p, q, d)$
**Private Operation - Signing**
$$
s = m^{d}\bmod n
$$
**Public Operation - Verification**
$$
v = \cases{1, \text{ if } s^{e}\bmod n \equiv m\\0, \text{ else}}
$$

> Only the private key can sign a message, but anyone can verify using the public key!

## Security Of RSA Signatures
1. If messages have low redundancy (all possible messages are meaningful), an attacker can just derive a matching message from a random signature. In reality, messages are quite redundant (remember, english is pretty redundant)
	- In this case, the attacker cannot choose the message. Basically would have to trial $s\rightarrow m$  combos until something interesting
2. Start from signed messages, and modify/combine signatures to get a new message with a new signature
	- given $m_{1}, m_{2}$ and $s_{1}, s_2$ :
		- $m' = m_{1}m_{2}\bmod n$
		- $s' = s_{1}s_{2}\bmod n$
		- This is valid because maths

### Major Problems
1. Cannot sign any message in $\{0, 1\}^*$, only in $\mathcal{Z}_n$
	1. e.g has to be less than $n$, remember RSA applies to any message from 0< M < n
2. Vulnerable to the maths (homomorphism) attacks above
### Hash and Sign
- Turn the arbitrary message into a fixed length representation
- Collission resistant $\rightarrow$ non-homomorphic, there is no mathematical properties that can be (feasibly) exploited to forge a hash

> The *signer* can hash the message, then sign that. 
> The *verifier* can then hash the received message, and verify that

1. $m$ is no longer constrained to less than $n$
2. one-time RSA signature is now reusable

# DLog Signatures
## ElGamal Signatures
**Signing:**
1. Siggy picks $k$ such that gcd$(k, p-1)=1$ and computes
$$
r = g^{k}\bmod p
$$
2. Siggy solves $m \equiv xr +ks \pmod{(p-1)}$ for $s$ by:
$$
s = k^{-1}(m-xr) \mod (p-1)
$$
3. Siggy outputs $(m, r, s)$
**Verification**:
1. Victor verifies that $g^{m}\equiv y^{r}r^{s} \pmod p$
> Inherently *randomised*, Hash-and-sign can and should still be applied

## DSA
- Work in a subgroup of prime order $q$ within multiplicative group $\mod p$. This makes the signatures shorter and avoids weaknesses that arise when groups aren't prime
## ECDSA - Elliptic-Curve DDigital Signature Standard
- Same principles, but using an elliptic-curve group
- Harder attacks
# Variations of Digital Signatures
**Blind Signatures**
Unknown what the user signs
**Proxy Signatures**
Signer can delegate their signing capabilities without revealing their full key
**Group Signatures**
Any member of a group can anonymously sign on the group's behalf
**Ring Signature**
Anyone can make an anonymous signature on behalf of a 'ring' of signers. 
**ID-based Signature**
Public verification key can be a real name, and the private key must be calculated by a trusted authority

## Breaking Signatures via Weak Hash Functions
> Suppose the hash function in a hash-and-sign method is not very good
1. Attacker has a target message $y$
2. Attacker finds a second preimage $x$ such that $H(x)=H(y)$
3. Alice signs $x$, now that signature can be used to 'verify' $y$
*Universal Forgery under Chosen-Message Attack*
1. Attacker observes a message $x$ that Alice signs
2. Attacker finds $y$ such that $H(x)=H(y)$
3. Attacker has Alice's signature on both $y$ and $x$
*Existential Forgery under Known-Message Attack*

> **Just make sure the hash function is secure, then the signature (should be) secure**

# Digital Certificates
Sometimes, we want to trust that a public key is actually from the entity we think it is. E.g, encrypting with Bob's $K_{P}$ is useless if its actually Darren in a moustache. 

> Use a trusted third party (TTP) to vouch for a key and identity pair

**At some point, there has to be trust in the infrastructure, through open-source, public consensus etc.**


