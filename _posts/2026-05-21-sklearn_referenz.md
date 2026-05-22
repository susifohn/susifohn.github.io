---
title: 8. Scikit-learn HOWTO
categories: [Machine Learning, Theorie ]
tags: [gibb, tsb, scikit, sklearn]     # TAG names should always be lowercase
math: true
---

# Scikit-learn – Kurzreferenz

> Scikit-learn folgt einer einheitlichen API, die für nahezu alle Modelle gleich aussieht.

---

## 1. Das Estimator-Interface

Jedes Modell hat denselben Aufbau: **Konstruktor → fit → predict**.

```python
from sklearn.linear_model import LogisticRegression

# 1. Konstruktor – Hyperparameter festlegen
model = LogisticRegression(C=1.0, max_iter=200)

# 2. fit() – Modell trainieren
model.fit(X_train, y_train)

# 3. predict() – Vorhersagen treffen
y_pred = model.predict(X_test)
```

> Tauscht man `LogisticRegression` gegen `RandomForestClassifier`, `SVC` oder ein anderes Modell aus,
> bleibt der Rest des Codes **unverändert**.

---

## 2. Wichtige Methoden im Überblick

| Methode            | Zweck                                      | Verfügbar bei                   |
|--------------------|--------------------------------------------|---------------------------------|
| `fit(X, y)`        | Modell trainieren                          | Alle Estimatoren                |
| `predict(X)`       | Klassen oder Werte vorhersagen             | Klassifikatoren, Regressoren    |
| `predict_proba(X)` | Klassenwahrscheinlichkeiten ausgeben       | Die meisten Klassifikatoren     |

---

## 3. Train/Test-Split

```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X, y,
    test_size=0.2,    # 20 % als Testmenge
    random_state=42,  # Reproduzierbarkeit
    stratify=y        # Klassenverteilung erhalten
)
```

---

## 4. Pipelines – Schritte verketten

```python
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression

pipe = Pipeline([
    ('scaler', StandardScaler()),
    ('modell', LogisticRegression())
])

pipe.fit(X_train, y_train)   # Scaler wird nur auf Trainingsdaten angepasst
pipe.predict(X_test)          # Skalierung + Vorhersage in einem Schritt
```

> Pipelines verhindern **Data Leakage**: Der Scaler „sieht" die Testdaten nie beim Trainieren.

---

## 5. Cross Validation 
Kreuzvalidierung

```python
from sklearn.model_selection import cross_val_score

scores = cross_val_score(model, X, y, cv=5, scoring='accuracy')

print(scores)         # z. B. [0.91, 0.89, 0.93, 0.90, 0.92]
print(scores.mean())  # Mittlere Genauigkeit
print(scores.std())   # Streuung
```

---

## 6. Hyperparameter-Suche

```python
from sklearn.model_selection import GridSearchCV

grid = GridSearchCV(
    LogisticRegression(),
    param_grid={'C': [0.01, 0.1, 1, 10]},
    cv=5,
    scoring='f1'
)
grid.fit(X_train, y_train)

print(grid.best_params_)     # z. B. {'C': 1}
print(grid.best_estimator_)  # Das beste Modell, direkt verwendbar
```

> `RandomizedSearchCV` funktioniert genauso, ist aber bei großen Suchräumen effizienter.

---

## 7. Vorverarbeitung (Transformer)

```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()
X_train_skaliert = scaler.fit_transform(X_train)  # Anpassen + transformieren
X_test_skaliert  = scaler.transform(X_test)        # Nur transformieren (kein Re-fit!)
```

**Häufig verwendete Transformer:**

| Transformer          | Zweck                              |
|----------------------|------------------------------------|
| `StandardScaler`     | Mittelwert 0, Standardabweichung 1 |
| `MinMaxScaler`       | Skalierung auf [0, 1]              |
| `OneHotEncoder`      | Kategorische Merkmale kodieren     |
| `SimpleImputer`      | Fehlende Werte auffüllen           |
| `PCA`                | Dimensionsreduktion                |
| `PolynomialFeatures` | Polynomiale Merkmale erzeugen      |

---

## 8. Gelernte Parameter inspizieren

Nach dem Training speichert scikit-learn alle gelernten Werte als Attribute mit einem **Unterstrich am Ende**:

```python
model.fit(X_train, y_train)

print(model.coef_)       # Gewichte (z. B. bei LogisticRegression)
print(scaler.mean_)      # Mittelwerte des Scalers
print(scaler.scale_)     # Standardabweichungen des Scalers
```

---

## 9. Vollständiges Beispiel

```python
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split, cross_val_score
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.ensemble import RandomForestClassifier

# Daten laden
X, y = load_iris(return_X_y=True)

# Aufteilen
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y
)

# Pipeline erstellen
pipe = Pipeline([
    ('scaler', StandardScaler()),
    ('modell', RandomForestClassifier(n_estimators=100, random_state=42))
])

# Trainieren & bewerten
pipe.fit(X_train, y_train)
print("Test-Accuracy:", pipe.score(X_test, y_test))

# Kreuzvalidierung
scores = cross_val_score(pipe, X, y, cv=5)
print(f"CV-Accuracy: {scores.mean():.3f} ± {scores.std():.3f}")
```

---

## 10. Designprinzipien

| Prinzip          | Bedeutung                                                                 |
|------------------|---------------------------------------------------------------------------|
| **Konsistenz**   | Alle Modelle haben `fit`, `predict`, `transform`                          |
| **Komposition**  | `Pipeline` verkettet beliebige Schritte                                   |
| **Inspektion**   | Gelernte Parameter als Attribute mit `_` (z. B. `coef_`, `mean_`)        |
| **Standardwerte**| Sinnvolle Defaults – nur anpassen, was nötig ist                          |
| **Duck Typing**  | Eigene Klassen funktionieren mit `GridSearchCV` etc., wenn die API stimmt |


---
$$
\text{T H E E N D}
$$
---