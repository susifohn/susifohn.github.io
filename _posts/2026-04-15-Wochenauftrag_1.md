---
title: Wochenauftrag 1
categories: [Machine Learning ]
tags: [gibb, tsb, exercises]     # TAG names should always be lowercase
math: true
---

# Wochenauftrag 1
- Start: Fr. 1. 5.
- Besprechung: Fr. 8. 5.

## Auftrag 1 - Einrichten der Entwicklungsumgebung
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

## Aufgabe 1
Ordne die folgenden Situationen dem passenden Anwendungsbereich des maschinellen Lernens zu.
Nehme das Bild in der Einführung für die Bereiche.

1. Es soll die benötigte Bewässerungsmenge in der Landwirtschaft unter verschiedenen Umweltbedingungen vorhergesagt werden. Ref: [Kaggle](https://www.kaggle.com/datasets/miadul/irrigation-water-requirement-prediction-dataset/data)

2. Ein Kamerasystem soll auf Bildern erkennen, ob es sich um eine Katze oder einen Hund handelt. 

3. Ein Kamerasystem soll in Echtzeit aus vielen Personengesichtern eine bestimmte Person erkennen.

4. Handgeschreibene Buchstaben und Zahlen sollen korrekt erkannt werden. 

5. Ein KI Roboter besiegt Tischtennis-Profis. Ref. [srf.ch](https://www.srf.ch/wissen/technik/mensch-gegen-maschine-wie-ein-ki-roboter-tischtennis-profis-besiegt)

6. In den sozialen Medien sollen Hashtags, welche von Usern kreiert werden, auf Gemeinsamkeiten analysiert werden um so unbekannte Trends festzustellen. 


## Aufgabe 2
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














