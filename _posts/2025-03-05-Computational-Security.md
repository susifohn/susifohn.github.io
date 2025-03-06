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



