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
Hast du dich schon mal gewundert, wie eine E-Mail als Spam markiert wird? Einer der klassischen Ansätze in den frühen Spamfiltern bestand darin, mit Wahrscheinlichkeiten zu arbeiten - insbesondere mit dem dem *Bayes' Theorem*. 

Dieser sehr naive Ansatz, wie wir noch sehen werden, funktioniert für viele Klassifizierungsprobleme überraschend gut, so auch zum Klassifizieren von E-Mails. 

![Spam](../assets/images/spam.png)

Um den sogenannten *Naive Bayes Classifier* zu verstehen und auf eigene Problemstellungen anwenden zu können, benötigen wir zunächst einige Grundbegriffe aus der Wahrscheinlichkeitsrechnung. 

# Grundbegriffe der Wahrscheinlichkeitsrechnung

#### Definitionen
Ein *Zufallsexperiment* ist ein Vorgang, dessen Ergebnis nicht vorhergesagt werden kann, dessen mögliche Ergebnisse jedoch bekannt sind.

Die Menge aller möglichen Ergebnisse eines Zufallsexperiments heißt *Ergebnisraum* (*sample space*) und bezeichnen wir mit $\Omega$. 

Ein *Ereignis* (*event*) ist eine Teilmenge des Ergebnisraums. Ein Ereignis mit genau einem Ergebnis, wird  *Elementarereignis* genannt. 

*Laplace-Experimente* sind Zufallsexperimente bei denen jedes Elementarereignis die gleiche Wahrscheinlichkeit besitzt. 

Die *Wahrscheinlichkeit* eines Ereignisses $A \subseteq \Omega$ ist eine Zahl zwischen $0$ und $1$ und berechnet sich bei Laplace-Experimenten als
$$\mathbb{P}(A) = \frac{\text{Anzahl der günstigen Ergebnisse}}{\text{Anzahl der möglichen Ergebnisse}} = \frac{|A|}{|\Omega|}$$

**Beispiel 1:** 
Wir messen den Glukosewert eines Patienten und klassifizieren ihn als diabetisch oder nicht-diabetisch. Da wir den Glukosewert nicht im Voraus kennen, ist das Ergebnis für uns zufällig. 
$$ \Omega = \{\text{diabetisch}, \text{nicht-diabetisch}\}$$
Dies ist kein Laplace-Experiment, da die beiden Ergebnisse nicht gleich wahrscheinlich sind. 

**Beispiel 2:**
Wir werfen einen fairen Würfel. Das Ergebnis des Würfelexperiments ist zufällig. Die Augenzahlen auf den sechs Seiten sind die Elementarereignisse und alle gleich wahrscheinlich. Somit handelt es sich um ein Laplace-Experiment.
$$\Omega = \{1,2,3,4,5,6\}$$

Die Wahrscheinlichkeit für das Ereignis $E$ dass die gewürfelte Zahl gerade ist $\{2,4,6\} \subseteq \{1,2,3,4,5,6\}$ berechnet sich wie folgt:
$$\mathbb{P}(E) = \frac{|\{2,4,6\}|}{|\{1,2,3,4,5,6\}|} = \frac{1}{2}$$

- Zufallsexperiment: Ein Würfelwurf
- Ergebnisraum: $\Omega=\{1,2,3,4,5,6\}$
- Elementarereignis: die Mengen $\{1\}...\{6\}$
- Ereignis: $\{2,4,6\}$ "gerade Zahl"
 
**Beispiel 3:**
Wir werfen zwei faire Würfel. Ein Ergebnis ist nun ein Zahlenpaar. Der Ergebnisraum besteht aus 36 gleich wahrscheinlichen Elementarereignissen.
$$\Omega = \{(1,1), (1,2), \ldots, (6,5), (6,6)\}$$

Die Wahrscheinlichkeit für das Ereignis $E=\{\text{gewürfelte Summe > 7\}}$ lässt sich berechnen, indem alle günstigen Kombinationen gezählt werden.
$$E = \{(2,6) (6,2) (3,5) (5,3) (3,6) (6,3) (4,4) (4,5) (5,4) (4,6) (6,4) (5,5) (5,6) (6,5) (6,6)\}$$
$$\mathbb{P}(Summe \gt 7) = \frac{|E|}{|\Omega|} = \frac{15}{36} \approx 0.4167$$

# Bedingte Wahrscheinlichkeit und das Theorem von Bayes

Wir betrachten einen Ergebnisraum $\Omega$ sowie zwei Ereignisse $A$ und $B$. 

![Ergebnisraum](../assets/images/bayes_mengenab.png)

Dann gilt 

$$\mathbb{P}(A) = \frac{|A|}{|\Omega|} ,\;\;\; \mathbb{P}(B) = \frac{|B|}{|\Omega|}$$

