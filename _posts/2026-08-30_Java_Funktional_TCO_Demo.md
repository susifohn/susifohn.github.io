---
title: Haskell vs Java Funktional 2 (Demo)
categories: [Haskell, Theorie ]
tags: [haskell, recursion, java]     # TAG names should always be lowercase
math: true
---

# Rekursion, TCO & Lazy Evaluation (Java vs. Haskell)


---

## Teil 1: Das Java-Dilemma (Kein TCO)

**Lerneffekt für Studierende:** Java besitzt keine Tail-Call Optimization (TCO). Selbst strukturell perfekte Endrekursion führt bei grossen Datenmengen unweigerlich zum Absturz.

### Schritt 1: Nicht-endrekursiv (Klassischer Stack-Overflow)
```java
public class JavaStackDemo {
    public static long sum(long n) {
        if (n <= 0) return 0;
        return n + sum(n - 1); // Die Addition '+' blockiert den Stack-Frame
    }
    public static void main(String[] args) {
        System.out.println(sum(50_000)); // 💥 java.lang.StackOverflowError
    }
}
```

### Schritt 2: Manuelle Optimierung auf Endrekursion (Nutzlos in Java)
Wir bauen einen Akkumulator ein. Der rekursive Aufruf ist nun die absolut letzte Aktion.
```java
public class JavaTailDemo {
    public static long sumTail(long n, long acc) {
        if (n <= 0) return acc;
        return sumTail(n - 1, acc + n); // Strukturell perfekte Endrekursion
    }
    public static void main(String[] args) {
        System.out.println(sumTail(50_000, 0)); // 💥 Immer noch java.lang.StackOverflowError!
    }
}
```
*💡 **Hinweis für die Demo:** Die JVM optimiert diesen Code standardmässig nicht. In Java führt an Schleifen oder der iterativ optimierten Stream-API kein Weg vorbei.*

---

## Teil 2: Die GHCi-Demo (TCO & die Lazy-Evaluation-Falle)

Starten Sie den GHCi im Terminal (`ghci`). Da Optimierungen standardmässig deaktiviert sind, lässt sich die Lazy-Evaluation-Problematik perfekt demonstrieren.

### Schritt 1: Die naive Endrekursion (Crash durch Thunks)
Strukturell ist das eine Endrekursion. Da Haskell aber "lazy" (träge) evaluiert, wird `(acc + n)` nicht berechnet, sondern als riesige Kette im Speicher gestapelt (ein *Thunk*).
```haskell
sumLazy n acc = if n == 0 then acc else sumLazy (n - 1) (acc + n)

sumLazy 10000000 0
-- Exception: stack overflow
```

### 🚀 Schritt 2: Die manuelle Lösung via Strictness (`$!`)
Der Operator `$!` zwingt Haskell, den Akkumulator in jedem Schritt *sofort* auszuwerten. Der Thunk-Stapel wird verhindert.
```haskell
sumStrict n acc = if n == 0 then acc else (sumStrict (n - 1) \$! (acc + n))

sumStrict 10000000 0
-- Ergebnis: 50000005000000 (Erfolg!)
```

### Schritt 3: Der Compiler-Zauber (`:set -O2`)
Zeige den Studierenden, dass der GHC-Compiler bei der echten Produktivarbeit (Kompilierung) mitdenkt. Wir aktivieren die Optimierung direkt im GHCi:
```haskell
-- Optimierung einschalten
:set -O2

-- WICHTIG: Funktion neu definieren, damit die Optimierung greift!
sumLazyOptimized n acc = if n == 0 then acc else sumLazyOptimized (n - 1) (acc + n)

sumLazyOptimized 10000000 0
-- Ergebnis: 50000005000000 (Läuft dank Strictness-Analyse des Compilers durch!)
```

### Schritt 4: Optimierung wieder ausschalten (`:unset -O2`)
Um den Gegenbeweis anzutreten, deaktivieren wir das Compiler-Flag wieder.
```haskell
-- Optimierung ausschalten
:unset -O2

-- WICHTIG: Erneut unter neuem Namen definieren!
sumLazyNoO2 n acc = if n == 0 then acc else sumLazyNoO2 (n - 1) (acc + n)

sumLazyNoO2 10000000 0
-- Exception: stack overflow (Der Schutz ist wieder weg)
```

---

## Teil 3: Vertiefung – Der Praxis-Klassiker: `foldl` vs. `foldl'`

**Lerneffekt für Studierende:** Das theoretische Wissen über Thunks erklärt ein berühmt-berüchtigtes Phänomen in der echten Haskell-Welt: Warum man die Standardfunktion `foldl` fast nie benutzen sollte.

Wenn wir eine echte Liste aufsummieren, verhält sich das träge `foldl` genau wie unsere `sumLazy`-Funktion oben. Es stapelt unzählige unausgewertete Additionen auf dem Stack. Die strikte Variante `foldl'` (aus `Data.List`) wertet den Akkumulator in jedem Schritt sofort aus.

### Das Standard-`foldl` (Lazy)
```haskell
-- Erzeuge eine Liste von 1 bis 10 Millionen
millionList = [1..10000000]

foldl (+) 0 millionList
-- Exception: stack overflow
```

### Die Lösung: `foldl'` (Strict)
```haskell
-- Importiere das Modul für die strikte Variante
import Data.List (foldl')

foldl' (+) 0 millionList
-- Ergebnis: 50000005000000 (Läuft in O(1) Speicher durch!)
```
*💡 **Erklärung für das Board:** `foldl` baut einen riesigen Baum im Speicher auf: `(((0 + 1) + 2) + 3) ...`. Erst ganz am Ende wird versucht, diesen von aussen nach innen aufzulösen, was den Stack sprengt. `foldl'` berechnet stattdessen im ersten Schritt `1`, im zweiten `3`, im dritten `6` und hält den Speicher sauber.*

---

## Wichtige Hinweise für eine reibungslose Live-Demo

1. **Der Funktions-Cache-Fallstrick:** 
   Wenn du Optimierungen im GHCi via `:set` oder `:unset` änderst, betrifft das **nur Funktionen, die danach neu definiert** (oder per `:reload` neu geladen) werden. Bereits deklarierte Funktionen im Speicher behalten ihren alten Kompilier-Status. Nutze in der Demo deshalb immer leicht abgewandelte Funktionsnamen (wie oben `sumLazyOptimized`, `sumLazyNoO2`).
2. **Aktive Flags kontrollieren:**
   Wenn du oder die Studierenden den Überblick verlieren, welche Flags gerade aktiv sind, tippst du einfach Folgendes ein:
   ```haskell
   :set
   ```
   *(Ohne Argumente eingegeben, listet dieser Befehl alle aktuell gesetzten Compiler-Flags auf. Nach `:unset -O2` sollte `-O2` dort verschwunden sein).*
3. **Falls du Code aus einer `.hs`-Datei lädst:**
   Wenn du die Funktionen lieber in einer Datei (z. B. `Demo.hs`) vorbereitest und via `:l Demo.hs` lädst, gilt dieselbe Regel: Nach einem `:set -O2` oder `:unset -O2` musst du die Datei zwingend mit **`:reload`** neu einlesen, damit der Compiler den Code mit den neuen Flags analysiert.
