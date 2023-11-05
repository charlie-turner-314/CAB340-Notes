Essentially a [[Vigenere Cipher]], however, subtract plaintext from the key to get ciphertext
subtract ciphertext from the key to get plaintext
***
**Key:** period, offsets
**Encryption:** $C_{i}= (K_{(i\mod d)}-P_{i}) \mod 26$
**Decryption:** $P_{i}= (K_{(i\mod d)}-C_{i}) \mod 26$
***
> Note that the beaufort is reciprocal, same operation to encrypt and decrypt

**Advantage:** Hardware does not need 'modes', can always do both operations
**Disadvantages:** 
