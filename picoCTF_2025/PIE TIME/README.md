# PIE TIME - PicoCTF 2025 (Binary Exploitation)

## Introduction
On starting an instance, a C file and it's compiled binary are given which runs on the netcat server.

## Analysis
Looking at the C code, we see that the `main()` function prints it's own address and then asks for a random address and runs it.
We need to run the `win()` function to print the flag.

The binary has `PIE` enabled which adds a random offset on each run.

## Exploiting
Using `objdump -d` or `gdb` to disassemble the file and look at the memory addresses of the functions.
We find the addresses of `main()` and `win()` and their difference comes out to be 96.

On connecting to the netcat server, after recieving the `main()` function address, we subtract 96 from it and input it, which runs the `win()` function outputs the flag.

## Flag
picoCTF{b4s1c_p051t10n_1nd3p3nd3nc3_00dea386}
