---
title: Wochenauftrag 1a
categories: [Machine Learning ]
tags: [gibb, tsb, exercises]     # TAG names should always be lowercase
math: true
---

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
Für
$$ u=
\begin{pmatrix}
1 \\
2 \\
-3
\end{pmatrix}
\text{und} \; v=
\begin{pmatrix}
10 \\
0 \\
-2
\end{pmatrix}
$$

berechne

$$
\begin{aligned}
u^T v \\
v^T v \\
u (v^Tv)
\end{aligned}
$$

**Freiwillig:** kannstu du $u v^T$ berechnen? Hierzu musst du herausfinden, wie Matrizen multipliziert werden. 

Gegeben ist folgende Hypothese:
$$
h_w(x) = 3x + y - z + \frac{1}{2} 
$$
Schreibe $h$ als Skalarprodukt. 

### b)
Überprüfe die folgenden Eigenschaften des Skalarproduktes $f$ mit $v,w \in \mathbb{R}^2$:

Bilinear: $f(\alpha v, \beta w)= \alpha \beta f(v,w)$ für $\alpha, \beta \in \mathbb{R}$

Kommutativ: $f(v, w) = f(w,v)$

$f(v,w) = 0 \implies  v,w$ stehen senkrecht aufeinander.

$f(v,v) = \|v\|^2_2$ , dabei ist $\|\cdot\|$ die euklidische Länge des Vektors. 

### c)
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

## Aufgabe 4
$A,B,C \in \mathbb{R}^{n \times n}$ invertierbar, $x,y \in \mathbb{R}^n$. Kannst du folgende Gleichung nach $x$ auflösen? Beachte, dass die Matrixmultiplikation nicht kommutativ ist, d.h. allgemein gilt $AB \neq BA$. 

$$
ABxC^{-1} = y
$$

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
```py linenums="1"
import numpy, matplotlib, sklearn, pandas
print("Ready!")
```
In VS Code benötigst du noch eine Graph-Library wie ```tkinter```, welche installiert werden muss, damit du Fenster mit mathplotlib anzeigen kannst.  

## Aufgabe 5

 Gegeben sind folgende Messwerte einer Wetterstation:

 | Temperatur $x$ (°C) | Glaceverkauf $y$ (Portionen) |
 |:---:|:---:|
 | 15 | 30 |
 | 20 | 50 |
 | 25 | 80 |
 | 30 | 110 |
 | 35 | 120 |

 1. Visualisiere die Daten mit matplotlib.
 2. Nehme als Hypothese eine Gerade und bestimme die Gewichte mit ```np.linalg.inv``` und ```np.linalg.lstsq``` 
 3. zeichne diese ein.
 3. Wie viele Glaceportionen werden bei 28°C verkauft?
 4. Ist das Modell bei 5°C noch sinnvoll?













