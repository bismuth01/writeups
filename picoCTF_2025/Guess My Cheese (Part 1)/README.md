# Guess My Cheese (Part 1) - PicoCTF 2025 (Cryptography)

## Introduction
A netcat server command is given where we join. On connecting to the server, we are given option to encrypt a cheese or to guess a cheese.
A hint is given that the cipher used is "The one that incorporates your affinity for linear equations???"

## Analysis
Using the hint, we try the Affine cipher. The Affine cipher requires 2 values - A and B using which the cipher can be decoded.

On connecting to the server, when we tell it to encrypt a cheese, it only takes in the names of actual cheeses rather than any strings.
I used the name `cheddar` and it successfully encrypted the name. It seems that the cheese name to guess also uses the same A and B values.

## Decrypting
Using `https://www.dcode.fr/affine-cipher`, I entered cheddar and brute forced it for all values of A and B. Then I took the encrypted `cheddar` and matched it.
From this I was able to extract the A and B values being used.

Now, I chose the option to guess the cheese, on which it gave me the encrypted cheese name. I decrypted the cipher using the acquired A and B values and guessed the right cheese name outputted.
On successfully doing so, it gave me the flag.

## Flag
picoCTF{ChEeSye67e919c}
