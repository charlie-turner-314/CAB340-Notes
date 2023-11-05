# Hash & MAC
# Hash Functions
> Building blocks for MAC, transform an arbitrarily long strig into a short string hard-to-reverse. No key, but still cryptographic

- H is reasonably simple and very fast
- Takes an input of any length and outputs a fixed length
	- Short as possible for efficiency
	- Not too short, don't want collisions
## Security Properties
**Collision Resistant** - It should be infeasible to find any two inputs with the same output
**Second-Preimage Resistant** - Given a known input, it should be infeasible to find a different input with the same hash
**One-way** - Given a hash value, it should be infeasible to find the input.

Collision resistance infers second-preimage resistance, not the other way around though. If you break second-preimage resistance, you have broken collisions.

### Birthday Paradox
Collisions become exponentially more likely over time, as there are more things to collide with.  
**Example**
Hash function has an output of $k$  bits. If H is random then $2^{k/2}$ trials are enough to find a collision with probability around 0.5
$2^{128}$ is still considered infeasible, therefore 256 bits is happily collision resistant. 
## Merkle-Damgard Construction
- Need to take arbitrary input and produce fixed size output
- Like block ciphers, process fixed-size input over and over
- Use a fixed compression function applied to multiple blocks of input

1. Break the message into $n$-bit blocks
2. Add padding to end
3. Compress first with IV in function $h$, then second with output of first etc.
- If $h$ is collision resistant, so is $H$
- Once you have one collision, easy to find more
## Standardized Hash Functions
### MDx
- 128-bit output
- Very broken
- Can do MD4 collision by hand!!!?
- Broken broken broken
### SHA-0 and SHA-1
- 160 bit output
- SHA-0 broken
- Broken broken broken
- In 2017, SHA-1 broken in about $2^{63}$ evaluations
- Bitcoin network does more than that (and a more complex function) every second
### SHA-2
- secure!
- Family with 224-512 bit outputs
- 32 or 64 bit blocks
### SHA-3
- Sponge construction
$$
\pi : {0, 1}^{1024}\rightarrow {0,1}^{1024}
$$
- $\pi$ is non-linear
- $\pi$ is fixed and known
- $\pi$ has same in and out, easy to iterate
- $\pi$ so wide that you can take large bits out or add them ?
1. iterate $\pi$ for as many input blocks as needed
2. Start from an initial state like all 0
3. Feed each 512-bit block in by $\oplus$ with top half of current
4. at the end, use the bottom half as the hash



# Message Authentication Codes

> Provide data autheticity and integrity. Only someone with the secret key can generate a valid MAC
- Ensure not altered in transmission
- Prevent ffrom reordering, replicating, or deleting blocks in order to alter the message
- With a secret key $K$ and a message $M$, give a fixed length string:
$$
T = MAC(M, K)
$$

## CBC-MAC
- Use a block cipher
- IV is fixed and public
- Break message into $l$ blocks
	- Set $C_{0}=IV$
	- For $t=1$ to $l$: compute $C_{t}=E(M_{t}\oplus C_{t-1}, K)$
	- Output $T = C_l$ as the tag
## HMAC
$$
HMAC-H(M, K) = H((K\oplus \text{opad}) || H((K\oplus \text{ipad})||m))
$$
- K: key padded to be the block size of H
- opad is fixed string
- ipad is fixed string
- Secure if H is collision resistant or if H is a random function
# Combining Encryption and MAC

*E&M* - Encrypt M; MAC M, send the two
*MtE* - Mac M to get T; Encrypt M||T; send that
*EtM* - Encrypt M to get C; Mac C; Send the two

