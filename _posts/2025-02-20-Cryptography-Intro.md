---
title: Crypto Intro 
categories: [cryptography, one-time-pad]
tags: [unibe]     # TAG names should always be lowercase
math: true
---

# Classic goals and techniques
* **C** onfidentiality --> Encryption
* **I** ntegrity --> Signatures, authentication (MAC), hash
* **A** vailability --> replication etc. but not part of this lecture

# Modern goals
* zero-knowledge
* anonymity
* compute with encrypted data
* distributed trust (store a key partially at different places)

**Note**  Do quantum computers exist? If yes, this could break the things we learn here in this 
lecture. Can we not just increase the key length? (PQC=post quantum cryptology)

# Encryption
You can't learn about cryptography without meeting Alice, Bob and Eve. 

*Eve the eavesdropper refers to someone who hung from the eaves (Dachvorsprung) of a building to hear converstaions happening inside. [R21]*
## A model for encryption
Shannon works with symmetric encryption (1949, classified stuff in WW2)

```
               key <---key gen -->key 
                |                  |
Alice -- m -> Enc ----- c ------> Dec--- ^m -->Bob            
                        |            
                      Eve  
```

1. key gen is a randomized algorithm, providing a random key
2. key is distributed over secure channel to Alice and Bob
3. Alice encrypts m using k to generate cipher c
4. Bob decrypts m with the decr-algorithm c using k and optains m^

The secrecy of the model shall only depend of the key. You might suggest to make the details of `Enc` and `Dec` algorithmus secret. Not a good approach as _Kerckhoff_ stated in 1883 as:

>**Kerckhoffs' Principle**
> *The method must not be required to be secret, and it must be able to fall into the eneny's hands without give the enemy any insight into the message.*

E.g. base64 encoding \
_joy of cryptography --> b25seSBuZXJkcyB3aWxsIHJlYWQgdGhpcw==_ \
involves
no secret information, it adds nothing in terms of security.

## Requirements for the encryption algorithm
* correctness, m=^m
* security, Eve should not learn m, i.e. Eve must not learn any usefull information about $m$.

**Question**  how do we make this formal? And how to implement? 
## Historic cryptograpy
Refer Book _the code Book from  simon singh_
* Scytale, tool from ancient Greece. Key is the diameter of the stick.
* Caesaer cipher. Key is the shift.
* Monoalphabetic substitution cipher. A permutation of the plain text, invertible of course for decryption. Key length is $26! \simeq 2^{88}$, the number of possible permutations. Thus the key is some sort of table, mapping each letter to a letter from the same alphabet. Despite the key space is huge, the structure of the plain text is not hidden and the cipher is insecure.

# Terminology
Cryptology = cryptography + cryptoanalysis

Probability with events $A$ , $B$, $\mathbf{P}[A]$

Probability distribution

$$ \mathbf{P}: \Omega \rightarrow [0,1]$$
$$ P[A] + P[B] = P[A \cup B], \forall A,B \; s.t. \; A \cap B = \emptyset$$

Random variables. $x \in X$. Probability distribution on $X$ 

$$ P_{X}(x) = P[X = x] \; for \; x \in X $$

Uniform randon variable, takes all values of its space with the same probability.

**Important** Arbitrary vs random! It is not the same. Adversary does arbitrary things, but we do controlled random things.
Random does not necessarily mean uniform random.

Finite set $\mathcal{S}$ , $s \leftarrow \mathcal{S}$ means $x$ is randomly choosen from set $\mathcal{S}$ with uniform probability.

$$\forall x \in \mathcal{S} : \mathbf{P}[s \leftarrow \mathcal{S}:s=x] = \frac{1}{ | \mathcal{S} | }$$ 

$s \leftarrow \mathcal{S}$ means an Algorithm chooses random $s$ from $\mathcal{S}$, and then in this case the Probability that $x=s$ is equal to $\frac{1}{ | \mathcal{S} | }$.
This notation is somehow the other way arround as in math for conditioned probability, where the form $\mathcal{P}[Event | condition]$ is used. 

Randomized algorithm $x \leftarrow \mathcal{R}(y)$ denotes the experiment of running $\mathcal{R}$ on input $y$ and assign its output to $x$. Here $\mathcal{R}$ is the algorithm  with its code.

And for comparison we write $=^?$

# One-time pad (OTP)
An important cipher, patented 1919 by the telegraph engineer Gilbert Vernam, first discovered 1882 by Frank Miller. First security proof by Shannon 1949.

Keys, messages and cipher text are all $\lambda$-bit strings. Thus $m,c \in \lbrace 0,1 \rbrace^{\lambda}$ and $ k \leftarrow \lbrace 0,1 \rbrace^{\lambda}$  which means to sample $k$ uniformly from the set of $\lambda$-bit strings.

Using the abbreviation $\Sigma := \lbrace 0,1 \rbrace$, the specific algorithms are given as follows


## keyGen()
$ k \leftarrow \Sigma ^{\lambda}$\
return k

## Enc(k,m)
return $k \oplus m$

## Dec(k,c)

return $k \oplus c$

## Correctness (Completeness)
Bob does indeed recover the intended plaintext from Alice and it holds
```
Dec(k, Enc(k,m)) = m
```

Proof: $k \oplus k \oplus m = 0 + \oplus m = m$

## Security
From Eve's pespective, seeing c corresponds to the following algorithm 

**Eavesdrop(m)**

$k \leftarrow \Sigma^{\lambda}$

$c \leftarrow k \oplus m$

return $c$

>**Note** The **Eavesdrop** algorithm is <ins>not</ins> what the attacker does, but what the attacker sees.  

## Thm
For all m, the output of the algortithm Eavesdrop is the uniform distribution over $\lambda$ -bit stirngs.

Proof: For arbitrarily fixed $m,c \in \lbrace 0,1 \rbrace^{\lambda}$ we will calculate the probability that Eavesdrop(m) produces $c$, using the fact 

$$k \oplus m = c \iff k= m \oplus c$$
The probability is 

$$ \mathbf{P}[Eavesdrop(m) = c] =$$

$$ \mathbf{P}[k \oplus m = c] =$$

$$ \mathbf{P}[k = m \oplus c] = 2^{-\lambda} $$ 

because k is uniformly random. 

**Remember** that the key $k$ has the same length as the message $m$. This is an unusual property. 

**Consider** that any goal of an attacker is to detect that $c$ does not follow an uniform distribution. In OTP the attacker can't. But maybe in other systems, an attacker could check whether the cipher follows properties of a true random generator and if not, use this for cryptoanalysis.  


# References
## [R21] 
Mike Rosulek, *The Joy of Cryptography*, 2021. 
Available online [here](https://joyofcryptography.com)

## [KL21]
Jonathan Katz, Yehuda Lindell 
*Introduction to Modern Cryptography*, Third Edition.







