## Preventing Easy Attacks
Transpositional Ciphers
- Letters are reordered, distribution is equal to plain-text, just different letters.
- Key recovery attack, takes guesswork, but easy
	- given enough plaintext relative to key size
	- using something other than single-letter frequencies
Substitution Ciphers
- Do change distribution
- Same peaks, but in different places
How to strengthen
- Break the obvious mapping of peaks in substitution ciphers
1. Change the key for each letter - rotate through different keys to smooth out distribution. **polyalphabetic substitution**
2. Combine or group multiple letters into blocks and apply the substitution on whole blocks. **polygraphic substitution**. 

# Polyalphabetic Substitution
- [[Vigenere Cipher]]
- [[Beaufort Cipher]], [[Autokey Cipher]]

# Polygraphic Ciphers
## Rationale
Frequency statistics leak info from plain to cipher
Need a way to make the distribution more uniform.
E.g [[#Polyalphabetic Substitution]] to *rotate* through keys
**or**, combine letters into blocks
***
Cut input into $d$-letter blocks called $d$-grams
- digram: d = 2, e.g TH, IF
- trigram: d = 3, e.g THE, BUT, BRO
The key specifies:
- The number d
- The substitution mapping operating on $d$-letter blocks
***
See:
- [[Playfair Cipher]], [[Hill Cipher]]