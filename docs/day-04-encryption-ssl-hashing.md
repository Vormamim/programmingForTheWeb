# Day 4 — Encryption, SSL Certificates and Hashing

**Syllabus:** Explain the processes for securing the web — SSL certificates, encryption algorithms, encryption keys, plaintext and ciphertext, authentication, authorisation, hash values, digital signatures
**Outcomes:** SE-12-03, SE-12-06
**Textbook:** Section 11.3

---

## Key Terms

**Plaintext** — Data in its original, readable form before any encryption is applied. Example: the word `password`.

**Ciphertext** — Data that has been encrypted and is unreadable without the appropriate key. Example: `aX7#mQ2!` (a scrambled version of `password`).

**Encryption** — The process of converting plaintext into ciphertext using an algorithm and a key, so that only authorised parties can read the data.

**Decryption** — The reverse process — converting ciphertext back to plaintext using the appropriate key.

**Encryption algorithm** — The mathematical procedure used to encrypt and decrypt data. Examples: AES (Advanced Encryption Standard), RSA.

**Encryption key** — A piece of data (a string of bits) used by the algorithm to encrypt or decrypt data. The security of encryption depends on the key remaining secret and being sufficiently long.

**Symmetric encryption** — The same key is used to encrypt and decrypt. Fast, but the key must be shared securely. Example: AES.

**Asymmetric encryption** — Uses a pair of mathematically related keys: a **public key** (shared openly) and a **private key** (kept secret). Data encrypted with the public key can only be decrypted with the private key. Example: RSA.

**SSL (Secure Sockets Layer)** — An older encryption protocol, now deprecated and replaced by TLS.

**TLS (Transport Layer Security)** — The current standard for encrypting data in transit. Used by HTTPS. Provides confidentiality, integrity, and authentication.

**SSL certificate** — A digital document issued by a Certificate Authority (CA) that verifies a website's identity and contains its public key. Required for HTTPS.

**Certificate Authority (CA)** — A trusted organisation that issues SSL/TLS certificates. Examples: Let's Encrypt, DigiCert, Comodo.

**Hash function** — A one-way mathematical function that converts any input into a fixed-length output (the hash value). The same input always produces the same hash, but you cannot reverse a hash to recover the original data.

**Hash value (digest)** — The fixed-length output produced by a hash function. Example: the SHA-256 hash of `hello` is always `2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043362938b9824`.

**Digital signature** — A cryptographic mechanism that proves a message or document was created by a specific sender and has not been altered. Uses asymmetric encryption.

---

## Theory

### Symmetric vs Asymmetric Encryption

**Symmetric encryption** uses one shared key:

```
Sender:    plaintext + key → [AES algorithm] → ciphertext
Receiver:  ciphertext + key → [AES algorithm] → plaintext
```

Problem: how do you securely share the key in the first place?

**Asymmetric encryption** solves this with a key pair:

```
Key pair:  Public key (share freely) + Private key (never share)

Encryption:    plaintext + public key → ciphertext
Decryption:    ciphertext + private key → plaintext
```

In practice, HTTPS uses asymmetric encryption to securely exchange a symmetric key, then uses the faster symmetric encryption for the rest of the session.

### How SSL/TLS Works

When you visit an HTTPS site, the following occurs:

1. **Browser requests** the server's SSL certificate
2. **Browser verifies** the certificate is valid and issued by a trusted CA
3. **Key exchange** — browser and server use asymmetric encryption to agree on a symmetric session key
4. **Encrypted session** begins — all subsequent data is encrypted with the symmetric session key

This is called the **TLS handshake**. It takes milliseconds.

### SSL Certificates

An SSL certificate contains:
- The domain name it is issued for (e.g. `bank.com`)
- The organisation's name
- The certificate's expiry date
- The server's public key
- The digital signature of the CA

When your browser sees a padlock, it means:
1. The site has a valid certificate
2. The certificate was issued by a trusted CA
3. You are actually connected to the real server, not an impostor

### Hash Functions

Hash functions are **one-way** — you can hash data but cannot reverse the hash to get the original.

```python
import hashlib

data = "password123"
hash_value = hashlib.sha256(data.encode()).hexdigest()
print(hash_value)
# Output: ef92b778bafe771e89245b89ecbc08a44a4e166c06659911881f383d4473e94f
```

**Properties of a good hash function:**
- **Deterministic** — same input always produces same output
- **Fast to compute**
- **One-way** — cannot reverse the hash to get the input
- **Avalanche effect** — tiny change in input produces completely different hash
- **Collision resistant** — two different inputs should not produce the same hash

**Common hash algorithms:**

| Algorithm | Output length | Status |
|---|---|---|
| MD5 | 128 bits | Broken — not secure |
| SHA-1 | 160 bits | Deprecated |
| SHA-256 | 256 bits | Current standard |
| SHA-3 | Variable | Modern alternative |

### How Hashing is Used in Practice

**Password storage:** websites never store passwords in plaintext. They store the hash:

```
User sets password:    password123
Stored in database:    ef92b778...  (hash)

User logs in:
  entered password → hash → compare to stored hash
  If hashes match → authenticated
```

**File integrity:** download sites publish the hash of a file. After downloading, you hash the file yourself and compare — if they match, the file was not tampered with.

### Digital Signatures

A digital signature proves two things:
1. **Authenticity** — the message came from the claimed sender
2. **Integrity** — the message was not altered in transit

**How it works:**
1. Sender hashes the message → produces a hash value
2. Sender encrypts the hash with their **private key** → this is the digital signature
3. Sender sends the message + signature
4. Receiver decrypts the signature with the sender's **public key** → recovers the hash
5. Receiver hashes the received message independently
6. If the two hashes match → the message is authentic and unaltered

---

## Objective Response Questions

**1.** A website stores user passwords as SHA-256 hash values rather than plaintext. Which of the following best explains why this is more secure?

- A) SHA-256 hashes are smaller, saving database space
- B) Hash values cannot be reversed to recover the original password
- C) SHA-256 encrypts passwords so they can be decrypted by the server when needed
- D) Hashing passwords makes them easier to compare

**Answer: B** — Hashing is one-way. Even if the database is compromised, attackers cannot recover the original passwords.

---

**2.** During a TLS handshake, asymmetric encryption is used to:

- A) Encrypt all data sent during the browsing session
- B) Store the user's password securely on the server
- C) Securely exchange a symmetric session key between client and server
- D) Replace the need for a Certificate Authority

**Answer: C**

---

**3.** A developer receives a file and its SHA-256 hash from a software vendor. After downloading, they hash the file themselves and find a different value. What does this indicate?

- A) The file is too large to hash correctly
- B) The hash algorithm is outdated and unreliable
- C) The file was corrupted or tampered with during transmission
- D) The vendor used the wrong hashing algorithm

**Answer: C** — If the hash does not match, the file content has changed since the hash was generated.

---

## Try It

1. Open a Python interpreter or file and run:
   ```python
   import hashlib
   text = "Hello, world!"
   print(hashlib.sha256(text.encode()).hexdigest())
   ```
   Record the hash output.

2. Change one character in the string (e.g. lowercase the `H`) and run again. Observe how different the output is — this is the avalanche effect.

3. Visit `https://google.com` in your browser. Click the padlock icon in the address bar. Find:
   - Who issued the certificate
   - When it expires
   - What algorithm was used

4. In your notes, explain in plain language how a digital signature proves a message has not been tampered with. Use the steps from the theory section as a guide but write in your own words.
