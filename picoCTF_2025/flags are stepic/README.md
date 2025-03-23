# flags are stepic - PicoCTF 2025 (Forensics)

## Introduction
On launching instance, we are given a website. The questions says hackers might be trying to communicate using the website.

## Analysis
The website contains multiple flags and the places they belong to. At first nothing seems off, so I researched the Javascript code, which also showed no signs of any odd information.

On scrolling through the website, we see a flag of `Upanzi, Republic The` The which doesn't seem to be something a flag would look like.
Searching about it, we find it's a Network company and not a real place and download the image.

## Forensics
Using `exiftool` to check the metadata of the image downloaded `upz.png` yields nothing.
Taking a closer look at the name of the challenge, it is found that `stepic` is a python package used for image stegnography.

Downloading the package, and using it on the image, we get the flag.

## Flag
picoCTF{fl4g_h45_fl4ga664459a}
