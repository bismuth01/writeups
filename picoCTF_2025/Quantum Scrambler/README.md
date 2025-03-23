# Quantum Scrambler - PicoCTF 2025 (Reverse Engineering)

## Introduction
Program's source code is given and a netcat server on which it runs.

## Reverse Engineering
We read the code and reverse engineer the loops and encoding and write a script..

```
def descramble(scrambled):
    A = scrambled
    i = len(A) - 1

    while i >= 2:
        A[i-1].pop(-1)
        A.insert(i-1, [A[i-2].pop(-1)])
        i-=1

    return A

def reverse_hex_flag(hex_flag):
    return "".join(chr(int(x[0], 16)) for x in hex_flag)
```

On connecting to the netcat server, we are given the list to decrypt and putting it through this script, gives us the flag.

## Flag
picoCTF{python_is_weird9ece5f24}

## Appendices

quantum_scrambler.py
```
import sys

def exit():
  sys.exit(0)

def scramble(L):
  A = L
  i = 2
  while (i < len(A)):
    A[i-2] += A.pop(i-1)
    A[i-1].append(A[:i-2])
    i += 1

  return L

def get_flag():
  flag = open('flag.txt', 'r').read()
  flag = flag.strip()
  hex_flag = []
  for c in flag:
    hex_flag.append([str(hex(ord(c)))])

  return hex_flag

def main():
  flag = get_flag()
  cypher = scramble(flag)
  print(cypher)

if __name__ == '__main__':
  main()
```
