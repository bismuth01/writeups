# Bitlocker-1 - PicoCTF 2025 (Forensics)

## Introduction
A bitlocker disk image is given and it is told that the password used by Jack to encrypt the drive is very weak.

## Forensics
Using `bitlocker2john`, we get the USER PASSWORD HASH.
We use the password hash and use `john` to crack it.

Now, we use `dislocker` to mount the drive and decrypt it.
To do so we make one mountable point to mount the drive and another to mount the decrypted drive..

To mount and decrypt I used `mkdir bitlocker_mount && sudo dislocker -V bitlocker-1.dd -u -- bitlocker_mount`
And then to mount the decrypted drive `mkdir bitlocker_decrypt && sudo mount -o loop bitlocker_mount/dislocker-file bitlocker_decrypt`

Going inside the decrypted drive, we get the flag.

To unmount the drives `sudo umount bitlocker_decrypt && sudo umount bitlocker_mount`

## Flag
picoCTF{us3_b3tt3r_p4ssw0rd5_pl5!_3242adb1}
