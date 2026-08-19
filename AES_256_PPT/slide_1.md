# Slide 1 — What is Encryption?

## 🎯 Objective

By the end of this slide, everyone should understand:

* What encryption actually is.
* Why encryption is needed.
* What AES is.
* Why AES became the industry standard.

> **Audience takeaway:** Encryption protects data by making it unreadable to unauthorized users, and AES is the standard algorithm used to achieve this.

---

# 🗣️ How to Present

## Opening

Let's start with a simple question.

Imagine you're using your banking app to transfer **₹10,000** to a friend.

Do you think the amount, your account number, and your personal information travel across the Internet exactly as you typed them?

*(Pause for a second and let the audience think.)*

Hopefully not.

If that happened, anyone who intercepted the network traffic could read everything.

Instead, before the data leaves your phone, it is transformed into an unreadable format.

This process is called **Encryption**.

Only the intended receiver, who has the correct secret key, can convert it back into the original readable data.

That's the primary purpose of encryption.

---

# 📄 Card 1 — Plaintext

Plaintext is simply the **original readable data** before encryption.

Examples include:

* Customer information
* Login credentials
* Banking transactions
* API requests
* JSON payloads
* Documents and files

Anything that humans or applications can directly read is called plaintext.

---

# 🔒 Card 2 — Encryption

Encryption is the mathematical process of converting plaintext into an unreadable format called **ciphertext**.

It uses two things:

* An encryption algorithm
* A secret encryption key

The algorithm follows a series of mathematical transformations so that the output appears completely random.

Without the correct key, recovering the original data should be computationally infeasible.

One important thing to remember:

> Encryption doesn't hide data—it makes the data useless to anyone who doesn't possess the correct key.

---

# 🔐 Card 3 — Ciphertext

Ciphertext is the encrypted output.

To us, it looks like random characters or random bytes.

For example:

```text
SEtERgHb4Bh28VEZBX+neAyDhiD0dZ...
```

Although it appears random, every byte has meaning to the encryption algorithm.

Only someone with the correct secret key can transform it back into the original plaintext.

---

# 🛡️ Card 4 — AES

Now the obvious question becomes:

**Which encryption algorithm should we trust?**

Today, the answer is almost always **AES**.

AES stands for **Advanced Encryption Standard**.

A common misconception is that AES was invented by NIST.

That's not exactly true.

In the late 1990s, NIST wanted to replace the aging DES encryption standard.

Instead of creating a new algorithm themselves, they organized an international competition where cryptographers from around the world submitted proposals.

The winning algorithm was **Rijndael**, designed by **Joan Daemen** and **Vincent Rijmen**.

In 2001, NIST officially selected Rijndael as the **Advanced Encryption Standard (AES)**.

Since then, AES has become the global standard for symmetric encryption.

Today, it is used by:

* Banking systems
* HTTPS websites
* AWS, Azure and Google Cloud
* Government organizations
* Enterprise applications
* Mobile applications
* Financial APIs

Our own application also relies on AES to protect sensitive API data.

---

# 💡 Bottom Card — Real World Usage

Whenever you:

* Open your banking app
* Pay using UPI
* Log in to a website over HTTPS
* Store encrypted information in the cloud

there is a very high chance that AES is protecting your data somewhere in that process.

Millions of transactions every second rely on AES to keep sensitive information confidential.

---

# 👨‍🏫 Technical Note

AES is **not a complete encryption solution** by itself.

AES is a **block cipher**.

To encrypt real-world data securely, AES must operate in a specific **mode of operation**.

Some common modes include:

* ECB: Electronic Codebook 
* CBC: Cipher Block Chaining
* CTR: Counter
* GCM: Galois/Counter Mode

These modes determine:

* How data blocks are processed
* Whether integrity is verified
* Whether tampering can be detected
* Overall security characteristics

This distinction is important because our migration is **not just from AES-128 to AES-256**.

It is also a migration from **CBC mode** to **GCM mode**, which significantly changes the security properties.

We'll explore those differences in the upcoming slides.

---

# ❓ Possible Audience Questions

### Is encryption the same as hashing?

No.

Encryption is reversible if you have the correct key.

Hashing is designed to be one-way and cannot be reversed.

---

### Is AES only used by banks?

No.

AES is used almost everywhere:

* Banking
* Cloud platforms
* VPNs
* HTTPS
* Wi-Fi (WPA2/WPA3)
* Mobile applications
* Enterprise software

---

### Who owns AES?

Nobody owns AES.

It is an open international standard published by NIST and freely available for anyone to implement.

---

# 🎤 Transition to the Next Slide

Now that we know **what encryption is** and **where AES comes from**, the next logical question is:

**What exactly does encryption promise?**

Many people say encryption provides "security," but that's only part of the story.

In the next slide, we'll look at the three actual guarantees encryption can provide—and just as importantly, what it does *not* guarantee.
