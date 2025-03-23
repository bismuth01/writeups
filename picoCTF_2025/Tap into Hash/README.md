# Tap into Hash - PicoCTF 2025 (Reverse Engineering)

## Introduction
A source file and the file encrypted using it is given.

## Analysis
The source file uses the cryptographic methods which are used in the bitcoin blockchain.

The function essential mines transactions and generates blocks. And the final blockchain block names is outputted and then used for encrypting along with a key.

On reading the encrypted file, it gives the final blockchain string, the key and the encrypted text.
So we reverse engineering the encryption part, we can write a python script.

```
import hashlib

def decrypt(ciphertext, key):
    key_hash = hashlib.sha256(key).digest()
    block_size = 16
    plaintext = b''

    for i in range(0, len(ciphertext), block_size):
        cipher_block = ciphertext[i:i + block_size]
        plain_block = xor_bytes(cipher_block, key_hash)
        plaintext += plain_block

    # Remove padding
    padding_length = plaintext[-1]
    plaintext = plaintext[:-padding_length]

    return plaintext.decode()

def xor_bytes(a, b):
    return bytes(x ^ y for x, y in zip(a, b))

# The key from enc_flag file
key = b'B\xfdL\x92\xf1C\x8fP\xb4\xd4dt\x8b\x18\xcfR\xfd`\xd1-\xa5\xde\xcd\x89\xee\xdb\xfb\r\x83&\x07\x82'

# The encrypted blockchain from enc_flag file
encrypted_blockchain = b'|`M\xb2&\xa7\xea\xd5m\xb5\x83g\x11\x90\x08\xdbx7L\xb2v\xa0\xb8\xd4;\xbe\xdd`E\xc4\x01\xdb/:\x18\xe5s\xa6\xee\x8e;\xbd\xd8i\x19\x94X\xda|2\x19\xe2v\xf4\xed\x81m\xbb\xdd1\x13\x98\x08\xd3d3K\xe4t\xf1\xec\x83;\xbe\x83c\x18\x93]\xda|2\x1e\xe7&\xa9\xbd\xd0a\xee\x8a4\x13\xc3Z\xd3x7J\xb5u\xa1\xe9\x81o\xbf\x8ec\x12\x90\x0c\xd3y3N\xe3 \xa6\xbe\x85>\xe9\x82c\x14\x92\x0c\x81\x7f.K\xb6t\xa3\xee\x80o\xb4\x8dcE\x94\x08\x82(3K\xe2r\xa5\xe9\xd0o\xef\x8f1\x10\xc5\x0c\xd2\x7f`\x0b\xefs\xff\xcc\xe2\x1e\xf7\xd9<O\xc2R\xbczP)\xeeF\xf9\xdd\xd4\x0c\xbd\xca3x\xfea\xb6#NK\xf4$\xa9\xec\xfe\x07\xfd\xf8*M\xebc\x99\x0bH$\xbe$\xa2\xec\xd0`\xbf\xda-\x10\x91\\\xd3-eJ\xb6#\xa3\xea\xd2m\xbc\xddfC\xc5\t\x82+fO\xe2(\xa9\xee\x85k\xe8\x822\r\x91\t\x86y1I\xe7!\xf5\xb6\x8e>\xbd\xdae\x14\x98\t\xd6(1M\xbfv\xa9\xe9\x85=\xe8\xdadB\x92\x00\xdb*fB\xb2!\xa5\xeb\x83<\xea\x8d5A\x96\x0c\xd2\x7f2M\xb2(\xa6\xea\xd5l\xea\xdab\x16\x8c\t\xd3z`C\xe3(\xa0\xb7\x81j\xee\xd9hE\x95\x0b\x82*aO\xb2"\xa7\xeb\x86:\xbe\xd9gC\x97\x01\xd4yf\x19\xe0\'\xa3\xb9\x80>\xbb\xdahE\x93\x0f\x82,`\x1d\xb5#\xa7\xb6\x82k\xbc\x8ci\x14\x97;\xe1'

# Decrypt the blockchain
decrypted_text = decrypt(encrypted_blockchain, key)
print("Decrypted text:", decrypted_text)
```

Inputting the variables in the appropriate places, we can decrypt the enc_flag file which gives the flag.

## Flag
picoCTF{block_3SRhViRbT1qcX_XUjM0r49cH_qCzmJZzBK_842cf83a}

## Appendices

block_chain.py
```
import time
import base64
import hashlib
import sys
import secrets


class Block:
    def __init__(self, index, previous_hash, timestamp, encoded_transactions, nonce):
        self.index = index
        self.previous_hash = previous_hash
        self.timestamp = timestamp
        self.encoded_transactions = encoded_transactions
        self.nonce = nonce

    def calculate_hash(self):
        block_string = f"{self.index}{self.previous_hash}{self.timestamp}{self.encoded_transactions}{self.nonce}"
        return hashlib.sha256(block_string.encode()).hexdigest()


def proof_of_work(previous_block, encoded_transactions):
    index = previous_block.index + 1
    timestamp = int(time.time())
    nonce = 0

    block = Block(index, previous_block.calculate_hash(),
                  timestamp, encoded_transactions, nonce)

    while not is_valid_proof(block):
        nonce += 1
        block.nonce = nonce

    return block


def is_valid_proof(block):
    guess_hash = block.calculate_hash()
    return guess_hash[:2] == "00"


def decode_transactions(encoded_transactions):
    return base64.b64decode(encoded_transactions).decode('utf-8')


def get_all_blocks(blockchain):
    return blockchain


def blockchain_to_string(blockchain):
    block_strings = [f"{block.calculate_hash()}" for block in blockchain]
    return '-'.join(block_strings)


def encrypt(plaintext, inner_txt, key):
    midpoint = len(plaintext) // 2

    first_part = plaintext[:midpoint]
    second_part = plaintext[midpoint:]
    modified_plaintext = first_part + inner_txt + second_part
    block_size = 16
    plaintext = pad(modified_plaintext, block_size)
    key_hash = hashlib.sha256(key).digest()

    ciphertext = b''

    for i in range(0, len(plaintext), block_size):
        block = plaintext[i:i + block_size]
        cipher_block = xor_bytes(block, key_hash)
        ciphertext += cipher_block

    return ciphertext


def pad(data, block_size):
    padding_length = block_size - len(data) % block_size
    padding = bytes([padding_length] * padding_length)
    return data.encode() + padding


def xor_bytes(a, b):
    return bytes(x ^ y for x, y in zip(a, b))


def generate_random_string(length):
    return secrets.token_hex(length // 2)


random_string = generate_random_string(64)


def main(token):
    key = bytes.fromhex(random_string)

    print("Key:", key)

    genesis_block = Block(0, "0", int(time.time()), "EncodedGenesisBlock", 0)
    blockchain = [genesis_block]

    for i in range(1, 5):
        encoded_transactions = base64.b64encode(
            f"Transaction_{i}".encode()).decode('utf-8')
        new_block = proof_of_work(blockchain[-1], encoded_transactions)
        blockchain.append(new_block)

    all_blocks = get_all_blocks(blockchain)

    blockchain_string = blockchain_to_string(all_blocks)
    encrypted_blockchain = encrypt(blockchain_string, token, key)

    print("Encrypted Blockchain:", encrypted_blockchain)


if __name__ == "__main__":
    text = sys.argv[1]
    main(text)
```
