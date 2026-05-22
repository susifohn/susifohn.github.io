---
title: Wochenauftrag 3
categories: [Machine Learning, Übungen ]
tags: [gibb, tsb, exercises, titanic]     # TAG names should always be lowercase
math: true
---

- Start: Fr. 22. 5.
- Besprechung: Fr. 29. 5.



## Aufgabe 1

In unserem Schulhaus soll automatisch erkannt werden, ob ein Raum ein **Klassenzimmer** ist oder nicht.

Dazu wird nur ein einziges Feature verwendet: Anzahl der Stühle im Raum.


- **1 = Klassenzimmer**
- **0 = kein Klassenzimmer (z.B. Büro, Lagerraum, Besprechungsraum)**


### Trainingsdaten

| Stühle | Klasse |
|---|---|
| 12 | 0 |
| 5 | 0 |
| 18 | 1 |
| 20 | 1 |
| 22 | 1 |
| 24 | 1 |
| 26 | 1 |
| 2 | 0 |
| 6 | 0 |
| 13 | 0 |

---

### Versuche folgende Fragen zu beantworten

Verwende [sklearn.linear_model.LogisticRegression](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html)


1. Wie hoch ist die Wahrscheinlichkeit für ein Klassenzimmer bei:
   - 17 Stühlen
   - 29 Stühlen
2. Bestimme die Gewichte und Zeichne die Aktivierungsfunktion. 
3. Bei welcher Anzahl Stühlen kippt die Klasse von 0 auf 1.

## Aufgabe 2
Benutze ```class sklearn.linear_model.LinearRegression``` um ein Polynom vom Grad 1 und vom Grad 5 durch folgende Datenpunkte (x,y) zu legen: (1,0), (2,0), (4,0), (8,1), (10,1), (11,1), (21,1). 

1. Erstelle ein Diagramm mit den Punkten und den zwei Polynomen. 
2. Warum ist lineare Regression nicht geeignet für Klassifikation?

## Aufgabe 3 - Titanic
In dieser Aufgabe betrachten wir eine Anwendung aus der Praxis. Wir haben nicht alle Aspekte betrachtet, die vorkommen. Versuche die Pipeline zu verstehen. 

Anwendung: Logistische Regression mit realen Daten und Testing. 

