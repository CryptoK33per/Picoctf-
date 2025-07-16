
# Writeup: Guess My Cheese - Part 2

## Category
Cryptography / Hash Cracking / Salted SHA-256

---

## Walkthrough

In this challenge, the service provided an encoded hash when connecting via netcat:

```
nc verbal-sleep.picoctf.net 54640
```

An example hash given:
```
d3125a2a7b0016a6d984c6b104ce4138df2be9ef52875562cc2f2
```

We are provided with a list of possible cheese names in a file called `cheese_list.txt`. The hints provided were:

1. Use rainbow tables.
2. The hashing algorithm is SHA-256.
3. There is a two-nibble salt (1 nibble = 4 bits, so 2 nibbles = 8 bits = 1 byte).

This means each cheese name was salted with one byte either prepended or appended before hashing.

---

## Approach

I wrote a Python script to brute-force the hash by trying all combinations of the cheese name with all possible salt bytes in both prepended and appended positions.

```python
import hashlib

r = "d3125a2a7b0016a6d984c6b104ce4138df2be9ef52875562cc2f2"

file = open("cheese_list.txt", "r").readlines()

for j in file:
    for k in "0123456789abcdef":
        for l in "0123456789abcdef":
            print(j[:-1]+k+l, hashlib.sha256((j[:-1]+k+l).encode('utf-8')).hexdigest())
            print(k+l+j[:-1], hashlib.sha256((k+l+j[:-1]).encode('utf-8')).hexdigest())

for j in file:
    for k in range(256):
        print(j[:-1]+hex(k), hashlib.sha256((j[:-1]+chr(k)).encode('utf-8')).hexdigest())
        print(hex(k)+j[:-1], hashlib.sha256((chr(k)+j[:-1]).encode('utf-8')).hexdigest())
        print(j[:-1]+hex(k), hashlib.sha256((j[:-1]+chr(k)).lower().encode('utf-8')).hexdigest())
        print(hex(k)+j[:-1], hashlib.sha256((chr(k)+j[:-1]).lower().encode('utf-8')).hexdigest())
```

This script generates all possible salted combinations and computes their SHA-256 hash.

Once the hashes were generated, I used grep to search for the target hash:

```
grep d3125a2a7b0016a6d984c6b104ce4138df2be9ef52875562cc2f2 outputfile.txt
```

This helped me find the correct cheese name and its associated salt.

For this specific instance, the correct cheese was:
```
Cheddarx10f
```

I submitted this name and received the flag:

```
picoctf{...}
```

---

## Conclusion

This challenge demonstrates the importance of understanding salted hashing and brute-forcing strategies with known plaintext lists. Using basic scripting and hashing libraries, we can efficiently reverse-engineer salted hashes when the salt space is limited.
