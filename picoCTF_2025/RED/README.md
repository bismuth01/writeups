# RED - PicoCTF 2025 (Forensics)

## Introduction
A image `red.png` is given.

## Analysis
On checking metadata of the image using `exiftool`, we see a poem.
Taking all captial letters it gives us `CHECKLSB` which suggests LSB stegnography.

## Forensics
To extract the message, `zsteg` can be used.
Or a python script

```
from PIL import Image
import numpy as np

def extract_lsb(image_path):
    img = Image.open(image_path)
    img = img.convert("RGBA")  # Ensure RGB format
    pixels = np.array(img)

    bit_string = ""
    for row in pixels:
        for pixel in row:
            for channel in pixel:  # Iterate through R, G, B
                bit_string += str(channel & 1)  # Extract LSB

    # Convert bits to characters
    bytes_list = [bit_string[i:i+8] for i in range(0, len(bit_string), 8)]
    decoded_text = "".join([chr(int(byte, 2)) for byte in bytes_list if len(byte) == 8])

    # Find picoCTF flag pattern
    start = decoded_text.find("picoCTF{")
    if start != -1:
        end = decoded_text.find("}", start)
        if end != -1:
            return decoded_text[start:end+1]

    return "No flag found"

if __name__ == "__main__":
    image_path = input("Enter the image file path: ")
    flag = extract_lsb(image_path)
    print("Extracted Flag:", flag)
```

The decoded text is base64 encoded. Decoding it gives the flag.

## Flag
picoCTF{r3d_1s_th3_ult1m4t3_cur3_f0r_54dn355_}