> #### Definition
> Die bedingte Wahrscheinlichkeit beschreibt die Wahrscheinlichkeit eines Ereignisses $A$, **gegeben dass** ein anderes Ereignis $B$ bereits eingetreten. Sie ist für $B \ne \emptyset$ definiert als
>
> $$\mathbb{P}(A | B) = \frac{\mathbb{P}(A \cap B)}{\mathbb{P}(B)} = \frac{\frac{A \cap B}{|\Omega|}}{\frac{|B|}{|\Omega|}} = \frac{|A \cap B|}{|B|}$$

Es gilt ausserdem aufgrund der Symmetrie

$$\mathbb{P}(B | A) = \frac{\mathbb{P}(B \cap A)}{\mathbb{P}(A)}$$

durch Gleichsetzen von $\mathbb{P}(A \cap B) = \mathbb{P}(B \cap A)$ erhalten wir das **Theorem vom Bayes**

$$\mathbb{P}(A | B) = \frac{\mathbb{P}(B|A) \; \mathbb{P}(A)}{\mathbb{P}(B)}$$

## Totale Wahrscheinlichkeit
Der Satz der totalen Wahrscheinlichkeit ermöglicht es, die Einzelwahrscheinlichkeiten aus den bedingten Wahrscheinlichkeiten zu berechnen. 

Mit einer Partition von $\Omega = B \cup \bar B$ gilt

$$P(A) = P(A|B) \cdot P(B) + P(A| \bar B)\cdot P(\bar B) $$

Das gilt analog für eine beliebige Anzahl Partitionen, hier nur zwei mit $B$ und dem Komplement $\bar B = \Omega \setminus B$. 

Die Herleitung ist mit der Definition der bedingten Wahrscheinlichkeit und der Einsicht, dass $A = (A \cap B) \cup (A \cap \bar B)$ einfach. 

## Unabhängigkeit
Der Naive Bayes Classifier basiert auf der Annahme der Unabhängigkeit, wie wir gleich sehen werden und welche wir noch definieren müssen.

Die Ereignisse $A$ und $B$ sind voneinander unabhängig wenn gilt
$$P(A) \cdot P(B) = P(A\cap B)$$

Daraus folgt, weil $P(A \cap B) = P(A,B)$ = Wahrscheinlichkeit, dass $A$ **und** $B$ eintreten

$$\mathbb{P}(A,B|D) = \mathbb{P}(A|D)\mathbb{P}(B|D)$$

# Naive Bayes Classifier
## Recap
Einige Begriffe, welche im Zusammenhang des maschinellen Lernens oft anzutreffen sind, werden nachfolgend erklärt.

- **P(A|B):**(Posterior) Das ist die Wahrscheinlichkeit, dass Ereignis **A** eintritt, **gegeben dass B eingetreten ist**.  
Wenn wir wissen, dass **B** bereits passiert ist (z. B. die E-Mail enthält das Wort „buy“), möchten wir bestimmen, wie wahrscheinlich es ist, dass **A** wahr ist (also dass die E-Mail Spam ist).

- **P(B|A):**(Likelihood) Das ist die Wahrscheinlichkeit, dass **B** eintritt, **wenn A bereits eingetreten ist**.  
  Wenn wir also wissen, dass die E-Mail Spam ist (**A**), dann sagt uns diese Wahrscheinlichkeit, wie wahrscheinlich es ist, dass sie das Wort „buy“ (**B**) enthält.

- **P(A):**(Prior) Das ist die **a-priori-Wahrscheinlichkeit** von **A**.  
  Sie beschreibt, wie wahrscheinlich es ist, dass eine E-Mail Spam ist, **bevor** wir bestimmte Merkmale wie **B** betrachten.

- **P(B):** (Prior/Evidence)Das ist die **a-priori-Wahrscheinlichkeit** von **B**.  
  Sie beschreibt, wie wahrscheinlich es ist, das Merkmal **B** allgemein zu beobachten (also z. B. das Wort „offer“), **unabhängig davon**, ob **A** eintritt oder nicht.

## Preprocessing the Emails
Es gibt diverse Fragestellungen bei der Verarbeitung von Text. Z.B. Umgang mit Rechtschreibung, Slang etc. Ein Email soll eine Liste aus Wörtern sein, mit folgendem Ansatz:

- Wortwiederholungen ignorieren
- Alle Grossbuchstaben umwandeln in Kleinbuchstaben
- Alle nicht-Buchstaben ignorieren. D.h. keine Satzzeichen und Zahlen.

Zum Beispiel wird das Email ```"You buy 99 Viagra!!!"``` zu ```['you', 'buy', 'viagra']``` 

## Bayes Theorem naiv anwenden
Wir wollen folgende Klassifikation beantworten: Ist das Email "You buy 99 Viagra!!!" spam=1 oder no-spam=0. 

Dazu betrachten wir die Wahrscheinlichkeit 
$$\mathbb{P}(\text{spam}| \{you,buy ,viagra\})$$
und
$$\mathbb{P}(\text{no-spam}| \{you, buy, viagra\})$$

Was für ersteres soviel heisst wie, gegeben das preprozessierte E-Mail {you, buy, viagra} ist eingetroffen, was ist die Wahrschienlichkeit dass es spam ist.  

