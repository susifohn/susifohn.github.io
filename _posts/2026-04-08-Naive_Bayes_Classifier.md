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
Ein *Zufallsexperiment* ist ein Vorgang, dessen Ergebnis oder Ausgang nicht mit Sicherheit vorhergesagt werden kann, aber dessen möglichen Ergebnisse bekannt sind.

Die Menge aller möglichen Ergebnisse eines Zufallsexperiments heißt *Ergebnisraum* (*sample space*) $\Omega$. Ein *Ereignis* (*event*) ist eine Teilmenge des Ergebnisraums oder eine Probe (*sample*).

Ein *Elementarereignis* ist ein einzelnes mögliches Ergebnis eines Zufallsexperiments.

*Laplace Experimente* sind Zufallsexperimente bei welchen jedes Elementarereignis dieselbe Wahrscheinlichkeit hat. 

Die *Wahrscheinlichkeit* eines Ereignisses $A \subseteq \Omega$ ist eine Zahl $0 \le \mathbb{P}(A) \le 1$ und berechnet sich für Laplace Experimente folgendermassen
$$\mathbb{P}(A) = \frac{\text{Anzahl der günstigen Ergebnisse}}{\text{Anzahl der möglichen Ergebnisse}} = \frac{|A|}{|\Omega|}$$

#### Beispiele
Wir messen den Glukosewert eines Patienten und klassifizieren ihn als diabetisch oder nicht-diabetisch. Wir kennen den Glukosewert nicht im Voraus, der Wert ist für uns zufällig. 
$$ \Omega = \{diabetes, nicht-diabetes\}$$
Dies ist kein Laplace Experiment. 

Wir werfen einen Würfel. Das Ergebnis oder der Ausgang des Würfelexperiments ist zufällig. 
$$\Omega = \{1,2,3,4,5,6\}$$

Für den Fall eines fairen Würfels handelt es sich um ein Laplace Experiment und die Wahrschienlichkeit für das Ereignis $W$ *die Zahl ist gerade* berechnet sich wie folgt
$$\mathbb{P}(W) = \frac{|\{2,4,6|}{|\{1,2,3,4,5,6\}|} = \frac{3}{6}$$
 

Wir werfen zwei faire Würfel. Hier ist ein Ergebnis ein Paar. Der Ergebnisraum ist
$$\Omega = \{(1,1), (1,2), \ldots, (6,5), (6,6)\}$$

Die Wahrschienlichkeit für das Ereignis $E$ *gefürfelte Summe > 7* berechnet sich wie folgt:
$$E = \{(2,6) (6,2) (3,5) (5,3) (3,6) (6,3) (4,4) (4,5) (5,4) (4,6) (6,4) (5,5) (5,6) (6,5) (6,6)\}$$
$$\mathbb{P}(Summe \gt 7) = \frac{|E|}{|\Omega|} = \frac{15}{36}$$

# Bedingte Wahrscheinlichkeit und das Theorem von Bayes
Wir betachten einen Ergebnisraum $\Omega$ und zwei Ereignisse $A$ und $B$. 

![Ergebnisraum](../assets/images/bayes_mengenab.png)

Dann gilt 

$$\mathbb{P}(A) = \frac{|A|}{|\Omega|} \text{,\;\;\;} \mathbb{P}(B) = \frac{|B|}{|\Omega|}$$

> #### Definition
> Die bedingte Wahrscheinlichkeit gibt die Wahrscheinlichkeit für Ereignis $A$ an, **gegeben dass** > Ereignis $B$ bereits eingetreten ist und ist definiert als
>
> $$\mathbb{P}(A | B) = \frac{\mathbb{P}(A \cap B)}{\mathbb{P}(B)} = \frac{\frac{A \cap B}{|\Omega|}}{\frac{|B|}{|\Omega|}} = \frac{|A \cap B|}{|B|}, \;\; B \ne \emptyset$$

Es gilt 

$$\mathbb{P}(A | B) = \frac{\mathbb{P}(A \cap B)}{\mathbb{P}(B)}$$

und aus Symmetrie

$$\mathbb{P}(B | A) = \frac{\mathbb{P}(B \cap A)}{\mathbb{P}(A)}$$

durch Einsetzen von $\mathbb{P}(A \cap B) = \mathbb{P}(B \cap A)$ erhalten wir das **Theorem vom Bayes**

$$\mathbb{P}(A | B) = \frac{\mathbb{P}(B|A) \; \mathbb{P}(A)}{\mathbb{P}(B)}$$

# Naive Bayes Classifier
Todo














