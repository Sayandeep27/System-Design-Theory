# HMAC (Hash-based Message Authentication Code)

---

## 📌 What is HMAC?

**HMAC (Hash-based Message Authentication Code)** is a cryptographic technique used to verify that a message:

* ✅ Comes from a trusted sender
* ✅ Has not been modified during transmission

It combines:

* A **Secret Key** (shared between sender and receiver)
* A **Hashing Algorithm** (like SHA-256)

---

## 🧠 Simple Definition

```
HMAC = Hash(message + secret key)
```

Used to prove **data integrity** and **authenticity**.

---

## ❓ Why HMAC is Needed

HMAC answers two critical security questions:

| Question                                                      | Meaning        |
| ------------------------------------------------------------- | -------------- |
| Was this message created by someone who knows the secret key? | Authentication |
| Was the message modified during transmission?                 | Integrity      |

Without HMAC, attackers could change data while it travels across the internet.

---

## ⚙️ How HMAC Works (Step‑by‑Step)

### Step 1 — Shared Secret

Sender and receiver share a **secret key** beforehand.

```
secret_key = "my_secret"
```

---

### Step 2 — Sender Creates HMAC

The sender generates an HMAC using:

```
HMAC = Hash(message, secret_key)
```

---

### Step 3 — Sender Sends Data

Sender sends:

```
message + HMAC
```

---

### Step 4 — Receiver Recomputes

Receiver recomputes HMAC using the same secret key.

---

### Step 5 — Verification

| Result          | Meaning          |
| --------------- | ---------------- |
| ✔ HMAC matches  | Message trusted  |
| ❌ HMAC mismatch | Message tampered |

---

## 🧪 Simple Example

### Message

```
"userId=101"
```

### Secret Key

```
"my_secret"
```

### Generated HMAC

```
a94f3c2...
```

Receiver recalculates and compares:

* ✔ Match → Valid message
* ❌ No match → Tampered message

---

## 🔐 Important Idea

> **HMAC does NOT encrypt data.**

* Anyone can read the message.
* Only someone with the **secret key** can create a valid HMAC.

HMAC provides:

* Integrity ✅
* Authenticity ✅
* Confidentiality ❌ (No encryption)

---

## 🏗️ Internal Concept (How HMAC Actually Works)

Real HMAC is slightly more secure than `hash(message + key)`.

It uses two hashing rounds:

```
HMAC(K, m) = H((K ⊕ opad) || H((K ⊕ ipad) || m))
```

Where:

* `H` = hash function (SHA‑256, SHA‑1, etc.)
* `K` = secret key
* `m` = message
* `ipad` / `opad` = fixed padding constants

This design prevents length‑extension attacks.

---

## 🧰 Where HMAC is Used

| Use Case             | Example                    |
| -------------------- | -------------------------- |
| JWT Signatures       | HS256 algorithm            |
| API Authentication   | Signed API requests        |
| Secure Cookies       | Tamper-proof cookies       |
| Payment Gateways     | Razorpay/Stripe signatures |
| Webhook Verification | GitHub/Stripe webhooks     |

---

## 🌍 Where HMAC Fits in Real Life (JWT, OAuth, APIs)

Many developers learn **JWT** and **OAuth** but don’t know where HMAC actually fits.

### Big Picture

```
User → App → Server → External Service
            ↑
          HMAC used here
```

HMAC is usually the **signature layer** inside authentication systems.

---

### 1️⃣ JWT (JSON Web Tokens)

JWT has three parts:

```
Header.Payload.Signature
```

When using **HS256**, the signature is created using HMAC:

```
signature = HMAC_SHA256(header + payload, secret_key)
```

Purpose:

* Prevent token tampering
* Verify token authenticity

So:

👉 **JWT uses HMAC internally for signing.**

---

### 2️⃣ OAuth 2.0

OAuth handles **authorization flow**.

But OAuth still needs secure communication between services.

HMAC is used for:

* Signing requests
* Verifying tokens
* Validating callbacks

Example:

```
Google → Your Server
Webhook + HMAC signature
```

Your server verifies the HMAC before trusting the request.

👉 OAuth = Permission system
👉 HMAC = Message verification tool

---

### 3️⃣ API Authentication (Real Industry Use)

Example: AWS API Requests

Client sends:

```
GET /orders
Timestamp
API Key
HMAC Signature
```

Server recalculates HMAC.

If valid → request accepted.

This prevents:

* Request tampering
* Replay attacks
* Fake clients

---

### 4️⃣ Payment Gateways

After payment:

```
Payment Gateway → Your Backend
```

They send:

* Payment data
* HMAC signature

Your backend verifies signature before confirming payment.

Without HMAC → attackers could fake payments.

---

### 5️⃣ Webhooks (Very Common)

Services like:

* GitHub
* Stripe
* Razorpay

Send events to your server.

Each webhook contains:

```
Payload + HMAC Signature Header
```

Your server verifies HMAC to confirm the sender is genuine.

---

## 🔄 Encryption vs Hash vs HMAC

| Feature         | Encryption | Hash | HMAC |
| --------------- | ---------- | ---- | ---- |
| Uses key        | ✅          | ❌    | ✅    |
| Reversible      | ✅          | ❌    | ❌    |
| Integrity check | ❌          | ✅    | ✅    |
| Authentication  | ❌          | ❌    | ✅    |
| Confidentiality | ✅          | ❌    | ❌    |

---

## 🧩 Common Algorithms Used with HMAC

| Algorithm   | Status        |
| ----------- | ------------- |
| HMAC‑SHA256 | ✅ Recommended |
| HMAC‑SHA512 | ✅ Strong      |
| HMAC‑SHA1   | ⚠️ Legacy     |
| HMAC‑MD5    | ❌ Avoid       |

---

## 🧑‍💻 Example (Python)

```python
import hmac
import hashlib

message = b"userId=101"
secret_key = b"my_secret"

hmac_value = hmac.new(secret_key, message, hashlib.sha256).hexdigest()

print(hmac_value)
```

---

## ✅ One‑Line Summary

> **HMAC = a cryptographic signature created using a secret key to prove data authenticity and integrity.**

---

## 🏁 Final Mental Model

Think of HMAC like a **tamper‑proof seal**:

* Anyone can read the package (message).
* Only the sender with the secret stamp (key) can create the seal.
* If the seal breaks → message cannot be trusted.

---

## 📚 Quick Revision

* HMAC = Hash + Secret Key
* Ensures integrity + authenticity
* Not encryption
* Used inside JWT, OAuth flows, APIs, payments, and webhooks
* Core building block of modern authentication systems

---

**End of Document**
