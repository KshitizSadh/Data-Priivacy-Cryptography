

# 🔐 WhatsApp Data Encryption Lifecycle (At Rest → In Motion)

---

## **1. Data at Rest (on device before sending)**

* **Plaintext message/media**: Created by user on their device.
* **Local storage** (message DB, media folder):

  * Encrypted with **Full-Disk Encryption (OS-level)** (Android: File-Based Encryption, iOS: Data Protection API).
  * WhatsApp also applies its own **SQLCipher (AES-256)** to its local SQLite database.
  * Media in cache is encrypted at the file level using OS cryptography.

➡️ **Transition trigger:** user presses **Send**.

---

## **2. Session Keys Setup (E2EE basis)**

* **Identity keys, signed prekeys, one-time prekeys** created and stored locally.
* When first communicating with a new contact, WhatsApp derives a **session key** using Signal’s **X3DH handshake** (Curve25519 ECDH).
* These session keys are **stored locally** (still protected at rest with SQLCipher + OS crypto).

➡️ **Transition trigger:** first-time communication or device re-linking.

---

## **3. Data Encryption Before Motion (on sender device)**

* For each outgoing message/media:

  1. **Derive Message Key** from **Double Ratchet** (HMAC-SHA256).
  2. **Encrypt plaintext** with **AES-256 in CBC mode** + **HMAC-SHA256** (Encrypt-then-MAC).
  3. **For media:** generate fresh random media key → encrypt file locally → upload blob → send encrypted pointer + key.
  4. **Fan-out encryption:** encrypt separately for each recipient device in 1:1 or group.

➡️ **Transition trigger:** ciphertext ready → handoff to transport channel.

---

## **4. Data in Motion (client → WhatsApp server → recipient)**

* **Transport channel:**

  * Client maintains a secure **Noise Pipes channel** (Curve25519 + AES-GCM + SHA-256) to WhatsApp servers.
  * This protects metadata-in-transit (though WhatsApp still knows sender/receiver IDs).

* **Server role:**

  * Stores ciphertext temporarily if recipient is offline.
  * Forwards ciphertext only — server cannot decrypt.

➡️ **Transition trigger:** recipient device retrieves ciphertext.

---

## **5. Data Decryption (on recipient device)**

* Recipient device uses its **Double Ratchet session state**:

  * Derives correct Message Key from its Chain Key.
  * Verifies HMAC-SHA256.
  * Decrypts message with AES-256.
* For media:

  * Receives encrypted media key inside message.
  * Downloads encrypted blob from media store.
  * Decrypts locally with AES-256 + HMAC.

➡️ **Transition trigger:** message/media displayed in chat UI.

---

## **6. Data at Rest (after receipt)**

* Once decrypted, message/media lives at rest again on device:

  * Protected by **OS disk/file encryption**.
  * Message DB secured by **SQLCipher AES-256**.
  * Media files encrypted in app storage/cache.

➡️ **Loop closes** — data has gone full cycle: Rest → Motion → Rest.

---

## **7. Backups (optional data at rest, off-device)**

* If enabled:

  * **End-to-End Encrypted Backups** use a backup key/password chosen by the user.
  * Without E2EE backups enabled, backup content is stored in iCloud/Google Drive, but **plaintext to the cloud provider** (only encrypted in transit via TLS).

---

# 🔄 Summary Flow (compressed view)

**At Rest (DB/Media encrypted)** → **Session key derivation** → **Encrypt with Double Ratchet & AES** → **Noise Pipe TLS-like tunnel** → **Server fan-out relay** → **Recipient decrypts with session state** → **Data at Rest (again, DB/Media encrypted)** → **(Optional: backup encryption)**

---


