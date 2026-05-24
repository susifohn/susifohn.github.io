---
title: DNS and DNSSEC 
categories: [unibe, Vernetzte Systeme und Betriebssysteme]
tags: [dns, network]     # TAG names should always be lowercase
math: true
---

# DNS & DNSsec

## 1. DNS — Domain Name System

### Zweck / Purpose
- Maps **hostnames → IP addresses** (e.g. `obelix.unibe.ch` → `130.92.64.5`)
- **Hierarchical** naming structure with delegated authority per domain level
- Request/Response protocol between client and server (over **UDP**)

### Query Types

| Type | Meaning |
|------|---------|
| **A** | IPv4 address |
| **AAAA** | IPv6 address |
| **MX** | Mail server |
| **NS** | Name server |
| **CNAME** | Alias name |
| **SRV** | Service record |

### Query Modes

| Mode | How it works |
|------|-------------|
| **Recursive** | Client asks DNS server → server does all the work and returns the final answer |
| **Iterative** | Server returns the address of the *next* DNS server to ask → client queries it directly |
> In practice both modes are often **combined**.

### Resource Records (RR)
Stored as tuples: **(Name, Value, Type, Class, TTL)**

Examples:
```
(inf.unibe.ch,  asterix.unibe.ch,  NS, IN)   ← Name Server
(inf.unibe.ch,  obelix.unibe.ch,   MX, IN)   ← Mail Server
(asterix.unibe.ch, 130.92.64.4,    A,  IN)   ← IP Address
```
> ⚠️ Cache entries have a **TTL (Time to Live)** — they expire!

### DNS Hierarchy

```
root
 ├── ch
 │    └── unibe.ch
 │         └── asterix.unibe.ch
 ├── de
 │    └── ibm.de
 └── fr
      └── inria.fr
```

- Each domain has **at least one DNS server**
- The root domain has **multiple servers** (see [root-servers.org](https://www.root-servers.org))
- Each name server knows the names/addresses of the **next lower level** (via NS and A records)

### Query Flow Example (asterix.unibe.ch → www.ibm.de)
1. `asterix` asks its local DNS server `unibe.ch` **(recursive)**
2. `unibe.ch` can't resolve it → returns root DNS IP **(iterative)**
3. Root DNS returns `.de` DNS server address
4. `asterix` asks `.de` → gets `ibm.de` DNS server IP
5. `asterix` asks `ibm.de` → gets final IP ✓

---



## 2. DNSsec — DNS Security (Tanenbaum Ch. 8)

### Why DNSsec?

DNS was designed for a small, trusted research network — **no security built in**. This enables **DNS spoofing**: an attacker poisons a DNS cache to redirect traffic through their own server (man-in-the-middle). DNSsec was created in **1994 by IETF** (RFC 2535) to fix this.

---

### Core Idea

> Every **DNS zone** has a **public/private key pair**. All DNS responses are signed with the zone's **private key** and verified by receivers using the **public key**.

**Note:** DNSsec provides **authenticity only — not secrecy** (DNS data is public).

---

### Three Services

| # | Service | Purpose |
|---|---------|---------|
| 1 | **Proof of origin** | Verifies data came from the legitimate zone owner |
| 2 | **Public key distribution** | Securely stores/retrieves public keys |
| 3 | **Transaction authentication** | Guards against replay & spoofing attacks |

---

### Key Concepts

### RRSets (Resource Record Sets)
- Records with the **same name, class, and type** are grouped into an RRSet
- Each RRSet is **hashed (SHA-1)** then **signed** with the zone's private key
- The signed RRSet is the unit sent to clients
- RRSets can be **safely cached anywhere** (even untrusted servers) — each carries its own signature

### New Record Types

| Record | Contents |
|--------|---------|
| **KEY** | Zone's public key, signing algorithm (e.g. MD5/RSA), protocol |
| **SIG** | Signed hash of the RRSet + validity period + signer name |
| **CERT** | Optional X.509 certificates (for PKI use) |

---

### Verification Flow (Alice → bob.com)

```
1. Alice is pre-configured with top-level domain public keys (.com, .org …)
2. Alice queries DNS for bob.com → receives RRSet:
     A record    →  IP: 36.1.2.3
     KEY record  →  Bob's public key
     SIG record  →  Signed by the .com server
3. Alice verifies SIG using the .com public key  ✓
4. Now Alice trusts Bob's public key
5. Alice queries Bob's DNS for www.bob.com → RRSet signed by Bob's private key
6. Alice verifies using Bob's public key  ✓
```

If Trudy injects a false RRSet → **SIG won't match → immediately detected**.

---

### Anti-Spoofing

To defeat sequence-number spoofing, DNSsec optionally adds to every response:

> **Hash of the original query, signed with the respondent's private key**

Trudy cannot forge this without the .com server's **private key** — her reply is rejected even if it arrives first.

---

### Offline Private Keys

| Step | Action |
|------|--------|
| 1 | Transport zone data to a **disconnected machine** (e.g. via CD-ROM) |
| 2 | Sign all RRSets with the private key offline |
| 3 | Bring SIG records back to the live server |
| 4 | Lock private key CD-ROM in a **safe** |

**Benefit:** No on-the-fly crypto → faster queries; security reduced to physical security.  
**Trade-off:** Some records grow ~**10× in size** due to stored signatures.

---

### Exam Cheat-Sheet

| Topic | Key Point |
|-------|-----------|
| Basis | Public-key cryptography per **zone** |
| Signing unit | **RRSet** (not individual records) |
| New record types | **KEY, SIG, CERT** |
| Secrecy? | ❌ No — authenticity only |
| Caching safe? | ✅ Yes — each RRSet is self-signed |
| Private key storage | Offline (CD-ROM in safe) |
| Anti-spoofing | Signs a **hash of the query** → Trudy can't forge without private key |
| ID weakness | 16-bit IDs → guessable; random IDs help but don't fully solve it |
