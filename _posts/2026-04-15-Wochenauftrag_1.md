---
title: Wochenauftrag 1
categories: [Machine Learning ]
tags: [gibb, tsb, exercises]     # TAG names should always be lowercase
math: true
---

# Wochenauftrag 1
- Start: Fr. 1. 5.
- Besprechung: Fr. 8. 5.


## Aufgabe 1
Ordne die folgenden Situationen dem passenden Anwendungsbereich des maschinellen Lernens zu.
Nehme das Bild in der Einführung für die Bereiche.

1. Es soll die benötigte Bewässerungsmenge in der Landwirtschaft unter verschiedenen Umweltbedingungen vorhergesagt werden. Ref: [Kaggle](https://www.kaggle.com/datasets/miadul/irrigation-water-requirement-prediction-dataset/data)

2. Ein Kamerasystem soll auf Bildern erkennen, ob es sich um eine Katze oder einen Hund handelt. 

3. Ein Kamerasystem soll in Echtzeit aus vielen Personengesichtern eine bestimmte Person erkennen.

4. Handgeschreibene Buchstaben und Zahlen sollen korrekt erkannt werden. 

5. Ein KI Roboter besiegt Tischtennis-Profis. Ref. [srf.ch](https://www.srf.ch/wissen/technik/mensch-gegen-maschine-wie-ein-ki-roboter-tischtennis-profis-besiegt)

6. In den sozialen Medien sollen Hashtags, welche von Usern kreiert werden, auf Gemeinsamkeiten analysiert werden um so unbekannte Trends festzustellen. 

7. Die Hausautomation in einem Gebäude zeichnet Daten von diversen Sensoren, darunter auch ein Feuchtigkeitsmesser,  alle 5s auf und speichert die Werte mit Zeitstempel in einem CSV-File, welches bis 5GB gross werden kann. Sie wollen herausfinden, in welchem Monat die grösste mittlere  Luftfeuchtigkeit herrschte.   

## Aufgabe 2
### a)
Überprüfe die folgenden Eigenschaften des Skalarproduktes $f$ mit $v,w \in \mathbb{R}^2$:

Bilinear: $f(\alpha v, \beta w)= \alpha \beta f(v,w)$ für $\alpha, \beta \in \mathbb{R}$

Kommutativ: $f(v, w) = f(w,v)$

$f(v,w) = 0 \implies  v,w$ stehen senkrecht aufeinander.

$f(v,v) = \|v\|^2_2$ , dabei ist $\|\cdot\|$ die euklidische Länge des Vektors. 

### b)
Berechne die Hypothese $h_\theta(x)$ für $\theta_1 = 2,\theta_2 = -2, \theta_3 = -1$ und die Features $x_1=650, x_2=3, x_3=12$ und Bias $b=-20$

## Aufgabe 3
### a)
Schreibe folgendes lineares Gleichungssystem in Matrixschreibweise und drücke $x$ mithilfe der inversen Matrix aus. Löse dann das System mit der ```numpy.linalg.inv``` Methode und zusätzlich auch mit ```numpy.linalg.solve```.

$$
\begin{cases}
2x + y = 5 \\
x - y = 1
\end{cases}
$$

### b) 
Finde ein lineares Gleichungssystem mit zwei Gleichungen und zwei Unbekannten, welches keine Lösung hat. Was ist in diesem Fall die Inverse der Matrix des Gleichungssystems? Ist die Matrix singular oder non-singuar? Zeige grafisch im $\mathbb{R}^2$, warum es keine Lösung gibt. 

## Einrichten der Entwicklungsumgebung
Auf [smartlearn-hfi.gibb.ch](https://smartlearn-hfi.gibb.ch) steht eine Windows Lernumgebung zur Verfügung, mit VS Code installiert. Selbstverständlich kannst Du auch auf deinem eigenen Rechner direkt arbeiten. VS Code ist eine gute Wahl. Du kannst auch ein Jupyter Notebook verwenden (Registrierung nötig)  mit z.B. 
- [anaconda.com](https://www.anaconda.com/docs/getting-started/anaconda/install/overview)
- [https://colab.research.google.com/](https://colab.research.google.com/)

Stelle sicher, dass du folgende Toolchain verfügbar hast:
1. python 3
2. pip3
2. numpy
3. matplotlib
4. scikit-learn
5. pandas

#### Check
Im Terminal in VS Code oder in PowerShell prüfe die Installation:

```bash
prompt>python --version
prompt>pip3 list
```
In einem  Jupyter Notebook, Run Cell:
```python
 import numpy, matplotlib, sklearn, pandas
print("Ready!")
```














