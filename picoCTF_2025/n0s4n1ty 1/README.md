# n0s4n1ty 1 - PicoCTF 2025 (Web Exploitation)

## Introduction
On launching an instance, we are given a website where we are given the option to upload a profile picture.
It is also told that the flag file is in the `/root` directory.

## Exploiting
We use a simple php backdoor and upload it on the profile picture section.

We are told the endpoint where we can find it and thus we go there and get access to the command line.
Using the backdoor, we execute the command `sudo cat /root/flag.txt` which gives the flag.

## Flag
picoCTF{wh47_c4n_u_d0_wPHP_80eedb7d}
