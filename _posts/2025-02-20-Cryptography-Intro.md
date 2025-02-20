---
title: Intro 
categories: [cryptography, one-time-pad]
tags: [unibe, cachin]     # TAG names should always be lowercase
math: true
---

# Classic goals and technique
* **C** onfidentiality --> Encryption
* **I** ntegrity --> Signatures, authentication (MAC), hash
* **A** vailability --> (replication) but not part in this lecture

# Modern goals
* zero-knowledge
* anonymity
* compute with encryoted data
distributed turust (store a key partially at different places)

Note: exist quantum computer? If yes this could break the things we learn here in this 
lecture. Can we not just increase the key length? (PQC=post quantum cryptology)

# Terminology
Cryptology = cryptography + cryptoanalysis

# Encryption
## A model for encryption
Shannon works with symmetric encryption (1949, classified stuff in WW2)

```
                  key   <------key gen ----------> k

Alice ------------>      Enc --------- c ----------> Dec  ^m --->Bob            
         m          key                                key
                                   Eve

```
1. key gen is a randomized algorithm, provicing a random key
2. key is distributed over secure channel to Alice and Bob
3. Alice encrypts using k to generate cipher c
4. Bob decrypts with the decr-algorithm c using k and optains^m

## Requirements for the encryption algorithm
* correctness, m=^m
* security, Eve should not learn m, i.e. eve must not learn any usefull information about $m$.

**Question**  how do we make this formal? And how to implement? 

