Not useful
I and J considered the same for some reason
lets you write the key as a 5x5 square, starting with the first row being a keyword (left to right, wrap to second row etc if needed). Fill out rest alphabetically

Works on *different letter* digraphs: e.g pad SS -> SXS or fold LL -> L

Play around with the square to encrypt:
```
FOR each DIGRAPH (a, b)
	IF row(a) == row(b):
		Replace each with letter to the right (wraparound on row)
		a' = GRID[row(a), col(a)+1]
		b' = GRID[row(b), col(b)+1]
    IF col(a) == col(b):
	    Replace each with letter below (wraparound on col)
	    a' = GRID[row(a)+1, col(a)]
	    b' = GRID[row(b)+1, col(b)]
	ELSE:
		Switch cols, keep rows for each in digraph
		a' = GRID[row(a), col(b)]
		b' = GRID[row(b), col(a)]
```

Enourmous key space - defeats single letter frequency cryptanalysis
Same plaintext digraphs map to same ciphertext digrams:
	just do digraph frequency analysis
