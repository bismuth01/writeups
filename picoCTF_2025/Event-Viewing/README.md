# Event-Viewing - PicoCTF 2025 (Forensics)

## Introduction
The story given by the employee is as follows:
- They installed software using an installer they downloaded online
- They ran the installed software but it seemed to do nothing
- Now every time they bootup and login to their computer, a black command prompt screen quickly opens and closes and their computer shuts down instantly.

We are provided the event log of windows to find the evidence.

## Analysis
We open the .evtx file on Windows, using Windows Event Viewer.

Now we look for event codes that could lead us to the logs related to the program.
- We know it shutdown the system
- It was downloaded
- It was given perms to shutdown the system

Using the info we experiment and collect the event codes that could be of use..

## Forensics
Using Event ID 1074 for User initiated Shut Down, and see that one has happened and has a comment in base64 which gives us the last part of the flag.

Since, we know that the program shuts the computer down so we start looking from the end of the log.
Now, we look into the part where the file got downloaded, but it is pretty hard to figure that out since there are many object requests. So we look into the next thing of when files were accessed.

Using Event ID 4663 for Object Accessed, the malicious file should be at the last of the timeline to be accessed before it caused havoc. So we find the last log and see a process named `Totally_Legit_Software`.

Clearing all filters, we can now search the logs for the name of this particular software and doing so we also get the 1st and 2nd parts of the flag.

## Flag
picoCTF{Ev3nt_vi3wv3r_1s_a_pr3tty_us3ful_t00l_81ba3fe9}
