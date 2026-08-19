Frame it gently first — nobody built anything negligent. This stack was 
fine by yesterday's standards; the directive raised the bar. Then go row 
by row.

CONFIDENTIALITY — AES-256-CBC. Someone will say "but it IS AES-256." 
They asked for AES-256 AND GCM. The 256 is the key size; CBC vs GCM is 
the mode. CBC is the exact mode we're retiring — no built-in tamper 
detection, which is the only reason the integrity row below even exists. 
HIGH: data's still confidential today, it's just the wrong mode.

KEY PROTECTION — RSA-2048. This is the CRITICAL one. RSA is precisely 
what quantum breaks — completely, with Shor's algorithm. This is the 
"harvest now, decrypt later" algorithm: capture our RSA-wrapped keys 
today, crack them the day quantum arrives. The mandate replaces it with 
Kyber-768. If we only swap CBC to GCM and leave RSA in, we've done the 
visible work and still failed the core quantum requirement.

INTEGRITY — the separate token. It only exists because CBC can't 
authenticate itself. GCM produces the tag in the same pass, so this 
layer largely disappears. MED — not dangerous, just redundant and vague 
to an auditor.

HASH — SHA-256 should be SHA-384. It's right there in the suite name, 
TLS_AES_256_GCM_SHA384. Minor but named. Our new info string already 
uses 384, so the target's right — this old stack just hasn't caught up.

Close: AES-256 alone isn't compliance. And the real question — is 
anyone scheduled to replace RSA with Kyber? If not, raise it today, 
not in an audit.