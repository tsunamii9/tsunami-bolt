# Slide 3 — Two Ways to Encrypt Data

## 🎯 Objective

By the end of this slide, everyone should understand that encryption comes in two completely different families—symmetric and asymmetric—that each solves a different problem, that real systems use both together, and that our problem is a symmetric one.

---

# 🗣️ Opening

We just agreed on what encryption is supposed to guarantee.

Now comes the practical question:

> **How do the sender and the receiver agree on a secret in the first place?**

This sounds like a small detail.

It isn't.

It's the question that splits all of encryption into two families.

And once you see the split, almost everything else in this session makes sense.

---

# 🔑 Symmetric Encryption — "One shared secret."

In symmetric encryption, **both sides hold exactly the same key**.

The sender locks the data with that key.

The receiver unlocks it with the same key.

Think of a bank locker where you and your spouse each carry an identical key.

Either of you can open it.

Either of you can lock it.

There is only one key design in existence, and you both have a copy.

**AES lives here.** So does ChaCha20, and almost every algorithm you've heard of that actually encrypts real data.

**Why it matters:** symmetric encryption is extremely fast.

Modern processors have dedicated instructions built into the silicon for AES.

We're talking gigabytes per second on an ordinary server.

That speed is not a nice-to-have.

It's the reason symmetric encryption is what protects essentially all real data in the world.

---

# 🗝️ Asymmetric Encryption — "A pair of different keys."

Asymmetric encryption uses **two mathematically related keys** instead of one.

* A **public key**, which you can hand to anyone
* A **private key**, which never leaves you

Whatever one key locks, only the other can unlock.

The classic way to picture this is a padlock.

Imagine you manufacture open padlocks and give them away freely.

Anybody in the world can take one, put a document in a box, and snap the padlock shut.

Once it clicks, **even the person who locked it cannot reopen it.**

Only you can, because only you hold the key to that padlock.

That's asymmetric encryption.

The public key is the open padlock you hand out.

The private key is the one key that opens them.

**RSA, ECDSA and Diffie-Hellman live here.**

---

# ⚖️ So Why Not Just Use Asymmetric for Everything?

This is the natural question, and it has two answers.

**First: it's slow.**

Asymmetric operations are hundreds to thousands of times slower than symmetric ones.

Encrypting an entire API payload this way would be painfully expensive.

**Second—and this surprises people—it physically can't hold much data.**

RSA can only encrypt a message smaller than its key size.

With a 2048-bit RSA key, that's roughly **245 bytes**.

Not 245 kilobytes. Two hundred and forty-five *bytes*.

You couldn't encrypt this paragraph with it.

So asymmetric encryption was never designed to carry your data.

It was designed to solve one specific, very hard problem:

> **How do two strangers agree on a shared secret over a network that everyone can listen to?**

---

# 🤝 In the Real World, They Work Together

Here's the part that makes everything click.

**HTTPS uses both.**

When your browser connects to a bank's website:

1. It uses **asymmetric** cryptography to verify the server's identity and to agree on a shared secret—without ever sending that secret across the wire.
2. The moment that handshake finishes, it **throws asymmetric away** and switches to **symmetric** encryption for every byte of actual traffic.

And the symmetric algorithm it switches to?

**AES-GCM.**

The exact thing we're implementing.

So every HTTPS connection you've ever made has already been doing this.

Asymmetric to agree.

Symmetric to work.

---

# 🎯 Where Our System Sits

Now let's apply this to us.

Our client and our server both trust the same vault.

Both of them can be given the same secret, safely, before any request is ever made.

So we never have the "two strangers on the internet" problem that asymmetric encryption exists to solve.

We have the simpler version:

> **Two parties who already trust a common source and need to exchange data quickly.**

That is a **symmetric** problem.

And the standard answer to a symmetric problem is **AES-256-GCM**.

This isn't a compromise or a shortcut.

It's the correct tool for the shape of the problem we actually have.

---

# ⚠️ The Hard Part of Symmetric Encryption

Symmetric encryption has one genuine weakness, and it's worth naming clearly.

**Getting the key to both sides safely.**

Asymmetric encryption solves this elegantly for strangers.

Symmetric encryption doesn't—it just assumes the key is already there.

So the security of our entire design collapses down to a single question:

> **How well do we protect that key?**

Our answer is the vault.

The key lives in a secret manager with access control and audit logging.

It is fetched at service startup.

It is never in the source code.

It is never in a config file that reaches Git.

It is never printed to a log.

We'll come back to this in detail later, because how you store this key matters more than which algorithm you picked.

---

# ❓ A Question You'll Probably Get Right Here

> "We already have HTTPS. Isn't the data encrypted already? Why are we encrypting it again?"

This is a fair question and worth answering properly.

**HTTPS protects the connection. We're protecting the payload.**

TLS encryption stops the moment the connection terminates—at the load balancer, at the API gateway, at the service mesh.

After that point, your data is plain text again:

* sitting in application memory
* written into logs
* passing through message queues
* captured in traces and monitoring tools
* stored in a database

Payload-level encryption means the sensitive fields stay protected across **all** of that.

It means a misconfigured logging pipeline is no longer a data breach.

We are not replacing HTTPS.

We are adding a second, independent layer underneath it.

---

# 🌍 Real-world Example

Think about the UPI payment we discussed on the previous slide.

**Asymmetric encryption** does its job at the very beginning—when your banking app establishes a secure connection with the bank's servers and confirms it's talking to the real bank and not an impostor.

That handshake takes a few milliseconds and happens once.

**Symmetric encryption** does everything after that—your account details, the transaction payload, the response, and every subsequent request in that session.

That's thousands of operations, and they need to be fast.

Two different tools.

Two different jobs.

Both necessary.

---

# 💡 Key Message

**Asymmetric encryption solves the problem of agreeing on a secret.**

**Symmetric encryption solves the problem of protecting data.**

We already have a trusted way to share our secret—the vault.

So our job is purely the second one, and AES is the right tool for it.

---

# 🎤 Transition

So we've established that we're in the symmetric world, and that AES is the algorithm we'll be using.

But AES by itself has a curious limitation.

It doesn't actually know how to encrypt a message.

It only knows how to encrypt exactly **16 bytes** at a time.

That sounds like a serious problem—and understanding how we work around it is the key to understanding the entire difference between the old CBC approach and the new GCM approach.

So let's look at what AES actually is, and what people mean when they call it a **block cipher**.