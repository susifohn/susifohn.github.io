---
title: Wochenauftrag 1
categories: [Machine Learning ]
tags: [gibb, tsb, exercises]     # TAG names should always be lowercase
math: true
---

# Wochenauftrag 1
- Start: Fr. 1. 5.
- Besprechung: Fr. 8. 5.

## Übung 0 - Einrichten der Entwicklungsumgebung
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
## Übung 1
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














