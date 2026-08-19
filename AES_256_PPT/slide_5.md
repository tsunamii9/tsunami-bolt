# Slide 5 — The Cast of Characters

## 🎯 Objective

By the end of this slide, everyone should be able to name the six pieces that make up an encrypted payload, say which ones are secret and which ones aren't, and know exactly how to generate each one correctly in their own stack.

> ⏱️ **Target: 5–6 minutes.** This is the most important slide in Part 1. If the vocabulary lands here, everything after it is just detail.

---

# 🗣️ Opening

For the rest of this session you're going to hear six words over and over.

Salt. Nonce. Tag. Key material. Derived key. Ciphertext.

If any of those are fuzzy, the second half of this session will feel like noise.

So let's meet all six now.

And here's the thing that surprises most people:

> **Four of these six are not secret at all.**

They travel in plain sight, right next to your encrypted data, completely readable by anyone.

That's not a mistake or a shortcut.

It's deliberate—and by the end of this slide you'll understand why.

---

# 🔐 Key Material — *the master secret*

This is the one true secret in the entire system.

It lives in a secret manager—Azure Key Vault, AWS Secrets Manager, HashiCorp Vault.

The service fetches it once at startup and holds it in memory.

**Important:** we never use it directly to encrypt anything.

It's the *input* to a key derivation step, never the encryption key itself.

Think of it as the master key to a building, locked in the facilities office.

Nobody carries it around. It's used to cut other keys.

**Size:** 48 random bytes in our implementation.

**Secret?** ✅ Yes. This is the only thing on this slide that matters if it leaks.

**If it's wrong:** nothing decrypts, anywhere.

**If it leaks:** everything ever encrypted with it becomes readable. Full rotation required.

---

# 🧂 Salt — *fresh randomness, every single message*

A salt is 16 random bytes, generated **new for every message**.

It gets mixed with the key material to produce this specific message's own encryption key.

Same master secret + different salt = completely different key.

**This is the part people find strange:**

The salt is sent along with the encrypted data, in the clear.

Anyone can read it.

> **"Doesn't that defeat the purpose?"**

No—and this is worth saying carefully.

The receiver has no way to rebuild the key without knowing which salt was used.

The salt isn't protecting anything.

Its job is to guarantee that no two messages ever share the same encryption key.

An attacker learning the salt gains nothing, because they still don't have the master secret.

**Size:** 16 bytes

**Secret?** ❌ No — travels with the message

**If it repeats:** messages start sharing derived keys, which quietly erodes our safety margin.

---

# 🎲 Nonce (also called IV) — *number used once*

12 random bytes, also generated fresh for every message.

Its job: make sure that encrypting the **same data twice never produces the same output**.

Without it, an attacker watching your traffic could spot repeated requests—login attempts, the same balance check, the same payload shape—without decrypting anything.

Also sent in the clear. Also not a secret.

**⚠️ The one rule that actually matters:**

> **A nonce must never repeat under the same key.**

Not "should rarely." Never.

We'll cover exactly why this is fatal later in the session, but for now: if you ever find yourself writing code that reuses a nonce, or generating one from a timestamp or a counter, stop and ask someone.

**Size:** 12 bytes (96 bits — this is the size AES-GCM is built around)

**Secret?** ❌ No

**If it repeats under one key:** catastrophic, and unrecoverable.

---

# 🔑 Derived Key — *the actual encryption key*

This is the real AES-256 key: 32 bytes.

It's calculated from `key material + salt` on **both sides independently**.

Which means something worth pausing on:

> **The encryption key is never transmitted. Ever.**

It isn't stored either. It exists in memory for the duration of one operation and then it's gone.

The sender computes it. The receiver computes the same one. Neither sends it.

**Size:** 32 bytes (256 bits)

**Secret?** ✅ Yes — but it never leaves memory, so there's nothing to protect in transit.

---

# 🏷️ Tag — *the tamper-evident seal*

16 bytes produced automatically by AES-GCM during encryption.

It's a cryptographic seal covering the entire message.

On the receiving side, the tag is verified **before a single byte of plaintext is released**.

That ordering is the whole point.

If even one bit of the payload was altered in transit, verification fails, and the decrypt call returns an error—not corrupted data, not partial data. Nothing.

This is the guarantee that our old CBC setup simply did not have.

**Size:** 16 bytes

**Secret?** ❌ No

**If it doesn't match:** hard failure, no output.

---

# 📦 Ciphertext — *your data, scrambled*

The actual encrypted payload.

Two things worth knowing:

**It's exactly the same length as your original data.**

Encryption doesn't compress. A 500-byte JSON body produces 500 bytes of ciphertext.

**It's the only part of the message that needs protecting.**

Everything else on this slide can be published on a billboard.

**Secret?** ✅ Yes — this is what we're actually protecting.

---

# 🧾 Quick Reference

| Piece | Size | Secret? | Generated by | When |
|---|---|---|---|---|
| Key material | 48 B | ✅ **Yes** | Ops / security team | Once, then rotated on schedule |
| Salt | 16 B | ❌ No | The library | Every message |
| Nonce / IV | 12 B | ❌ No | The library | Every message |
| Derived key | 32 B | ✅ Yes (in memory only) | HKDF | Every message, both sides |
| Tag | 16 B | ❌ No | AES-GCM | Every message |
| Ciphertext | = plaintext | ✅ Yes | AES-GCM | Every message |

**Say this line out loud:**

