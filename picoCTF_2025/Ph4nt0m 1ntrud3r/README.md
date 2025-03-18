# Ph4nt0m 1ntrud3r - PicoCTF 2025 (Forensics)

## Introduction

We are given information that someone stole sensitive data and to track down the intruder we have a PCAP file of the intrusion.

## Analysis

Upon opening the PCAP file we see a lot of connections. On taking a shallow look at the packets, when we inspect the first TCP payload, we find a base64 string in it which, upon decoding, gives us start of the flag.

Another thing was some of the frames had negative relative time and only a few had positive.
