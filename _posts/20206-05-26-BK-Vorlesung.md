---
title: Notes 
categories: [unibe, Berechenbarkeit und Komplexität]
tags: [3KNF, SAT]     # TAG names should always be lowercase
math: true
---

# 0. Vorbemerkungen

# 1. Turing Maschinen

# 2. Loop While Goto

# 3. Rec

# 4. Unentscheidbarkeit

# 5. Kolmogorov

# 6. Vorbemerkungen Komplexität

# 7. Die Klasse P

## Uniformelles und logarithmisches Kostenmass
Es gibt zwei Möglichkeiten die Kosten zu messen:
1. uniform, einfach Anweisungen zählen.
2. logatithmisch, #Bits, die gesetzt werden. 

> um eine Zahl $$z$$ darzustellen, brauch's immer $$log(z)$$ Zeichen.

- uniform, wenn Wertzuweisung x=y 1 kostet
- log wenn Zeitaufwand gleich der Anzahl der transportierten Bits. 


## Komplexität von WHILE Programmen
Sind die gespeicherten Zahlenwerte kleiner als eine Konstante, dann unterscheidet sich die Rechenzeit der beiden Kostenmasse nur um einen fixen Faktor. 

WHILE Progr mit log Kostenmass gibt Klasse P, also in Polynomial vielen Schritten lösbar. 

Mit dem uniformen Kostenmass umfasst die Klasse auch exponentielle Algorithmen, somit mächtiger. 

## Beispiel Berechnung von $2^y$

```
INPUT(y);
d := 1;
Sei y_k y_k−1... y_1 y_0 die Binärdarstellung von y
FOR i := k, . . . , 0 DO
d := d ∗ d;
IF y_i = 1 THEN d := d ∗ 2 END;
END
OUTPUT(d)
```

### Vergleich der Kostenmasse
- FOR Loop wird $$log_2(y)$$ mal ausgeführt. Das entspricht der Länge der Eingabe y und somit ist der Alg bezüglich dem uniformen Kostenmass linear in der Länge des Input.
- Bei log Kostenmass ist der Alg exponentiell. Denn alleine um $$2^y$$ zu schreiben braucht's y+1 Operationen. y ist exponentiell in der Länge von y als Binärzahl. 

## PATH 
Erreichbarktie, kann von s auch t erreicht werden?

> Theorem
> $$\text{PATH} \in P$$

Es gibt $m!$ Pfade in $G$, (m=#Knoten), darum alle durchsuchen ist exponentiell. Wir brauchen die **Tiefensuche**, wo wir alle Knoten weiss einfärben und die besuchten schwarz und rekursiv suchen. Wenn wir so bei s starten ist das polynomial

## Relative PRIM ist in P

> Theorem
> $$\text{RELPRIM} \in P$$
Beweis mit Euklid, wo Inputzahlen abwechselnd immer mindestend halbiert werden. 

## 2KNF ERfüllbarkeit ist in P
> Theorem
> $$ \text{2KNF-SAT} \in P$$
Brauchen *strake Zusammenhangskomponenten* und Tiefensuche. 

> Theorem
> Wir können in einem Graphen die SCC (strongly connected components) in polynomialer Zeit finden. 

Beweis: mit *Finishing Time*, transponiertem Graphen und Tiefensuche.





# 8. Die Klasse NP

# 9. NP Vollständigkeit

# 10. Weitere NP Probleme
## 3KNF-SAT
Erinnere, wir haben gesehen, dass 2KNF-SAT in P liegt. 
