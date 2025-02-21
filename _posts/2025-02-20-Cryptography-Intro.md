---
title: Intro 
categories: [cryptography, one-time-pad]
tags: [unibe, cachin]     # TAG names should always be lowercase
math: true
---

# Classic goals and techniques
* **C** onfidentiality --> Encryption
* **I** ntegrity --> Signatures, authentication (MAC), hash
* **A** vailability --> (replication) but not part in this lecture

# Modern goals
* zero-knowledge
* anonymity
* compute with encrypted data
* distributed trust (store a key partially at different places)

**Note**  Do quantum computers exist? If yes, this could break the things we learn here in this 
lecture. Can we not just increase the key length? (PQC=post quantum cryptology)

# Encryption
## A model for encryption
Shannon works with symmetric encryption (1949, classified stuff in WW2)

```
               key <---key gen -->key 
                |                  |
Alice -- m -> Enc ----- c ------> Dec--- ^m -->Bob            
                        |            
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
## Historic cryptograpy
Refer Book _the code Book from  simon singh_
* Scytale, tool from ancient Greece. Key is the diameter of the stick.
* Caesaer cipher. Key is the shift.
* Monoalphabetic substitution cipher. A permutation, invertible. Key ($26! \simeq 2^{88}$ is the permutation, means a table in the form
 - a->f
 - b->m
 - ...
 - z->o
Keyspace is huge, but leaks the structure of the plaintext and is therefore insecure.

# Terminology
Cryptology = cryptography + cryptoanalysis

Probability with events $A$ , $B$, $\mathbf{P}[A]$

Probability distribution 
$$ \mathbf{P} : \omega \rightarrwo [0,1]$$
$$ P[A] + P[B] = P[A \union B] for all A,B \; s.t. \; A \cut B = {}$$

Random variables. $x \in \chi$. Probability dist on $\chi$ 
$$ P_{\chi}(x) = P[\chi =x] \; for \; x \in \chi$$

Unoform randon variable, takes all values of its space with the same probability.

**Important** arbitrary vs random! It is not the same. Adversary does arbitrary things, but we do controlled random things.

Random does not necessarily mean uniform random.

Finit set S, (x <- S) means x is randomly choosen from set S with uniform probability.

$$\forall x \in \mathcal{S} : \mathbf{P}[s \leftarrow \mathcal{S}:s=x] = \frac{1}{ | \mathcal{S} | }$$ 

$s \leftarrow \mathcal{S}$ means an Algorithm chooses randomy s from S, and then in this case the Probability that x=s.
What is the other way arround in math for conditioned probability in the form P[ that event given condition]. 

Randomized algorithm $x \leftarrow \mathcal{R}(y)$ denotes the experiment of running R on input y and assign its output to x. Here R is the algorithm  with its code.

For comparison we write $=^?$

# One-time pad
An important Vernam cipher. Firs security proof by Shannon 1949.
## Syntax
Keys, messages and cipher text are all $\lambda$-bit strings.

E.g. 
$$m \in {0,1}^{\lambda}$$

$$ k \leftarrow {0,1}^{\lambda}$$

Using the abbreviation $\sum := {0,1}$


# our first crypto system 
## keyGen()

$$ k \leftarrow \Sigma ^{\lambda}$$

return k

## Enc(k,m)
return $k \oplus m$

## Dec(k,c)

return $k \oplus c$

## Correctness (Completeness)

Dec(k, Enc(k,m)) = m

Proof: $k \oplus k \oplus m = 0 + \oplus m =m$

# Security
Eve optains c, the ciphertext. 

Consider an Algorithm **Eavesdrop(m)**

$k \leftarrow \Sigma^{\lambda}$

$c \leftarrow k \oplus m$

return c   (Eve "sees" c)

## Thm:
For all m, the output of the Alg. Eavesdrop is the unoform distribution over $\lambda$ -bit stirngs.

**Proof** Take any m, take any c. then

$$ \mathbf{P}[Eavesdrop(m) = c] =$$

$$ \mathbf{P}[k \oplus m = c] =$$

$$ \mathbf{P}[k = m \oplus c] = 2^{-\lambda} $$ 

because k is uniformly random. 

**Remember** that the key k has the same length as the message m. This is an unusual property. 

# Todo 
read chapters in referred book.







