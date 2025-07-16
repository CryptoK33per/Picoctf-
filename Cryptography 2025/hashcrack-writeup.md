
# Writeup: Hashcrack

## Category
Cryptography / Hash Cracking

---

## Walkthrough

In this challenge, we are given three hashes sequentially and are expected to crack each to proceed to the next one.

### First Hash
```
482c811da5d5b4bc6d497ffa98491e38
```

After researching, I recognized that this is a **32-character hexadecimal string**, typical of **MD5** hashes.

By decoding this MD5 hash (using an online decoder or a cracking tool), I obtained:
```
password123
```

### Second Hash
```
b7a875fc1ea228b9061041b7cec4bd3c52ab3ce3
```

This hash is **40 hexadecimal characters long**, which identifies it as a **SHA-1** hash.

Upon decoding the SHA-1 hash, the result was:
```
letmein
```

### Third Hash
```
916e8c4f79b25028c9e467f1eb8eee6d6bbdff965f9928310ad30a8d88697745
```

This is a **64-character hexadecimal string**, characteristic of **SHA-256** hashes.

After decoding this SHA-256 hash, I finally got the flag:
```
picoctf{...}
```

---

## Summary of Hash Algorithms
- **MD5:** 128 bits → 32 hex characters
- **SHA-1:** 160 bits → 40 hex characters
- **SHA-256:** 256 bits → 64 hex characters

Understanding the length and format of hashes helps in quickly identifying the algorithm used, which is crucial for efficient decoding or cracking.

