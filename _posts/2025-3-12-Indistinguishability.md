---
title: 3 Second part 
categories: [cryptography]
tags: [unibe, indistinguishability, birthday probabilities, birthday paradox, bad event lemma, collisions]     # TAG names should always be lowercase
math: true
---

## Recap
Explain negligible probability. 

This is an asymptotic behaviour, parametrized by $\lambda$. $\lim_{\lambda\to\infty} f(\lambda) = 0$ does not imply negligiblity. Remember the definition in other words:
> $f(\lambda) \ \text{is negligible} \iff \forall \ c \ \exists \ \lambda_0 : \ \forall \ \lambda \ge \lambda_0 \ f(\lambda) < \frac{1}{\lambda^c}$

# Indistinguishability
Our previous security definition for *interchangeability* requires the two libraries have exactly the same effect. We want weaken this, because it is sufficent, if the two libraries have negligible difference, if ppt programms are using them. 

> **Def**
> Let $L_l$ and $L_r$ be two libraries with a common interface. We say they are indistinguishable and write $L_l \cong L_r$, if for all ppt $A$ that output 1 bit
> 
> $$P[A \diamond L_l \implies 1] \cong P[A \diamond L_r \implies 1]$$
> 
> The absolute difference of the two probabilities we call *advantage* or *bias* and is a function of $\lambda$.

**Remark** The operator $\cong$ is only transitive, if applied a polynomial number of time. Because closure of polynom multiplication. 

### Example
Take the two libraries

image::../assets/images/example4_5.png[]

One strategy of an $A_q$ could be to try $q$ times. Rather than calculating the exavt probabilita, we use union bound to give an upper bound:

$$P[A_q \diamond L_l \implies 1] \le P[1^{st} \ \text{returns true}] + P[2^{nd} \ \text{returns true}] + \ldots = q\cdot 2^{-\lambda}$$

Thus $A_q$ has negligible probability to distinguish the two libreries. But we have to show this for all ppt $A$. this leads us to the following 
## Bad-Event Lemma
A rare event, helping the adversary to distinguish the libraries, can be seen as a bad event. E.g. the adversary guesses our key. In that case, we can bound an Attackers advantage by the probability of the bad event. Formally
> **Lemma**
> Let $L_{left}$ and $L_{right}$ be libraries, that define a variable `bad` initialized to 0 (False). If both libraries have identical code, except for code blocks reachable only `if bad = 1` statement, then
>
> $$ \left| Pr[A \diamond L_{left} \implies 1] - Pr[A \diamond L_{right} \implies 1] \right| \le Pr[A \diamond L_{left} \ \text{sets bad} =1]$$

**Remark** We can also use $L_{right}$ instead, because the code is the same, except behind the `if bad`.

*Proof* Let $A$ be any calling program fix. Define two events
* $B_l$ the event that $A \diamond L_l$ sets `bad` =1 at some point
* $B_r$ same but for $L_r$

Let's write $\bar{B}$ for the complement event i.e. `bad`= 0. \
Observe $Pr[B_l] = Pr[B_r]$ because the code executed to affect `bad` is in both libraries the same. And remember *Gesetz der totalen Wahrscheinlichkeit*: $P(A) = P(A|B) \cdot P(B) + P(A|\bar{B}) \cdot P(\bar{B})$. Write 

* $Pr[A \diamond L_{left} \implies 1] = \ldots$
* $Pr[A \diamond L_{right} \implies 1] = \ldots$
* put in $advantage_A= \left| Pr[A \diamond L_{left} \implies 1] - Pr[A \diamond L_{right} \implies 1] \right|$

Which simplifies finally to 

$$advantage_A \le p^* = Pr[B_l] = Pr[A \diamond L_l \ \text{sets bad}=1]$$

$\square$ *(for all details refer to [R21 p74ff)*

### Example
Do the example above with the simple predict() as exercise. See solution in [R21 page 75f].

# Collision Probabilities
### Video of the week
Hashing Algorithms and Security

!(Computerphile[https://www.youtube.com/watch?v=b4b8ktEV4Bg]

## Intro - Birthday paradox (23!)
* Sampling with replacement. Take a glass full of different balls. What is the probability, drawing the same ball twice.
* Collision Probabilities in gerneral

Take $q$ samples {1,...,N}. What is the probability of having at least two equal?

  


 

