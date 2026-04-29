---
title: 3b. Lineare Regression – Beispiel
categories: [Machine Learning, Lineare Regression]
tags: [gibb, tsb, lms, gradient descent]
math: true
---

# Lineare Regression – Ein vollständiges Beispiel

Wir wollen den **Preis eines Hauses** anhand seiner **Grösse** vorhersagen.

## Die Daten

| Grösse $x$ (100 m²) | Preis $y$ (1000 CHF) |
|:---:|:---:|
| 1 | 2 |
| 2 | 4 |
| 3 | 5 |
| 4 | 4 |
| 5 | 5 |

Ein Freund besitzt ein Haus mit 350 m². Was ist es wert?

## Das Modell

Wir suchen eine Gerade

$$\hat{y} = \theta_1 x + \theta_0$$

Die **Parameter** $\theta_1$ (Steigung) und $\theta_0$ (y-Achsenabschnitt) sind unbekannt – sie sollen *gelernt* werden.

## Die Fehlerfunktion (Loss Function)

Für jeden Datenpunkt berechnen wir den Fehler zwischen Vorhersage $\hat{y}_i$ und dem tatsächlichen Wert $y_i$.

$$J(\theta_1, \theta_0) = \frac{1}{n} \sum_{i=1}^{n} (\hat{y}_i - y_i)^2$$

> #### Warum quadrieren?
> - Die einfache Differenz $\hat{y} - y$ kann sich aufheben (positive und negative Fehler).
> - Der Absolutbetrag $|\hat{y} - y|$ ist am Minimum nicht differenzierbar.
> - Das Quadrat $(\hat{y} - y)^2$ ist immer positiv und überall differenzierbar. ✓

## Lösung 1 – Normalengleichung (exakter Weg)

Die Normalengleichung liefert die optimalen Parameter **direkt** in einem Schritt:

$$\theta = (A^\top A)^{-1} A^\top y$$

```python
import numpy as np
import matplotlib.pyplot as plt

# Trainingsdaten
x = np.array([1.0, 2.0, 3.0, 4.0, 5.0])
y = np.array([2.0, 4.0, 5.0, 4.0, 5.0])

# Matrix A aufbauen: [x, 1] für jede Zeile
A = np.column_stack([x, np.ones(len(x))])
print("Matrix A:\n", A)

# Normalengleichung: θ = (AᵀA)⁻¹ Aᵀy
theta = np.linalg.inv(A.T @ A) @ A.T @ y
print(f"θ₁ = {theta[0]:.3f}, θ₀ = {theta[1]:.3f}")

# Vorhersage für 3.5 (= 350 m²)
x_neu = 3.5
y_pred = theta[0] * x_neu + theta[1]
print(f"Vorhersage für {x_neu*100:.0f} m²: {y_pred:.1f} × 1000 CHF")

# Plot
x_plot = np.linspace(0.5, 5.5, 100)
y_plot = theta[0] * x_plot + theta[1]

plt.scatter(x, y, color='steelblue', s=80, label='Trainingsdaten')
plt.plot(x_plot, y_plot, color='tomato', label='Regressionsgerade')
plt.scatter(x_neu, y_pred, color='green', s=120, zorder=5,
            label=f'Vorhersage: {y_pred:.1f}k CHF')
plt.xlabel('Grösse (100 m²)')
plt.ylabel('Preis (1000 CHF)')
plt.title('Lineare Regression – Hauspreis')
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()
```

## Lösung 2 – Gradientenabstieg (iterativer Weg)

Statt einer direkten Formel tasten wir uns **schrittweise** zum Minimum der Fehlerfunktion vor.

```python
import numpy as np
import matplotlib.pyplot as plt

x = np.array([1.0, 2.0, 3.0, 4.0, 5.0])
y = np.array([2.0, 4.0, 5.0, 4.0, 5.0])

# Startwerte (zufällig / schlecht)
theta1 = 0.0
theta0 = 0.0
alpha  = 0.02   # Lernrate
n      = len(x)

losses = []

for step in range(200):
    y_hat  = theta1 * x + theta0       # Vorhersagen
    error  = y_hat - y                 # Fehler

    # Ableitungen (einmal von Hand bestimmt, dann für immer benutzt)
    grad1 = (2/n) * np.sum(error * x)
    grad0 = (2/n) * np.sum(error)

    # Parameter aktualisieren
    theta1 = theta1 - alpha * grad1
    theta0 = theta0 - alpha * grad0

    losses.append(np.mean(error**2))

print(f"θ₁ = {theta1:.3f}, θ₀ = {theta0:.3f}")

# Verlauf der Fehlerfunktion
plt.plot(losses)
plt.xlabel('Schritt')
plt.ylabel('Fehler J')
plt.title('Gradientenabstieg – Fehler nimmt ab')
plt.grid(True, alpha=0.3)
plt.show()
```

> #### Hinweis zur Lernrate $\alpha$
> - $\alpha$ zu **gross** → Fehler springt hin und her, konvergiert nicht
> - $\alpha$ zu **klein** → Fehler nimmt ab, aber sehr langsam
> - Probiere $\alpha \in \{0.2, 0.05, 0.001\}$ aus und beobachte den Unterschied!

## Lösung 3 – scikit-learn (professioneller Weg)

```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error

x = np.array([1.0, 2.0, 3.0, 4.0, 5.0]).reshape(-1, 1)  # shape (5,1)
y = np.array([2.0, 4.0, 5.0, 4.0, 5.0])

# Schritt 1: Modell erstellen
model = LinearRegression()

# Schritt 2: Trainieren
model.fit(x, y)

# Schritt 3: Vorhersagen
y_pred = model.predict(x)

print(f"θ₁ = {model.coef_[0]:.3f}")
print(f"θ₀ = {model.intercept_:.3f}")
print(f"MSE = {mean_squared_error(y, y_pred):.4f}")

# Vorhersage für neues Haus
print(f"Vorhersage 350 m²: {model.predict([[3.5]])[0]:.1f}k CHF")
```

> Das Muster **Erstellen → Trainieren → Vorhersagen** ist bei *jedem* scikit-learn Modell gleich. Es lohnt sich, es auswendig zu kennen.

## Vergleich der drei Methoden

| Methode | Wie | Wann sinnvoll |
|---|---|---|
| Normalengleichung | Exakte Formel, ein Schritt | Kleine Datensätze |
| Gradientenabstieg | Iterativ, viele Schritte | Grosse Datensätze, neuronale Netze |
| scikit-learn | Intern optimiert | Immer in der Praxis |

Alle drei liefern (nahezu) **identische Resultate** – das ist kein Zufall.

---

## Übung

> #### Aufgabe
>
> Gegeben sind folgende Messwerte einer Wetterstation:
>
> | Temperatur $x$ (°C) | Eisverkauf $y$ (Portionen) |
> |:---:|:---:|
> | 15 | 30 |
> | 20 | 50 |
> | 25 | 80 |
> | 30 | 110 |
> | 35 | 120 |
>
> 1. Visualisiere die Daten mit matplotlib.
> 2. Bestimme die Regressionsgerade mit scikit-learn.
> 3. Wie viele Portionen werden bei 28°C verkauft?
> 4. Ist das Modell bei 5°C noch sinnvoll? Begründe.
> 5. **Optional, später** Vergleiche Normalengleichung und scikit-learn – ergibt sich dasselbe $\theta$?
