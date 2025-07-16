
# Writeup: EVEN RSA CAN BE BROKEN???

## Category
Cryptography / RSA

---

## Walkthrough

The challenge starts by providing a command to connect to a remote server:

```
nc verbal-sleep.picoctf.net 51624
```

Upon connection, the following RSA parameters are provided:

```
N: 16362093939527432832710193884521092762365996876089918606081406228645564481160937444533014485672875754874725021974465277415234738040199326861909961647753874
e: 65537
cyphertext: 13536020740210069348685947924163085214606309873821087154448001531231420832076355332112069019138043680608414757654892728158244007059228352910021548672055741
```

We are also given the source code. On inspecting the function responsible for key generation:

```python
def gen_key(k):
    """
    Generates RSA key with k bits
    """
    p, q = get_primes(k//2)
    N = p * q
    d = inverse(e, (p-1)*(q-1))
    return ((N, e), d)
```

The function confirms that it's generating an RSA key with modulus `N`, public exponent `e`, and the ciphertext is produced using these.

To decrypt the ciphertext, since `N` and `e` are given, and because the primes `p` and `q` might be small enough (due to improper generation or small bit length), we can factor `N` and retrieve the private key `d`.

Instead of writing a factoring tool manually, I opted for an online tool that can handle RSA decryption if the modulus `N` is factored easily.

I used the following site:

```
https://www.dcode.fr/rsa-cipher
```

By entering the **ciphertext**, **N**, and **e** into this tool, it successfully decrypted the ciphertext.

### Result
```
picoCTF{...}
```

---

## Conclusion

This challenge demonstrates how **weak key generation in RSA**, such as using small primes, can lead to easy factorization of `N` and hence breaking the RSA encryption. Always use sufficiently large primes for secure RSA.
