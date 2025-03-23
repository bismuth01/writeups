# hashcrack - PicoCTF 2025 (Cryptography)

## Introduction
A netcat server is given which got breached due to usage of weak passwords.

## Decrypting
On connecting to the server, we get a hash which seems to be an md5 hash. On using `john` with the format `raw-md5`, we can crack the password to be `password123`.
Inputting the password, we are given another hash.

This one seems to be sha1 and repeating the process, using `john` with format `raw-sha1`, we crack the password `letmein`.
Another hash is given after this.

This one looked like sha256. Using `john` with format `raw-sha256`, we crack the password `qwerty098`.

After this flag is printed.

## Flag
picoCTF{UseStr0nG_h@shEs_&PaSswDs!_dcd6135e}
