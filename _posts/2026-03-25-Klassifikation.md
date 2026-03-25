---
title: Klassifikation in ML
categories: [Machine Learning ]
tags: [gibb, tsb]     # TAG names should always be lowercase
math: true
---

# Klassifikation
*Ref: Neuronale Netze, Tariq R. O'REILLY*

## Mit Trennlinie

Hier haben wir zwei Klassen, nämlich {Rupen, Käfer} und die Features {Länge, Breite}.
Die Lineare Regression ist hier nicht nützlich. 

![Klassification](../assets/images/Insect_classes1.png)

Vielmehr suchen wir eine Trennlinie, welche es uns erlaubt, ein unbakanntes Insekt zu klassifitzieren. Die Trennlinie wird dabei anhand der Trainingsdaten gelernt. 

![Klassification](../assets/images/Insect_classes2.png)


> #### Frage
> Warum kann eine optimale Trennlinie im allgemeinen nicht als $f(x)=ax+b$ definiert werden?

>#### Übung

>Benutze *python* und *matplotlib* um die zwei Datenpunkte in einem Diagramm zu visualisieren. 

>| Trainingsdaten | B(Breite) | L (Länge) | BxL | Punktfarbe
>|---|---:|---:|---|---|
>| Marienkäfer | 3.0 | 1.0 | 3.0 × 1.0 |grün|
>| Raupe | 1.0 | 3.0 | 1.0 × 3.0 |rot|

>Zeichne eine beliebige Gerade ein, in blau, welche die beiden Punkte separiert. 
>Beschrifte die Achsen und füge eine Legende sowie einen Titel hinzu.

Lösung zur Übung
```python
import matplotlib.pyplot as plt

# Data points
x = [1, 3]
y = [3, 1]

# Plot each point separately so they can appear in the legend
plt.scatter(1, 3, color="red", label="Raupe")
plt.scatter(3, 1, color="green", label="Marienkäfer")

# Labels and title
plt.xlabel("X")
plt.ylabel("Y")
plt.title("Scatter Plot of Two Points")

# Add legend
plt.legend()

# Show plot
plt.show()
```
Antwort zur Frage: Eine Trennlinie kann auch senkrecht sein, das wäre aber eine lineare Funktion mit Steigung $\infty$. 


## Mit Aktivierungsfunkion

Haben wir nur ein Feature und eine Klassifizierung Raupen=0 und Käfer=1, können wir das wie folgt visialisieren. 

![Klassification](../assets/images/Insect_classes3.png)

Eine Funktion $f:\mathbb{R} \rightarrow \{0,1\}$ welche ab einer bestimmten Breite $b_R$ von $0$ auf $1$ springt

$$f(x) = \begin{cases}
0 & ,x \le b_R \\
1 &  ,sonst 
\end{cases}$$

kann zur Klassifizierung gelernt werden. Geeigneter ist herfür die *Sigmoidfunktion*, welche nicht sprungartig ändert und beobachtetes verhalten realstischer abbildet - *natura non facit saltus* (die Natur macht keine Sprünge). Die Sigmoidfunktion, welche auch als *logistische Funktion* bezeichnet wird, ist wie folgt definiert:
$$y(x) = \frac{1}{1+e^{-x}}$$

![Klassification](../assets/images/Insect_classes4.png)


```python
import numpy as np
import matplotlib.pyplot as plt

# Threshold
b = 2.0

# X range for plotting functions
x = np.linspace(-1, 5, 500)

# Step function: 0 if x < b, else 1
step = np.where(x < b, 0, 1)

# Sigmoid function centered at b
k = 5  # steepness of sigmoid
sigmoid = 1 / (1 + np.exp(-k * (x - b)))

# Example class 0 and class 1 points
x_class0 = [0.2, 0.8, 1.4, 1.8]
y_class0 = [0] * len(x_class0)

x_class1 = [2.2, 2.8, 3.5, 4.2]
y_class1 = [1] * len(x_class1)

# Plot step function
plt.step(x, step, where='post', label='Stufen-Fkt', linewidth=2)

# Plot sigmoid
plt.plot(x, sigmoid, label='Sigmoid-Fkt', linewidth=2)

# Plot points
plt.scatter(x_class0, y_class0, color='red', s=80, label='Raupen = 0')
plt.scatter(x_class1, y_class1, color='green', s=80, label='Käfer = 1')

# Threshold line
plt.axvline(b, linestyle='--', label=f'Threshold b = {b}')

# Labels and title
plt.xlabel("Breite x")
plt.ylabel("Klasse")
plt.title("Stufen- und Sigmoidfunktion zur Klassifizierung")

# Limits and grid
plt.ylim(-0.1, 1.1)
plt.grid(True, alpha=0.3)

# Legend
plt.legend()

# Show plot
plt.show()
```

Du kannst drei Personen auf 5 mögliche Arten gruppieren. Was denkst du, wieviele Möglichkeiten gibt es mit deiner Klasse? Es sind vermutlich mehr als du denkst. Dieses Problem ist nur rekursiv lösbar! Findest du eine explizite Formel, wirst du sehr berühmt. 

