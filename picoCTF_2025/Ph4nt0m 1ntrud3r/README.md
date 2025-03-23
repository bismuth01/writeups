# Ph4nt0m 1ntrud3r - PicoCTF 2025 (Forensics)

## Introduction
We are given information that someone stole sensitive data and to track down the intruder we have a PCAP file of the intrusion.

## Forensics
Upon opening the PCAP file we see a lot of connections. On taking a shallow look at the packets, when we inspect the first TCP payload, we find a base64 string in it which, upon decoding, gives us start of the flag.

Another thing was some of the frames had negative relative time and only a few had positive.
Filtering based on `frame.relative_time >= 0` gave us all the packets we needed to see.

Decoding the base64 strings in the TCP Payloads gives us parts of the strings.

On putting them together we get the whole flag and add `picoCTF` in front of it to bring it in format.

## Flag
picoCTF{1t_w4snt_th4t_34sy_tbh_4r_d4b57909}