Die höhere Wahrscheinlichkeit gibt uns die Klasse an. Anwenden von Bayes gibt

$$\mathbb{P}(\text{spam}| \{you, buy, viagra\})= \frac{\mathbb{P}(\{you, buy, viagra\})|spam)\mathbb{P}(spam)}{\mathbb{P}(\{you, buy, viagra\})|spam)\mathbb{P}(spam)+\mathbb{P}(\{you, buy, viagra\})|no-spam)\mathbb{P}(no-spam)}\\$$

Unsere naive Annahme ist, dass die Wörter unabhängig sind, wenn die Klasse vorgegeben ist. Offensichtlich ist das nicht ganz korrekt, denn die Sprache hat eine Struktur. Der naive Ansatz liefert jedoch in vielen Anwendungen sehr gute Resultate. Sind also die Ereignisse $A$, $B$ , $C$ unabhängig, gilt
$$\mathbb{P}(A,B,C|S) = \mathbb{P}(A|S)\mathbb{P}(B|S)\mathbb{P}(C|S)$$

Damit die Formel übersichtlicher wird, schreiben wir nur den Anfangsbuchstaben der Wörter und erhalten folgende gesuchte Wahrscheinlichkeit:

$$P(s|y,b,v) = \frac{P(y|s)P(b|s)P(v|s)P(s)}{P(y|s)P(b|s)P(v|s)P(s)+P(y|\bar s)P(b|\bar s)P(v|\bar s)P(\bar s)}$$

## Von bekannten Daten lernen
Um die diversen Wahrscheinlichkeite im Ausdruck oben zu bestimmen, schauen wir unsere Trainingsdaten an.

|preprocessed Email|Label|
|:---|---|
|buy help| spam|
|you good|no-spam|
|viagra help you|spam|
|good viagra help|spam|
|need to buy viagra for health|no-spam|
|you buy viagra|?|

Die Wahrscheinlichkeit ein Spam-Mail zu erhalten ist 
$$P(s) = \frac{3}{5}$$
und für ein no-spam
$$P(\bar s) = \frac{2}{5}$$

Um $P(word|spam)$ zu bestimmen betrachten wir alle Spam-Emails welche das Wort beinhalten und teilen durch die Anzahl Spam-Emails. Und tun dasselbe für Wörter in den No-Spam-Emails. So erhalten wir alle gesuchten Wahrscheinlichkeiten wie folgt:
- $P(y|s)=\frac{1}{3}$
- $P(b|s)=\frac{1}{3}$
- $P(v|s)=\frac{2}{3}$
- $P(y|\bar s)=\frac{1}{2}$
- $P(b|\bar s)=\frac{1}{2}$
- $P(v|\bar s)=\frac{1}{2}$

und können diese in die Formel oben einsetzen

$$P(s|y,b,v) = \frac{\frac{1}{3}\frac{1}{3}\frac{2}{3}\frac{3}{5}}{\frac{1}{3}\frac{1}{3}\frac{2}{3}\frac{3}{5}+\frac{1}{2}\frac{1}{2}\frac{1}{2}\frac{2}{5}} \approx 0.470588$$

und 
$$P(\bar s|y,b,v) = 1-P(s|y,b,v) \approx 0.529412$$

Somit klassifizieren wir das Email "You buy Viagra!!!" als No-Spam. 

### Hinweis
Die Trainingsdaten im Beispiel sind absichtlich so gewählt, dass keine bedingte Wahrscheinlichkeit 0 oder 1 ist. Das kann z.B. passieren, wenn "buy" nicht vorkommt in allen No-Spam-Emails, dann ist $P(b|\bar s)=\frac{0}{2}$, was den Klassifizierer instabil macht. Das Problem wird mit **Laplace Smoothing** behoben. Der [sklearn.naive_bayes.BernoulliNB](https://scikit-learn.org/stable/modules/generated/sklearn.naive_bayes.BernoulliNB.html#sklearn.naive_bayes.BernoulliNB) Klassifizierer verwendet Standardmässig Laplace Smoothing, mit $\alpha =1$ und $n=2$ weil jedes Feature (=Wörter) binär ist, als present 1 oder nicht-present 0. Z.B. $P(buy|spam) = \frac{1+\alpha}{3+n\alpha}=\frac{1+1}{3+2}$. Mit dem [sklearn.naive_bayes.MultinomialNB](https://scikit-learn.org/stable/modules/generated/sklearn.naive_bayes.MultinomialNB.html#sklearn.naive_bayes.MultinomialNB) Klassifier ist $n=$ # Wörter. 

Hier haben wir diskrete Features betrachtet. Der Naive Bayes Klassifzierer kann auch für reelle Features eingesetzt werden, siehe [sklearn.naive_bayes.GaussianNB](https://scikit-learn.org/stable/modules/generated/sklearn.naive_bayes.GaussianNB.html#sklearn.naive_bayes.GaussianNB). Das wäre z.B. der Fall bei Features {Breite, Länge} beim Bestimmen von Insekten als Raupe oder Käfer.












