This turns the gaps into risks a manager cares about, worst first.

1. QUANTUM EXPOSURE (critical). RSA-2048 wraps our AES keys, RSA is what 
quantum breaks, and harvest-now-decrypt-later puts our long-lived 
financial data on a countdown. Fix: hybrid Kyber key exchange at TLS — 
X25519 for today, Kyber-768 for quantum. Honest caveat: that's the TLS 
workstream, not our AES code, so name who owns it. Until it lands, treat 
long-retention data as exposed.

2. SILENT TAMPERING (high). CBC turns a tampered message into corrupted 
data with no alarm — that's why a token got bolted on. Fix: AES-256-GCM. 
The tag is checked before any plaintext is returned, so tampering fails 
loudly, in one pass. This is our team's core deliverable.

3. AUDIT FAILURE (high). Not just technical — on paper we fail the named 
requirements, and regulators read the details. "We use AES-256" doesn't 
pass when the directive says GCM, SHA-384 and Kyber specifically. Fix: 
match the approved list exactly, and back it with a CBOM inventory plus 
cross-stack test vectors so we can prove it, not just claim it.

Close: none of these are mysteries. Every red box has a green fix already 
written in the directive. What's left is doing it, and proving it.