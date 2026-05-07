---
title: Wochenauftrag 2
categories: [Machine Learning ]
tags: [gibb, tsb, exercises]     # TAG names should always be lowercase
math: true
---

- Start: Fr. 8. 5.
- Besprechung: Fr. 22. 5.

In diesem Wochenauftrag tauchen wir in die Wahrscheinlichkeitstheorie ein und befassen uns mit einigen Grundbegriffen, welche uns dann helfen, mit statistischen Methoden Klassifikationsprobleme zu lösen. Die Methoden, welche wir betrachten sind *Naive Bayes Classifier* und *logistische Regression*.

## Aufgabe 1
Du Wirfst einen Würfel und eine Münze, beide fair. 

1. Was ist der Ergebnisraum?
2. Welches sind die Elementarereignisse?
3. Ist das ein Laplaceexperiment?
4. Was ist die Wahrcheinlichkeit dass du Kopf wirfst und eine Zahl kleiner als 5?

## Aufgabe 2
Analsiere beim Monty-Hall-Problem, welche Strategie besser ist. Tu das, indem du für beide Strategien die günstigen Ergebnisse zählst und so die Wahrscheinlichkeit für einen Gewinn für beide Strategien bestimmst. 

## Aufgabe 3
Die Standardanwendung des Bayes Theorems ist sinngmäss die folgende Aufgabe, mit erfundenen Daten: Wir nehem an, es gibt einen medizinischen Test, um bei einer Person festzustellen, ob eine Infiszierung mit dem Hantavirus vorliegt oder nicht. Der Test hat eine Zuverlässigkeit von 98.5%. Wir nehmen weiter an, dass Hantavirus-Infektionen in der Bevölkerung bei 0.003% liegen.

Nun testen wir einen Patienten positiv. Wie gross ist die Wahrscheinlichkeit, dass der Patient tatsächlich infisziert ist? 

## Aufgabe 4a

Wir möchten mit einem Bernoulli-Naive-Bayes-Klassifikator entscheiden,  
ob ein Text von der bevorstehenden Fußball WM handelt oder nicht.

Wir betrachten nur drei Wörter: 
1. Tor
2. Ball
3. Trainer

Für jedes Word gilt.

- `1` = Wort kommt im Text vor
- `0` = Wort kommt nicht vor


### Trainingsdaten

| Tor | Ball | Trainer | Klasse |
|---|---|---|---|
| 1 | 1 | 1 | Fußball |
| 1 | 1 | 0 | Fußball |
| 0 | 1 | 1 | Fußball |
| 0 | 0 | 1 | Nicht Fußball |
| 0 | 0 | 0 | Nicht Fußball |
| 1 | 0 | 0 | Nicht Fußball |


### Unbekannter Text

Ein neuer, unbekannter Text enthält das Wort Tor und Ball, jedoch nicht das Wort Trainer. 

### Bestimme von Hand auf Papier mit Bernoulli Naive Bayes:

1. Die Klassenwahrscheinlichkeiten
2. Die bedingten Wahrscheinlichkeiten der Wörter. Ohne Laplace Smoothing.
3. Die Wahrscheinlichkeit für:
   - Fußball
   - Nicht Fußball
4. Entscheide, zu welcher Klasse der Text gehört.


## Aufgabe 4b
Versuche die Aufgabe mit [sklearn.naive_bayes.BernoulliNB](https://scikit-learn.org/stable/modules/generated/sklearn.naive_bayes.BernoulliNB.html) zu lösen.

## Aufgabe 5

In unserem Schulhaus soll automatisch erkannt werden, ob ein Raum ein **Klassenzimmer** ist oder nicht.

Dazu wird nur ein einziges Feature verwendet: Anzahl der Stühle im Raum.


- **1 = Klassenzimmer**
- **0 = kein Klassenzimmer (z.B. Büro, Lagerraum, Besprechungsraum)**


## Trainingsdaten

| Stühle | Klasse |
|---|---|
| 12 | 0 |
| 5 | 0 |
| 18 | 1 |
| 20 | 1 |
| 22 | 1 |
| 24 | 1 |
| 26 | 1 |
| 2 | 0 |
| 6 | 0 |
| 13 | 0 |

---

## Aufgabe
Verwende [sklearn.linear_model.LogisticRegression](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html)

Trainiere mit logistischer Regression und beantworten Sie:


1. Wie hoch ist die Wahrscheinlichkeit für ein Klassenzimmer bei:
   - 17 Stühlen
   - 21 Stühlen
   - 29 Stühlen
2. Bestimme die Gewichte und Zeichne die Aktivierungsfunktion. 
3. Bei welcher Anzahl Stühlen kippd die Klasse.



## Hinweis

Die logistische Regression modelliert eine Wahrscheinlichkeit:

:contentReference[oaicite:0]{index=0}

Dabei gilt:

- \(x\) = Anzahl der Stühle
- \(y=1\) = Klassenzimmer
- \(y=0\) = kein Klassenzimmer

---

## Zusatzfrage (Interpretation)

Warum ist dieses Problem gut für logistische Regression geeignet?

- lineare Trennbarkeit im 1D-Fall
- klare Schwelle (ungefähr 18–26)
- Übergang ist probabilistisch, nicht hart
















