# hash-only-1 - PicoCTF 2025 (Binary Exploitation)

## Introduction
On starting instance, a compiled binary of `flaghasher` is provided and a ssh server is given to log into which contains `flaghasher`.

## Exploitation
On putting it through the `angr` decompiler. We look at the main function which uses `system()` function to execute the `md5sum` command line program to generated the md5 hash of the flag.
Thus, if we could make a malicious version of `md5sum`, we could get direct acess to tthe flag.

We create a malicious version using the command `echo '#!/bin/bash' > md5sum; echo 'cat "$@"' >> md5sum`.
And give everyone it's execution rights with `chmod +x md5sum`.

Now, we need to add the current directory in which the new `md5sum` recides to the starting of the `PATH` environment variable so that our version gets called instead of the one used by the machine.

Using `pwd`, I get my current path as `/home/ctf-player`.
So, I add the path using `export PATH="/home/ctf-player:$PATH"`

Running the `flaghasher`, now outputs the content of the flag.txt and we get the flag.

## Flag
picoCTF{sy5teM_b!n@riEs_4r3_5c@red_0f_yoU_cc661106}
