---
title: 3 Proofs and Computational Security
categories: [cryptography]
tags: [unibe, security proof]     # TAG names should always be lowercase
math: true
---

# Todays topics
* Security proofs, hybrid proofs
  - Ref [R21] 2.4, 2.5
* Computational security, negiligible probability, indistinguishability
  - Ref [R21] 4.1, 4.2, 4.3
  
# Recap

Last time have seen 2 notions for crypto systems, so called _one-time notions_: (know this for oral exam)
1. **ots$** one time random $\rightarrow$ we have random distribution 
2. **ots**    one time security $\rightarrow$ we cannot distinguish from the cipher, which of to messages was encrypted. Leads to **interchangeability**.

From the exercises, we know

$$ ots\$ \implies ots$$

But not the other way arround. E.g. a random number $r$ concatenated $r \Vert r$ ist $ots$ but not $ots\$$. 

This notions are very strong, because the key is used only once for each encryption. 

# Composing Libraries
Consider a compound programm $A \diamond L_1 \diamond L_2$. As per our convention, function calls only happen from left to right across $\diamond$. Then we have associativity as:
* $(A \diamond L_1) \diamond L_2$ a compound calling Programm $A$ linked to $L_2$ and $A \diamond L_1$ is a programm calling functions in $L_2$.
* $A \diamond (L_1 \diamond L_2)$ means $A$ is linked to the compound library $L_1 \diamond L_2$ and $A$ is a programm calling functions in $L_1 \diamond L_2$

The placement of the parantheses makes no difference in the functionality. In our security proofs we will have intermediate steps using _compound libararies_ and the following Lemma will be very helpful: 

**Lemma** If $L_{left} \equiv L_{right}$ then for any $L^*$ it holds 

$$L^* \diamond L_{left} \equiv  L^* \diamond L_{right}$$

*Proof* $A$ is an arbitrary programm. Then from the assumption follows $P[A \diamond L_L \implies 1] = P[A \diamond L_R \implies 1]$


Now as with currying in Haskell functions, we cann add $L^\*$ to $A$ and treat this as our program with $L^\*$ linked in. Thus


$$P[A \diamond (L^* \diamond L_L) \rightarrow 1]$$

$$=P[(A \diamond L^*) \diamond L_L \rightarrow 1]$$

and by assumption that $L_L \equiv L_R$ we get

$$=P[(A \diamond L^*) \diamond L_R \rightarrow 1]$$

$$=P[A \diamond (L^* \diamond L_R) \rightarrow 1] $$

$\square$

# Prove a relation among security notions
Using `ots` and `ots$` 

**Thm** if an encr scheme $\Sigma$ has one-time cipher text ots$ (also named uniform cipher text) then it also has one-time secrecy ots. More formaly this means:

$$L_{ots\$-real}^{\Sigma} \equiv L_{ots\$-rand}^{\Sigma} \implies L_{ots-L}^{\Sigma} \equiv L_{ots-R}^{\Sigma}$$

*Proof* (hybrid proof) Transform one library into an other in steps. This intermediate libraries are called _hybrids_.

TODO (sollte man auswendig können :-)
# Chapter 3: (Secret Sharing) omitted
# Chapter 4: Computational security

> John Nassh wrote 1955 to NSA some letter, telling that it doesn't really matter whether attacks are impossible, only whether attacks are _computationally infeasible_. This letter was classified until 2012, probaly preventing modern cryptography to be developping faster. [Original letter](https://www.nsa.gov/portals/75/documents/news-features/declassified-documents/nash-letters/nash_letters1.pdf)

* perfect security so far $\iff$ unconditioned security.
* perfect security we have seen, is to strong and not relevant in practice.
* We want distribute the key once and then use the key many times to encrypt and decrypt.
* perfect security $\implies$ keys can be used only once and key as long as the message.
* Computational security exploits that keys can be reused and the key space is to large for brute force (try every key by an adversary)

$\implies$ we can use the key many times and the key can be shorter than the message.

We introduce a security parameter $\lambda$ to quantify the security level. Here for computational security, not one-time security.
* ideally the adversary needs $2^{\lambda} steps or memory
* ideally the work for honest users is proportional to $\lambda^c$ for $c$ small.

*Example* AES-128 has 128-bit keys. In principle takes $2^{128}$ steps to break it. 

## Illustration of practical cryptographical difficulty
Unit: 1 crypto operation. You could also estimate the costs using Amazon EC2, $2^{50}$ clock cycles cost CHF 3.50, a cup of coffee, ref [21R] 4.1)

| Scheme    | Difficulty | Time $10^9 op \cdot s^{-1}$
| -------- | ------- | ------- |
| 40-bit key (export-permitted by US until 1999   | $2^{40}$    | 18 Min |
| DES 1977  | $2^{56}$     | 2.3 years |
| SHA-1 1995    | $2^{80}$    | 584 years |
| AES-128 (2001) and SHA-256| $2^{128}$| $10.8 \cdot 10^{21}$ years (longer than universe)|
| AES-256, SHA-512 | $2^{512}$ | $3.7 \cdot 10^{60}$ years |

- Refer to [keylength.com](https://www.keylength.com) 
- See video [3Blue1Brown](https://www.youtube.com/watch?v=S9JGmA5_unY)

  
## Formalize efficient computation
* efficient means polynomial time computable
* Parametrized by $\lambda$

**Def** An algorithm $A$ runs in (probablistic) polynomoial time (ppt) in the length of the input, in our case this is $\lambda$, if exists some $c>0$ constant such that for all inputs x of size $\lambda$ $A(x)$ halts after at most $\mathcal{O}(\lambda^c$) steps.

Think: can we enumerate $2^{\lambda}$ keys in ppt?

**Def** An algorithm is efficient if it is `ppt`.

### Example
Asymptotic consideration only.
An alg. takes a number of the following steps
* $\lambda^3 \rightarrow$ is efficient
* $\lambda^{1000} \rightarrow$ is efficient
*  $e^{\lambda} \rightarrow$ is **not** efficient
*  $\lambda^{log log \lambda} \rightarrow$ is not efficient
  
## Negligible Probabilities
- Traidoff between computation time and success probability in cryptography. Adding on Bit to the key doubles the adversaries time.
- Guessing the key is always possible, but the probability is $2^{-\lambda}$

We need a negligible function. 

**Def** A function $f:\mathbf{R} \rightarrow \mathbf{R}_{\geq 0}$ is negligible iff for all polynomials $p$ it holds 

$$\lim_{\lambda\to\infty} p(\lambda) \cdot f(\lambda) = 0$$

Repeating some attack that has negligible successprobability $c$ polynomila number of times still results in a negligible success.

**Lemma** If $\forall c > 0$ , it holds

$$\lim_{\lambda\to\infty} \lambda ^c \cdot f(\lambda) = 0$$

then $f$ is negligible.

### Examples
* $2^{-\lambda}$ is negligible
* $\lambda ^{-100}$ is not negligible

*Missing lats few minutes in the podcast, no sound*


