# Even RSA can be broken - PicoCTF 2025 (Cryptography)

## Introduction
A netcat server command is provided to connect to it which gives us the N & e being used for the RSA and the encrypted cipher text.
We are also given the source code being hosted on the netcat server.

## Decrypting
Using `factordb.com`, we can input N and get it's factors, which are essentially the p and q values we need to decipher the RSA.

Using p and q, we can get d and successfully break the cipher text. On deciphering, we get the flag.

## Flag
picoCTF{tw0_1$_pr!m3993b4dd0}

## Appendices

encrypt.py
```
from sys import exit
from Crypto.Util.number import bytes_to_long, inverse
from setup import get_primes

e = 65537

def gen_key(k):
    """
    Generates RSA key with k bits
    """
    p,q = get_primes(k//2)
    N = p*q
    d = inverse(e, (p-1)*(q-1))

    return ((N,e), d)

def encrypt(pubkey, m):
    N,e = pubkey
    return pow(bytes_to_long(m.encode('utf-8')), e, N)

def main(flag):
    pubkey, _privkey = gen_key(1024)
    encrypted = encrypt(pubkey, flag)
    return (pubkey[0], encrypted)

if __name__ == "__main__":
    flag = open('flag.txt', 'r').read()
    flag = flag.strip()
    N, cypher  = main(flag)
    print("N:", N)
    print("e:", e)
    print("cyphertext:", cypher)
    exit()
```
