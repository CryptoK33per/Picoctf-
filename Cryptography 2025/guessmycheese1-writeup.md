
# Writeup: Guess My Cheese - Part 1

## 📂 Category
Cryptography / Affine Cipher

---

## 🧪 Walkthrough

This challenge throws you into a **cheesy** cryptographic mystery using netcat:

```
nc verbal-sleep.picoctf.net 58847
```

Once connected (via the PicoCTF webshell or your terminal), you're presented with a choice:
- Do you want to guess a cheese?
- Or encrypt one?

Let’s be honest — I’m awful at guessing cheese names, so I went with encryption.

I chose a classic cheese: `CHEDDAR`.

After submitting it, the program spit back the encrypted string:  
```
WRUVVYH
```

Now the real puzzle begins.

---

## 🔐 Understanding the Cipher

The challenge hinted that the cipher involves something like a **linear transformation** — yep, that’s a dead giveaway for an **Affine Cipher**.

---

### What is an Affine Cipher?

The Affine Cipher is a **monoalphabetic substitution cipher**. It transforms each letter with a mathematical formula.

Each letter is first converted to a number using its position in the alphabet:
```
A = 0, B = 1, ..., Z = 25
```

Then the encryption formula is:
```
E(x) = (A * x + B) mod 26
```
Where:
- `x` is the numeric value of the letter,
- `A` and `B` are keys,
- `mod 26` wraps it back into the alphabet.

To decrypt:
```
D(x) = A_inv * (x - B) mod 26
```
Where `A_inv` is the **modular inverse** of A under mod 26. For decryption to work, `A` must be coprime with 26 (i.e., gcd(A, 26) = 1).

---

## 🎯 Cracking It

Now that we know the encrypted version of `CHEDDAR` is `WRUVVYH`, I plugged that into the Affine Cipher tool at [dcode.fr/affine-cipher](https://www.dcode.fr/affine-cipher).

By using the plaintext and ciphertext, the tool derived:
```
A = 25
B = 24
```

These values are the **secret sauce**. Now we apply these to decrypt the mystery ciphertext given by the challenge:

```
Ciphertext: HKMYLKUQW
```

Using the same A and B in the decryption formula or tool, the plaintext cheese name pops out. And with that, you get the **flag**.

---

# 🧠 Why A and B Matter

- `A` determines how you “stretch” and “shift” the alphabet.
- `B` determines how much the result is shifted.
- Without the correct values, the decryption is just gibberish.
- Affine ciphers are easily breakable with known plaintext — just like we did here.

---

## 🏁 Flag
 Decrypted the ciphertext using Affine Cipher with `A = 25` and `B = 24`
 picoctf{...}

---

*Writeup by: Crypto_keeper*