> Only two things in this table are genuinely secret: the master key material, and your actual data.
>
> Everything else is public plumbing.

---

# 🛠️ How We Actually Generate These

We're running Angular, Go, .NET and Node. The rules are identical everywhere—only the function names change.

## Generating key material (ops task, done once)

The canonical command:

```bash
openssl rand -base64 48
```

That produces 48 random bytes, base64-encoded into a 64-character string. That's the value that goes into the vault.

Alternatives if OpenSSL isn't available:

```bash
head -c 48 /dev/urandom | base64        # Linux / macOS
openssl rand -hex 32                     # hex output instead of base64
```

**Verify what you generated is actually the right length:**

```bash
echo -n 'PASTE_YOUR_BASE64_HERE' | base64 -d | wc -c
# must print: 48
```

If that number is wrong, something mangled your string—usually a copy-paste that dropped padding.

## The same thing, in each of our stacks

**Go**

```go
import (
    "crypto/rand"          // NOT math/rand
    "encoding/base64"
)

b := make([]byte, 48)
if _, err := rand.Read(b); err != nil {
    panic(err)
}
fmt.Println(base64.StdEncoding.EncodeToString(b))
```

**Node.js**

```js
const crypto = require('crypto')
console.log(crypto.randomBytes(48).toString('base64'))
```

**.NET**

```csharp
using System.Security.Cryptography;

var bytes = RandomNumberGenerator.GetBytes(48);
Console.WriteLine(Convert.ToBase64String(bytes));
```

> Note: `RNGCryptoServiceProvider` is obsolete from .NET 6 onward. Use `RandomNumberGenerator`.

**Angular / browser (Web Crypto)**

```typescript
const bytes = new Uint8Array(48);
crypto.getRandomValues(bytes);
const b64 = btoa(String.fromCharCode(...bytes));
```

## Generating salt and nonce

**You almost certainly should not be writing this code.**

The library generates the salt and nonce internally on every encrypt call. That's deliberate—it's the single easiest place to introduce a fatal bug.

But so you can recognise correct code in a review:

| Stack | Salt (16 bytes) | Nonce (12 bytes) |
|---|---|---|
| Go | `rand.Read(salt)` from `crypto/rand` | `rand.Read(nonce)` |
| Node | `crypto.randomBytes(16)` | `crypto.randomBytes(12)` |
| .NET | `RandomNumberGenerator.GetBytes(16)` | `RandomNumberGenerator.GetBytes(12)` |
| Browser | `crypto.getRandomValues(new Uint8Array(16))` | `crypto.getRandomValues(new Uint8Array(12))` |

## ❌ Never use these to generate anything cryptographic

| Stack | Do not use | Why |
|---|---|---|
| Go | `math/rand` | Predictable. Seeded, reproducible output. |
| Node | `Math.random()` | Not a cryptographic RNG. Never was. |
| .NET | `System.Random` | Same problem. Also `Guid.NewGuid()` — not random enough. |
| Angular | `Math.random()` | Same. Use `crypto.getRandomValues`. |
| Any | Timestamps, counters, request IDs, UUIDs | Predictable by definition. Fatal as a nonce. |

**If you take one rule away from this section:**

> If the function name doesn't have the word **crypto** or **secure** in it, it does not belong anywhere near a key, a salt, or a nonce.

---

# ⚠️ A Note on Angular Specifically

This needs saying clearly, because it's the one place this design can go wrong quietly.

**Key material must never be shipped inside an Angular bundle.**

Anything in the browser is readable by the user. Environment files, minified builds, obfuscated constants—all of it is one DevTools window away.

If a browser client needs to encrypt payloads, the key must be **issued per session by the server after authentication**, held in memory only, and never written to `localStorage` or `sessionStorage`.

If that's not the design we have today, it needs to be a conversation—and it's on the open decisions slide at the end.

Backend-to-backend (Go, .NET, Node) is the straightforward case: the key comes from the vault and never leaves the server.

---

# 🌍 Real-world Example — Sending a Sealed Document Through Office Courier

Picture the internal courier envelope we all use.

* **Key material** — the master key in the facilities office. Nobody carries it.
* **Salt** — the reference number stamped on this one envelope. Printed on the outside. Anyone can read it.
* **Derived key** — the key cut specifically for this envelope, using the master key plus that reference number.
* **Nonce** — the unique serial on the tamper strip. Also printed on the outside.
* **Ciphertext** — the sealed document inside.
* **Tag** — the tamper-evident seal across the flap.

Now look at what's visible on the outside of that envelope: a reference number and a serial number.

Reading them tells you nothing.

You still can't open the envelope, because you don't have the master key in the facilities office.

But the recipient—who *does* have their own copy of that master key—needs those numbers to cut the matching key.

That's why salt and nonce travel in plain sight.

**And the seal?** If it's broken when it arrives, the recipient doesn't peek inside to see if the contents look okay.

They reject the envelope entirely.

That's the tag.

---

# 💡 Key Message

> **One secret. Two secrets, technically: the master key and your data.**
>
> **Everything else is public plumbing that makes the maths work.**

And on generation:

> **Never invent randomness yourself. Every one of our four stacks has exactly one correct function for this—use it.**

---

# 🎤 Transition

We now know the six pieces and where each one comes from.

The obvious next question:

> **How do all six of these travel together in a single API call?**

Because they don't arrive separately—they're packed into one string, in a fixed order, that every one of our four stacks has to agree on byte for byte.

Let's look at exactly what goes on the wire.