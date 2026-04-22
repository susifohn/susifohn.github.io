---
title: Lineare Algebra und ML
categories: [Machine Learning ]
tags: [gibb, tsb]     # TAG names should always be lowercase
math: true
---

## Inhaltsverzeichnis

- [Definition (Lineares Gleichungssystem)](#definition-lineares-gleichungssystem)
- [Beispiel: Polynom durch Datenpunkte](#beispiel-polynom-durch-datenpunkte)
- [Die Idee der inversen Matrix](#die-idee-der-inversen-matrix)
- [Polynomapproximation, Regression und Overfitting](#polynomapproximation-regression-und-overfitting)
- [Zusammenfassung](#zusammenfassung)


### Definition (Lineares Gleichungssystem)

Seien $m,n \in \mathbb{N}$.  
Ein *lineares Gleichungssystem* mit $m$ Gleichungen und $n$ Unbekannten besteht aus Gleichungen der Form

$$
\sum_{j=1}^{n} a_{ij} x_j = b_i 
\quad \text{für } i = 1,\dots,m,
$$

wobei $a_{ij}, b_i \in \mathbb{R}$ gegebene Zahlen sind und $x_1,\dots,x_n$ die Unbekannten bezeichnen. 

In Matrixschreibweise schreibt man das System kompakt als

$$
Ax = b,
$$

wobei $A \in \mathbb{R}^{m \times n}$ die Koeffizientenmatrix,  
$x \in \mathbb{R}^n$ der Vektor der Unbekannten und  
$b \in \mathbb{R}^m$ der bekannte Vektor auf der rechten Seite bezeichnen.

$a_{i,j}$ bezeichnet das Matrixelement in der $i$'ten Zeile und $j$'ten Spalte. Wobei $i,j \ge 1$. 

Eine *Lösung* ist ein Vektor $x \in \mathbb{R}^n$, der die Gleichung $Ax = b$ erfüllt.


## Beispiel: Polynom durch Datenpunkte

Viele praktische Probleme lassen sich als lineare Gleichungssysteme formulieren. Ein wichtiges Beispiel ist die Bestimmung eines Polynoms, das durch gegebene Datenpunkte verläuft.

Wir suchen ein Polynom dritten Grades

$$
p(x) = ax^3 + bx^2 + cx + d,
$$

das durch vier gegebene Punkte  
$(x_1, y_1), (x_2, y_2), (x_3, y_3), (x_4, y_4)$ verläuft.

Durch Einsetzen der $x_i$ entstehen vier Gleichungen für die unbekannten Koeffizienten $a, b, c, d$:

$$
\begin{aligned}
a x_1^3 + b x_1^2 + c x_1 + d &= y_1, \\
a x_2^3 + b x_2^2 + c x_2 + d &= y_2, \\
a x_3^3 + b x_3^2 + c x_3 + d &= y_3, \\
a x_4^3 + b x_4^2 + c x_4 + d &= y_4.
\end{aligned}
$$

Dieses System hat vier Gleichungen und vier Unbekannte und lässt sich in Matrixform schreiben als

$$
\begin{pmatrix}
x_1^3 & x_1^2 & x_1 & 1 \\
x_2^3 & x_2^2 & x_2 & 1 \\
x_3^3 & x_3^2 & x_3 & 1 \\
x_4^3 & x_4^2 & x_4 & 1
\end{pmatrix}
\begin{pmatrix}
a \\ b \\ c \\ d
\end{pmatrix}
=
\begin{pmatrix}
y_1 \\ y_2 \\ y_3 \\ y_4
\end{pmatrix}.
$$

Das Lösen solcher Systeme ist eine zentrale Aufgabe der linearen Algebra.  
Typische Verfahren sind das Gauß-Verfahren oder numerische Methoden. Im Folgenden betrachten wir die Lösung mit der Idee der inversen Matrix.


## Die Idee der inversen Matrix

Gegeben sei ein lineares Gleichungssystem

$$
A x = b, 
\qquad 
A \in \mathbb{R}^{n \times n}, 
\quad 
x,\, b \in \mathbb{R}^n.
$$

Gesucht ist ein Vektor $x$, sodass $Ax = b$ gilt.

Falls die Matrix $A$ invertierbar ist, existiert eine Matrix $A^{-1}$ mit

$$
A^{-1}A = AA^{-1} = I,
$$

wobei $I$ die Einheitsmatrix ist.

Multipliziert man die Gleichung $Ax = b$ von links mit $A^{-1}$, erhält man direkt

$$
x = A^{-1} b.
$$

Das bedeutet: Ist $A^{-1}$ bekannt, kann die Lösung unmittelbar berechnet werden.

**Wichtiger Hinweis:**  
Nicht jede quadratische Matrix ist invertierbar. Falls die Zeilen oder Spalten von $A$ linear abhängig sind, existiert keine Inverse und die Matrix ist *singulär*. In diesem Fall hat das Gleichungssystem entweder keine Lösung oder unendlich viele Lösungen.  Vektoren sind linear abhängig, falls ein Vektor durch eine Linearkombination der anderen dargestellt werden kann.

### Die Berechnung der inversen Matrix
nimmt uns die Funktion ```numpy.linalg.inv``` ab.

Um direkt das Gleichungssystem $Ax=b$ zu lösen, ist die Funktion ```numpy.linalg.solve``` effizienter. 

---

## Polynomapproximation, Regression und Overfitting

Das obige Beispiel führt zu grundlegenden Konzepten aus dem maschinellen Lernen: *Regression* und *Overfitting*.

In der Regression versucht man, aus Datenpunkten $(x_i, y_i)$ ein Modell zu bestimmen, das Zusammenhänge beschreibt und Vorhersagen ermöglicht.

Das Polynom

$$
p(x) = ax^3 + bx^2 + cx + d
$$

ist ein solches Modell. Die Koeffizienten $a, b, c, d$ nennt man *Parameter*, die aus den Daten bestimmt werden.

Im obigen Beispiel wird gefordert, dass das Polynom **alle Punkte exakt trifft**. Man spricht von *exakter Anpassung*.

In der Praxis ist eine exakte Anpassung nicht anwendbar, weil

- Reale Daten enthalten meist Rauschen (Messfehler, Zufallseinflüsse).
- Ein zu komplexes Modell kann dieses Rauschen „mitlernen“.

Dieses Phänomen nennt man *Overfitting*:  
Das Modell passt perfekt zu den Trainingsdaten, liefert aber schlechte Vorhersagen für neue Daten.


## Zusammenfassung

- Lineare Gleichungssysteme ermöglichen die Bestimmung von Modellparametern.  
- Regression bedeutet, solche Parameter aus Daten zu bestimmen.  
- Overfitting entsteht, wenn ein Modell zu komplex im Verhältnis zur Datenmenge ist.

Im nächsten Abschnitt über **Least-Means-Squares (LMS) und die Normalengleichungen** betrachten wir Anwendungen der linearen Algebra für maschinelles Lernen. 

$$\text Viel \: Spass!$$