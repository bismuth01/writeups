# Cookie Monster Secret Recipie - PicoCTF 2025 (Web Exploitation)

## Introduction
We are given a website by Cookie Monster to outsmart him and find the hidden recipe.

## Exploiting
On entering the webpage, we see a simple login page. On entering any set of credentials, it gives us an Access Denied and a hint "Have you checked your cookies lately?"

On checking the cookies, there is a cookie named `secret_recipe` whose value seems to be HTTP encoded base64 string.
Replacing the `%3D` with `=`, bringing it back to UTF-8 encoding and then decoding the base64 gives us the flag

## Flag
picoCTF{c00k1e_m0nster_l0ves_c00kies_057BCB51}
