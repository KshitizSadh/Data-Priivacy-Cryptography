
---

# 📂 **Hashing – A Beginner-Friendly Guide**

---

## ✅ **What is Hashing?**

A **hash function** takes any data (big or small) and turns it into a short, fixed-size value called a **hash**, **digest**, or **fingerprint**.

* It's **one-way** → You can easily create a hash from data but **can't reverse it** to get the original data.
* It's like pressing a large book into a tiny barcode—quick, unique, but not reversible!

---

## ✅ **Why Hashing is Important**

✔ **Data Integrity** – Detect if files or messages have been altered.
✔ **Password Security** – Store password hashes, not actual passwords.
✔ **Digital Signatures** – Hash data before signing for authentication.
✔ **Faster Lookups** – Compare hashes instead of full data.
✔ **Blockchain** – Link blocks using hashes for trust and tamper resistance.

---

## ✅ **How Hashing Works – Simple Flow**

```
Input Data ──► Hash Function ──► Fixed-Size Hash Output
```

Example:
"Hello World" → SHA-256 → `a591a6d40bf420404a011733cfb7b190...`

---

## ✅ **Key Properties of Cryptographic Hashes**

| 🔑 Property                    | 📖 What It Means                                                |
| ------------------------------ | --------------------------------------------------------------- |
| **Deterministic**              | Same input always gives the same hash.                          |
| **Preimage Resistance**        | Can't easily reverse the hash to get the input.                 |
| **Second Preimage Resistance** | Can't easily find another input with the same hash.             |
| **Collision Resistance**       | Hard to find two different inputs with the same hash.           |
| **Avalanche Effect**           | A tiny change → drastically different hash.                     |
| **Efficiency**                 | Fast even for large files.                                      |
| **Fixed Output Size**          | Output is always the same size, no matter how big the input is. |

---

## ✅ **Types of Hash Functions**

### 🔹 Non-Cryptographic (For speed, not security)

* **MurmurHash**, **FNV**, **CityHash** → Used in databases, caching, etc.

### 🔹 Cryptographic (For security purposes)

* ✅ **SHA-256**, **SHA-512** – Widely used, secure.
* ⚠ **MD5**, **SHA-1** – Outdated and vulnerable; avoid for security.
* ✅ **SHA-3**, **BLAKE2**, **BLAKE3** – Modern, efficient, and secure.

---

## ✅ **How Are Hash Functions Built?**

| 🏗 Construction Style | 🔑 Example        | 📖 How it Works                                           |
| --------------------- | ----------------- | --------------------------------------------------------- |
| **Merkle–Damgård**    | MD5, SHA-1, SHA-2 | Processes input in chunks using a compression function.   |
| **Sponge**            | SHA-3 / Keccak    | Absorbs input, then squeezes out hash bits.               |
| **Tree-based**        | BLAKE3            | Splits data, processes in parallel, and combines results. |

---

## ✅ **Hashing-Based Cryptographic Tools**

| 🔐 Tool                | 📖 What It Does                                          |
| ---------------------- | -------------------------------------------------------- |
| **HMAC**               | Uses a secret key + hash to check message authenticity.  |
| **HKDF**               | Derives secure keys from a shared secret.                |
| **Digital Signatures** | Hashes data before signing for verification.             |
| **Commitment Schemes** | Commit to a value while keeping it hidden using hashing. |

---

## ✅ **Password Hashing – Do It Right!**

Simple hashes like SHA-256 are **not enough** for storing passwords securely.

Use **slow and memory-intensive algorithms** designed to resist brute-force attacks:
✔ **bcrypt**
✔ **scrypt**
✔ **Argon2** (recommended by security experts)

---

## ✅ **Salt & Pepper – Extra Protection**

### 🧂 **Salt**

A random value added to a password before hashing.

* Prevents attackers from using precomputed hash tables (rainbow tables).
* Stored with the hash in the database.
* Ensures even identical passwords produce unique hashes.

**Example:**
`Hash = SHA256(Password + Salt)`

---

### 🌶 **Pepper**

A secret value added alongside the salt to further protect passwords.

* Keeps attackers from breaking the hash even if the database is leaked.
* Stored separately from the hash.
* Adds another layer of defense.

**Example:**
`Hash = SHA256(Password + Salt + Pepper)`

---

### 📊 Salt vs Pepper

| Feature    | Salt                                   | Pepper                                 |
| ---------- | -------------------------------------- | -------------------------------------- |
| Purpose    | Protect against precomputation attacks | Adds an extra hidden layer of security |
| Storage    | Stored with hash                       | Stored separately (e.g., config files) |
| Uniqueness | Different for every user               | Usually global or per system           |
| Visibility | Public                                 | Secret                                 |

---

## ✅ **Real-Life Uses of Hashing**

| 📂 Area                 | 🔑 Use Case                                                |
| ----------------------- | ---------------------------------------------------------- |
| **File Integrity**      | SHA-256 checksums verify files haven't been tampered with. |
| **Digital Signatures**  | Hash first, sign later for trust.                          |
| **Blockchain / Crypto** | Blocks linked with SHA-256 hashes (Bitcoin).               |
| **Certificates / PKI**  | Hashes public keys to build trust infrastructure.          |
| **Authentication**      | HMAC used in secure protocols like TLS or SSH.             |

---

## ✅ **Important Security Notes**

✔ **Avoid outdated algorithms** like MD5 and SHA-1.
✔ Always use a **salt** with password hashing.
✔ For authentication, prefer **keyed hashing (HMAC)**.
✔ Pick algorithms that have been tested and reviewed (SHA-2, SHA-3, BLAKE2).

---

## ✅ **Integrity vs Confidentiality**

| 🔑 Property         | 📖 What It Means                              |
| ------------------- | --------------------------------------------- |
| **Integrity**       | The hash confirms the message wasn't changed. |
| **Confidentiality** | Encryption hides the message’s contents.      |

**Hashing alone → checks integrity.**
**Encryption alone → hides content.**
**Together → protects both integrity and confidentiality.**

---

### Example – Integrity Flow

1. Sender hashes the message.
2. Sender sends the message and hash.
3. Receiver re-hashes the message and compares with the received hash.
4. If hashes match → message is unchanged!

---

## ✅ Avalanche Effect Example

**Input 1:** `"Hello"`
→ SHA-256:
`185f8db32271fe25f561a6fc938b2e264306ec304eda518007d1764826381969`

**Input 2:** `"hello"` (only one letter changed!)
→ SHA-256:
`2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043362938b9824`

➡ A tiny change leads to a completely different hash!

---

## ✅ Visual Summary

### Hashing Flow

```
Data + (Salt + Pepper) → Hash Function → Hash Output
```

### Integrity Check

```
Sender → Message + Hash → Receiver → Recalculate & Compare
```

### Avalanche Effect

Even one letter’s change produces a drastically different hash.


