---
title: Bells number
categories: [haskell, listcomperhension]
tags: [function]     # TAG names should always be lowercase
math: true
---

---
math: true
---

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

# Hakkell implementation

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
```bash
Prelude> map bell [0..10]
```