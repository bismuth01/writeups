# head-dump - PicoCTF 2025 (Web Exploitation)

## Introduction
On starting instance, we are given a website. The website consists of details about the API being used.

## Exploiting
On clicking `#API docs`, we are taken to the API documentation page where, at the end we see under the diagnosis section to have an endpoint `/heapdump`.

On going to the api endpoint, we get a file named heapdump downloaded. Opening the file, at the end we have the flag.

## Flag
picoCTF{Pat!3nt_15_Th3_K3y_388d10f7}
