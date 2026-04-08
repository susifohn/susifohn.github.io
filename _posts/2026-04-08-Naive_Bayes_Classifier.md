---
title: The Naive Bayes Classifier
categories: [Machine Learning ]
tags: [gibb, tsb, Bayes, Wahrscheinlichkeit, classification]     # TAG names should always be lowercase
math: true
---

# Literatur
- Kapitel *Bayes' Theorem*, Math for deep learning, Kneusel, no starch press. 
- [https://web.stanford.edu/class/archive/cs/cs109/cs109.1218/files/student_drive/9.3.pdf](https://web.stanford.edu/class/archive/cs/cs109/cs109.1218/files/student_drive/9.3.pdf)

# Motivation
Schon mal gewundert, wie ein E-Mail als Spam markiert wird? Ein klassischer Ansatz in den ersten Spamfilter dazu war, mit Wahrscheinlichkeiten zu arbeiten, im speziellen mit dem *Bayes' Theorem*. Dieser sehr naive Ansatz, wie wir sehen werden, funktioniert für viele Klassifizierungsprobleme überraschend gut, so auch zum Klassifizieren von E-Mails. 

![Spam](../assets/images/spam.png)

Damit wir den sogenannten *Naive Bayes Classifier* verstehen und auf eigene Probleme anwenden können, brauch's einige 

# Grundbegriffe der Wahrscheinlichkeitsrechnung

#### Definitionen
Ein *Zufallsexperiment* ist ein Vorgang, dessen Ereignisse oder Ausgänge nicht mit Sicherheit vorhergesagt werden kann, aber dessen mögliche Ereignisse bekannt sind.

Die Menge aller möglichen Ereignisse eines Zufallsexperiments heißt *Ereignisraum*  $\Omega$.

Die *Wahrscheinlichkeit* eines Ereignisses $A \subseteq \Omega$ ist eine Zahl $0 \le \mathbb{P}(A) \le 1$ 
$$\mathbb{P}(A) = \frac{|A|}{|\Omega|}$$

#### Beispiele
Wir messen den Glukosewert eines Patienten und klassifizieren ihn als diabetisch oder nicht-diabetisch. Wir kennen den Glukosewert nicht im voraus, der Wert ist für uns Zufällig. 
$$ \Omega = \{diabetes, nicht-diabetes\}$$

Wir werfen einen Würfel. Das Ereignis oder der Ausgang des Würfelexperiments ist zufällig. 
$$\Omega = \{1,2,3,4,5,6\}$$

Wir werfen zwei Würfel. Hier ist ein Ereignis ein Paar.
$$\Omega = \{(1,1), (1,2), \ldots, (6,5), (6,6)\}$$

Die Wahrschienlichkeit für das Ereignis $W$ *beide Würfel zeigen dieselbe Zahl* berechnet sich wie folgt
$$\mathbb{P}(W) = \frac{|(1,1), (2,2), (3,3), (4,4), (5,5), (6,6)|}{|\Omega|} = \frac{6}{36}$$









