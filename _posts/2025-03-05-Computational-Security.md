---
title: 3 Proofs and Computational Security
categories: [cryptography]
tags: [unibe]     # TAG names should always be lowercase
math: true
---

# Recap

Last time have seen 2 notions for crypto systems, so called _one-time notions_: (know this for oral exam)
1. **ots$** one time random $\rightarrow$ we have random distribution 
2. **ots**    one time security $\rightarrow$ we cannot distinguish from the cipher, which of to messages was encrypted. Leads to **interchangeability**.

From the exercises, we know

$$ ots\$ \implies ots$$

But not the other way arround. E.g. a random number $r$ concatenated $r \Vert r$ ist $ots$ but not $ots\$$. 

This notions are very strong, because the key is used only once for each encryption. 

# Composing Libraries

**Lemma** If $L_{left} \equiv L_{right}$ then for any $L^*$ it holds 

$$L^* \diamond L_{left} \equiv  L^* \diamond L_{right}$$

Here the Symbol $\diamond$ is extended to libraries. 

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





