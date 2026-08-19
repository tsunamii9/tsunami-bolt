# Slide 4 — Ciphers Have a Lifecycle. This Is a Scheduled Move.

## 🎯 Objective

By the end of this slide, everyone should understand that we are not reacting to a breach. Cryptography retires on a planned timetable, deliberately, years before anything actually breaks—and that's exactly what's happening here.

> ⏱️ **Target: 2 minutes.** This slide has one job. Make the point, get the laugh, move on.

---

# 🗣️ Opening — Ask the Room First

Before you explain anything, ask this out loud:

> **"Quick show of hands. Who thinks we're doing this because AES-128-CBC got hacked?"**

Wait for the hands. Some will go up. Some will hesitate.

Then give them the answer:

> **"Nobody has ever broken AES. Not once. Not in twenty-five years."**

Let that land for a second.

That's the whole slide.

We are not here because something failed.

We're here because everything in cryptography has an expiry date, and ours came up.

---

# ⏱️ The Story of DES — Interactive

Here's how it usually goes.

In **1977**, the US government standardised an algorithm called **DES**.

It used a 56-bit key.

At the time, that was considered strong enough to last indefinitely.

Now ask the room:

> **"How long do you think it took before someone cracked it in public?"**

Let two or three people guess. You'll usually get "5 years," "10 years."

**The real answer: 21 years.**


In 1998, a purpose-built machine broke a DES key in about two days.
It was called "Deep Crack" (formally the EFF DES Cracker), and it was built by a non-profit digital rights group called the Electronic Frontier Foundation (EFF). 

Then ask the follow-up:

> **"And how long the very next year?"**

**22 hours.**

One year later, the same problem took less than a day.

---

## 🔍 The Important Detail

Nobody found a clever mathematical flaw in DES.

The algorithm worked exactly as designed, right to the end.

**What changed was the hardware.**

The key was simply too short for the computers that existed by then.

That's the pattern to remember:

> Cryptography usually doesn't fail because it's wrong.
>
> It ages out because the world around it gets faster.

---

# 📜 What Happened Next

Once DES aged out, the industry didn't panic—it planned.

* **1997** — NIST opened a public competition. Anyone in the world could submit an algorithm, and everyone was invited to attack all of them.
* **2001** — A design called Rijndael won and became **AES**.
* **2007** — **GCM** was standardised, adding built-in tamper detection to AES.
* **2018** — TLS 1.3 removed CBC modes entirely from the modern internet.
* **Today** — Compliance frameworks are catching up. Which is why we're in this room.

Notice that AES itself is 25 years old and still standing.

**It's not AES we're leaving. It's CBC.**

---

# 🧯 The Analogy — Look Around This Room

Point at the fire extinguisher on the wall.

There's a date printed on it.

When that date arrives, facilities replaces it.

Not because there was a fire.

Not because it failed a test.

Not because anything went wrong at all.

They replace it **because waiting for it to fail is not a strategy.**

That's exactly what we're doing today.

CBC still works.

It hasn't let us down.

And we're replacing it anyway—on schedule, calmly, with a rollback plan—because that's how you're supposed to do this.

---

# 💡 Key Message

> **Nothing was hacked.**
>
> **Nothing is on fire.**
>
> **This is scheduled maintenance, and it's the boring kind on purpose.**

DES got 24 years.

CBC got its run.

AES-256-GCM will have its own expiry date one day too, and some team will sit in this room and replace it.

That's not a weakness in the system.

**That *is* the system.**

---

# 🎤 Transition

So the move is planned, not reactive.

But that raises the obvious question:

> **If AES itself is fine—what exactly is wrong with CBC?**

To answer that, we need to look at what AES actually does under the hood.

Because AES has one strange limitation that explains this entire migration.