---
title: Notes 
categories: [unibe, Berechenbarkeit und Complexity]
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
## Definition Polynomiale Reduktion
$$A \le_p B$$ heisst A ist auf B polynomial reduzierbar. Dazu braucht's eine totale Funktion $f \in P$ welche von A nach B abbildet. Damit kann das Entscheidungsproblem in B gelöst werden und es gilt
$$
x \in A \iff f(x) \in B
$$

Beachte, dass $f^{-1}$ nicht berechnet werden muss, weil uns nur der Entscheid $x \in A$ interessiert, nicht die Lösung. 

## Lemma: P Reduktion
Falls  $A \le_p B$ und $B \in P$ dann auch $A \in P$. 

Das Lemma sagt: *if A reduces polynomially to B, and B is easy (in P or NP), then A is also easy.*

Beweis: es gibt eine TM $M_f$ welche f in P berechnen kann. Und es gibt auch eine TM M, welche B entscheidet, in P. Die seq. Komposition mit $A=T(M_f;M) nimmt zuerst x vom Band und wendet $M_f$ and und schreibt das aufs Band. Dann liest M das vom Band und entscheidet $f(x)$. Und das alles in P Schritten, weil auch $p(q(x))$ wieder ein Polynom ist.

# 10. Weitere **NP Vollständige** Probleme
## 3KNF-SAT
Erinnere, wir haben gesehen, dass 2KNF-SAT in P liegt. 

Aber 3KNF Erfüllbarkeit liegt in NP.

## Mengenüberdeckung
Gibt es eine Auswahl aus $n \le k$ Teilmengen, welche $M$ bilden?

## Rucksack Problem
Können wir aus gegebenen Zahlen k auswählen um genau b zu erhalten?

## Partition

Können wie gegebene Zahlen so aufteilen, dass beide Teile dieselbe Summe ergeben?

## Gerichteter/ungerichteter  Hamilton-Kreis

Gibt es eine Rundreise im Graph G wo jede Kante genau einmal besucht wird?

- Euler Kreis: jede Kante
- Hamilton Kreis: jeder Knoten
- Beim Travellin sales Person gab's noch eine Obergrenze mit der Läge, weil die Kanten eine Länge hatten.

## Färbbarkeit
 k Farben, keine zwei Farben sind Nachbarn. 

## Übersicht

SAT -> 3KNF-SAT->Clique->Knotenüberdeckung

Die Pfeile sind die Reduktionsrichtung. Haben gezeigt dass SAT **NP hart** ist. Dann ist das auch Clique und Knotenüberd. Umgekehrt ist Knotenüberdeckung in NP so dann rückwärts auch SAT.

Um das zu zeigen müssen wir zuerst NP-vollständigkeit von 3KNF-SAT zeigen, weil dann erhalten wir eine einfache Struktur und können alles andere auch auf NP vollständig zeigen. 

## NP-Vollständigkeit von 3KNF-SAT

> Theorem: 
> 3KNF-SAT ist NP-Vollständig.

Die Reduktionsfunktion muss auch in polynomialer Zeit mögich sein, und einfach eine Formel in KNF umzuformen ist exponentiell (beim Ausmultiplizieren kommen Variablen mehrmals vor, und die Formel wird immer länger) und geht nicht, wir brauchen eine schlauere Reduktion. 