> Datensatz: [Kaggle Titanic Dataset](https://www.kaggle.com/datasets/yasserh/titanic-dataset)
> Ziel: Vorhersagen, ob ein Passagier überlebt hat (`Survived`: 0 = Nein, 1 = Ja)

---

### Ziel

- Einen echten Datensatz laden und verstehen
- Daten bereinigen und vorbereiten
- Ein Modell (Logistische Regression) trainieren
- Das Modell bewerten

---

### Setup – Bibliotheken importieren

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.metrics import accuracy_score, classification_report, confusion_matrix
```

---

##3 Datensatz laden

Ladet die Datei `Titanic-Dataset.csv` von Kaggle herunter und legt sie in euren Projektordner.

```python
df = pd.read_csv("Titanic-Dataset.csv")

# Ersten Überblick verschaffen
print(df.shape)       # Zeilen x Spalten
print(df.head())      # Erste 5 Zeilen
print(df.info())      # Datentypen und fehlende Werte
```

### Was bedeuten die Spalten?

| Spalte | Bedeutung |
|---|---|
| `Survived` | **Ziel** – 0 = gestorben, 1 = überlebt |
| `Pclass` | Ticketklasse (1 = 1. Klasse, 3 = 3. Klasse) |
| `Sex` | Geschlecht |
| `Age` | Alter |
| `SibSp` | Anzahl Geschwister / Ehepartner an Bord |
| `Parch` | Anzahl Eltern / Kinder an Bord |
| `Fare` | Ticketpreis |
| `Embarked` | Einschiffungshafen (C, Q, S) |

---

### Daten bereinigen

Rohdaten sind selten perfekt — fehlende Werte müssen behandelt werden.

```python
# Fehlende Werte anzeigen
print(df.isnull().sum())

# Age: fehlende Werte mit dem Median füllen
df["Age"] = df["Age"].fillna(df["Age"].median())

# Embarked: fehlende Werte mit dem häufigsten Wert füllen
df["Embarked"] = df["Embarked"].fillna(df["Embarked"].mode()[0])

# Cabin: zu viele fehlende Werte → Spalte weglassen
df = df.drop(columns=["Cabin", "Name", "Ticket", "PassengerId"])
```

---

### Kategorische Spalten kodieren

Modelle verstehen keine Texte wie "male" oder "female" — wir wandeln sie in Zahlen um.

```python
# Sex: male → 0, female → 1
df["Sex"] = df["Sex"].map({"male": 0, "female": 1})

# Embarked: C → 0, Q → 1, S → 2
df["Embarked"] = df["Embarked"].map({"C": 0, "Q": 1, "S": 2})

print(df.head())
```

---

### Features und Ziel trennen

```python
# Ziel (was wir vorhersagen wollen)
y = df["Survived"]

# Features (womit wir vorhersagen)
X = df.drop(columns=["Survived"])

print("Features:", X.columns.tolist())
print("Ziel:", y.value_counts())
```

---

### Train/Test-Split

```python
X_train, X_test, y_train, y_test = train_test_split(
    X, y,
    test_size=0.2,     # 20% zum Testen
    random_state=42,   # Reproduzierbarkeit
    stratify=y         # Gleiche Überlebensrate in beiden Splits
)

print(f"Training: {len(X_train)} Passagiere")
print(f"Test:     {len(X_test)} Passagiere")
```

---

### Daten skalieren

Logistische Regression funktioniert besser, wenn alle Features auf ähnlicher Skala sind.

```python
scaler = StandardScaler()

X_train = scaler.fit_transform(X_train)   # Anpassen NUR auf Trainingsdaten!
X_test  = scaler.transform(X_test)        # Testdaten nur transformieren
```

> **Wichtig:** `fit_transform` nur auf Trainingsdaten anwenden.
> `transform` auf Testdaten — sonst schummeln wir beim Training!

---

### Modell trainieren

```python
model = LogisticRegression(max_iter=200)
model.fit(X_train, y_train)

print("Training abgeschlossen!")
```

---

## # Modell bewerten

```python
y_pred = model.predict(X_test)

# Genauigkeit
print(f"Accuracy: {accuracy_score(y_test, y_pred):.2%}")

# Detaillierter Bericht
print(classification_report(y_test, y_pred, target_names=["Gestorben", "Überlebt"]))
```

**Beispielausgabe:**
```
Accuracy: 80.45%

              precision    recall  f1-score
   Gestorben       0.83      0.85      0.84
   Überlebt        0.76      0.73      0.75
```

---

### Confusion Matrix visualisieren

```python
cm = confusion_matrix(y_test, y_pred)

plt.figure(figsize=(5, 4))
plt.imshow(cm, cmap="Blues")
plt.colorbar()
plt.xticks([0, 1], ["Gestorben", "Überlebt"])
plt.yticks([0, 1], ["Gestorben", "Überlebt"])
plt.xlabel("Vorhergesagt")
plt.ylabel("Tatsächlich")
plt.title("Confusion Matrix")

for i in range(2):
    for j in range(2):
        plt.text(j, i, cm[i, j], ha="center", va="center", fontsize=14)

plt.tight_layout()
plt.show()
```

---

### Welche Features sind wichtig?

```python
feature_names = ["Pclass", "Sex", "Age", "SibSp", "Parch", "Fare", "Embarked"]
koeffizienten = model.coef_[0]

for name, koeff in sorted(zip(feature_names, koeffizienten), key=lambda x: abs(x[1]), reverse=True):
    richtung = "↑ Überleben wahrscheinlicher" if koeff > 0 else "↓ Überleben unwahrscheinlicher"
    print(f"  {name:12s}: {koeff:+.3f}  {richtung}")
```

**Beispielausgabe:**
```
  Sex         : +2.41  ↑ Überleben wahrscheinlicher   (Frauen hatten Vorrang)
  Pclass      : -0.89  ↓ Überleben unwahrscheinlicher  (höhere Klasse → schlechter)
  Age         : -0.43  ↓ Überleben unwahrscheinlicher  (ältere Passagiere → schlechter)
  Fare        : +0.31  ↑ Überleben wahrscheinlicher
```

---

### Eigene Vorhersage machen

```python
# Beispiel: Frau, 28 Jahre alt, 1. Klasse, allein reisend, Ticketpreis 100, Hafen C
passagier = pd.DataFrame([[1, 1, 28, 0, 0, 100.0, 0]],
                          columns=feature_names)

passagier_skaliert = scaler.transform(passagier)
vorhersage = model.predict(passagier_skaliert)
wahrscheinlichkeit = model.predict_proba(passagier_skaliert)

print(f"Vorhersage: {'Überlebt ✅' if vorhersage[0] == 1 else 'Gestorben ❌'}")
print(f"Wahrscheinlichkeit: {wahrscheinlichkeit[0][1]:.1%}")
```

---

### Zusammenfassung

| Schritt | Was | Warum |
|---|---|---|
| Laden | `pd.read_csv()` | Daten einlesen |
| Bereinigen | Median / Modus auffüllen | Fehlende Werte behandeln |
| Kodieren | `map()` | Text → Zahlen |
| Splitten | `train_test_split()` | Fair bewerten |
| Skalieren | `StandardScaler` | Gleiche Skala für alle Features |
| Trainieren | `model.fit()` | Modell lernt Muster |
| Bewerten | `accuracy_score`, `classification_report` | Wie gut ist unser Modell? |

---

### Aufgaben zum Ausprobieren

1. **Ändert den `test_size`** auf 0.3 — wird das Modell besser oder schlechter?
2. **Lasst `Age` weg** aus den Features — wie verändert sich die Accuracy?
3. **Probiert `max_iter=500`** — was ändert sich?
4. **Erstellt ein neues Passagier-Beispiel** und macht eine Vorhersage.
5. **Bonus:** Versucht, einen `RandomForestClassifier` statt `LogisticRegression` zu verwenden — was muss geändert werden?



















