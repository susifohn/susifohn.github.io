---
title: Bells number
categories: [haskell, listcomperhension]
tags: [function]     # TAG names should always be lowercase
math: true
---

# Fun fact
Du kannst drei Personen auf 5 mögliche Arten gruppieren. Was denkst du, wieviele Möglichkeiten gibt es mit deiner Klasse? Es sind vermutlich mehr als du denkst. Dieses Problem ist nur rekursiv lösbar! Findest du eine explizite Formel, wirst du sehr berühmt. 

# Bellsche Zahlen
Gegeben ist eine Menge und wir wollen die Anzahl Partitionen bestimmen. Zum Beispiel für die Menge {a,b,c} sind das

  *  {a,b,c}
  *  {a,b}, {c} 
  *  {a,c}, {b} 
  *  {b,c}, {a} 
  *  {a}, {b}, {c}

also fünf Partitionen. Dies ist die _Bellsche Zahl_ und kann rekursiv wie folgt berechnet werden: 

$$B_{n+1} = \sum^{n}_{k=0} \binom{n}{k} B_k$$

Dabei ist der Binomialkoeffizient $\binom{n}{k}$ wie folgt definiert:

$$\binom{n}{k} = \frac{n!}{k!(n-k)!}$$

Und die Fakultät $n!$ kennst du ja bereits als Funktion `fac`.

$$n! = \begin{cases}
1 & ,n = 0 \\
n(n-1)! &  ,n > 0 
\end{cases}$$

# Haskell implementation
```haskell
    fac :: Integer -> Integer
    fac 0 = 1
    fac n = n*fac (n-1)
    
    binom :: Integer -> Integer -> Integer
    binom n k = fac n `div` fac k `div` fac (n-k)
    
    bell :: Integer -> Integer
    bell 0 = 1
    bell n = sum [bell k * binom (n-1) k | k <- [0..n-1]]
```

## Test
Mit der `map` Funktion kannst du die ersten 11 Bellschen Zahlen einfach bestimmen:
```bash
Prelude> map bell [0..10]
[1,1,2,5,15,52,203,877,4140,21147,115975]
```
