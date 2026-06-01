---
title: 11. Sicherheit in vernetzten Systemen  
categories: [unibe, Vernetzte Systeme und Betriebssysteme]
tags: [morris, thompson, lamport, kerberos]     # TAG names should always be lowercase
math: true
---

# Sicherheit in vernetzten Systemen — Exam Notes

---

## 1. CIA — Security Properties (Ex. 11.1)

### Definitions


| Prop. | Definition | Attack |
|--------|------------|--------|
| **Conf.** | Authorized access only | Theft |
| **Integ.** | No unauthorized changes | Tampering |
| **Avail.** | Accessible when needed | DoS |

### Attack ↔ CIA Mapping

```
Data Theft       →  Confidentiality  (unauthorized read)
Data Tampering   →  Integrity        (unauthorized modification)
Denial of Service→  Availability     (service made unreachable)
```

---

## 2. Authentication — Passwords & Salting (Ex. 11.2)

### How Passwords Are Stored

Passwords are **not stored in plaintext** — only their **hash** is stored:
```
stored = hash(password)
```

### Dictionary Attack (How the Attacker Proceeds)

1. Attacker steals the password file (contains usernames + hashed passwords)
2. Attacker builds a **dictionary**: a list of likely passwords (common words, names, etc.)
3. For each dictionary entry: compute `hash(word)` and compare to stored hashes
4. If a hash matches → password found ✓

> One pre-computed dictionary works against **all** 5000 users simultaneously — very efficient!

### Morris & Thompson Counterattack: **Salting**

**Idea:** Append a random value (the **salt**) to the password before hashing:

```
stored = hash(password + salt)
```

The salt is stored in plaintext alongside the hash. It is **not secret** — its purpose is to make pre-computation useless.

**Why it works:**
- Two users with the same password will have **different hashes** (different salts)
- The attacker **cannot reuse** one pre-computed dictionary for all users
- For each user, the attacker must compute a **separate dictionary** using that user's specific salt

### Dictionary Size Calculation

> Original dictionary: **1024 entries**, Salt: **64-bit** → 2⁶⁴ possible salt values

Without salt: attacker needs **1024** pre-computed hashes (works on all users).

With 64-bit salt: for each dictionary word, there are 2⁶⁴ possible salted variants.

```
Final dictionary size = 1024 × 2^64
                      = 2^10 × 2^64
                      = 2^74 entries
```

> This makes pre-computation **completely infeasible** in practice. The attacker must recompute the entire dictionary from scratch for each individual user's salt.

---

## 3. Malware — Trojans, Worms, Viruses (Ex. 11.3)

### Definitions & Comparison

| | **Tr** | **Vi** | **Wo** |
|---|---|---|---|
| **Def.** | Disguised malware | Infects host files | Standalone malware |
| **Host?** | ✓ App disguise | ✓ Needs file | ✗ No |
| **Replicates?** | ✗ | ✓ | ✓ |
| **Infection** | User runs it | Runs infected file | Network exploit |
| **Spread** | Download/social | Files/email | Auto network spread |
| **User action** | ✓ | ✓ | ✗ |
| **Examples** | Fake AV | Macro virus | WannaCry |

### How Malware Spreads WITHOUT User Intervention

A worm can spread autonomously by:

1. **Exploiting unpatched vulnerabilities** in network services (e.g. a buffer overflow in a web server or OS service that is reachable over the network)
2. **Scanning** the network for other vulnerable hosts automatically
3. **Copying itself** to the remote host and executing itself there — all without any human clicking anything

> Classic example: **Morris Worm (1988)** — exploited bugs in `fingerd`, `sendmail`, and `rsh`/`rexec` to propagate automatically across the early Internet.

### Summary Comparison

```
Trojan:  Fake legitimate app  →  user runs it  →  damage done (no spreading)
Virus:   Attaches to files    →  user runs infected file  →  infects more files
Worm:    Standalone           →  exploits network service  →  spreads automatically
```

---

## Exam Cheat-Sheet

| Topic | Key Point |
|-------|-----------|
| **CIA** | Confidentiality, Integrity, Availability |
| **DoS** | Attacks **Availability** |
| **Data Theft** | Attacks **Confidentiality** |
| **Data Tampering** | Attacks **Integrity** |
| **Dictionary Attack** | Hash all common words, compare to stolen hashes |
| **Salt** | Random value appended to password before hashing |
| **Salt purpose** | Makes pre-computed dictionaries useless |
| **Salt size formula** | Dictionary size = entries × 2^(salt bits) |
| **64-bit salt, 1024 words** | 1024 × 2⁶⁴ = **2⁷⁴** entries needed |
| **Trojan** | Disguised app, no self-replication, needs user to run |
| **Virus** | Attaches to files, needs user to run infected file |
| **Worm** | Standalone, spreads automatically via network exploits |
| **No-user-intervention spread** | Worm exploiting a network vulnerability (buffer overflow etc.) |
