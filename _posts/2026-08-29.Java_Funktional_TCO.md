---
title: Haskell vs Java Funktional 1
categories: [Haskell, Theorie ]
tags: [haskell, recursion, java]     # TAG names should always be lowercase
math: true
---

# Funktionale Programmierung: Java vs. Haskell

Dieses Dokument fasst die Kernkonzepte der funktionalen Programmierung beim Übergang von Haskell zu Java zusammen – insbesondere den Umgang mit Listen, Rekursion und Optimierungen.

---

## 1. Listen-Iteration im Vergleich

In **Haskell** sind Listen rekursiv als `(x:xs)` (Head und Tail) definiert. Da es keine Schleifen gibt, wird per Rekursion oder High-Order Functions iteriert.

In **Java** sind Listen im Speicher flache Arrays (`ArrayList`). Für den funktionalen Ansatz nutzt man ab Java 8 die **Stream API**. Sie wandelt die Liste in einen sequentiellen Datenstrom um, auf den High-Order Functions angewendet werden.

### Beispiel: Aufsummieren einer Liste

#### Haskell
```haskell
-- Verwendung der eingebauten sum-Funktion oder via foldl
totalSum :: [Int] -> Int
totalSum numbers = foldl (+) 0 numbers
```

#### Java (Standard-Weg mit Streams)
In der Java-Praxis nutzt man für Performance-Zwecke meist primitive Streams (`IntStream`) oder die `.reduce()`-Methode (das Äquivalent zu `foldl`).

```java
import java.util.List;

public class FunctionalSum {
    public static void main(String[] args) {
        List<Integer> numbers = List.of(1, 2, 3, 4, 5);

        // Variante A: Über den primitiven IntStream (Empfohlen & hochperformant)
        int sum1 = numbers.stream()
                          .mapToInt(Integer::intValue)
                          .sum();

        // Variante B: Klassisches Reduce (entspricht exakt foldl)
        int sum2 = numbers.stream()
                          .reduce(0, (a, b) -> a + b);

        System.out.println("Ergebnis: " + sum1); // 15
    }
}
```

---

## 2. Rekursion & Pattern Matching im Haskell-Stil (Ab Java 21)

Seit Java 21 unterstützt die Sprache mächtiges **Pattern Matching für Records** und **Sealed Interfaces**. Damit lässt sich die rekursive Natur von Haskell fast 1:1 nachbauen.

```java
public class RecursiveList {
    // Definition der algebraischen Struktur (ähnlich wie: data List a = Nil | Cons a (List a))
    sealed interface MyList<T> {}
    record Nil<T>() implements MyList<T> {}
    record Cons<T>(T head, MyList<T> tail) implements MyList<T> {}

    // Rekursive Summen-Funktion
    public static int sum(MyList<Integer> list) {
        return switch (list) {
            case Nil<Integer> _               -> 0;
            case Cons<Integer>(var x, var xs) -> x + sum(xs); // Entspricht (x:xs) -> x + sum xs
        };
    }

    public static void main(String[] args) {
        MyList<Integer> list = new Cons<>(1, new Cons<>(2, new Cons<>(3, new Nil<>)));
        System.out.println("Rekursive Summe: " + sum(list)); // 6
    }
}
```

---

## 3. Das Problem mit der Rekursion in Java: Tail-Call Optimization (TCO)

Ein fundamentaler Unterschied zwischen den beiden Sprachen liegt in der Speicherverwaltung bei Rekursionen.

### Begriffe sauber getrennt:
* **Tail Recursion (Endrekursion):** Eine Eigenschaft des Codes. Der rekursive Aufruf ist die *absolut letzte Aktion* einer Funktion. Es stehen keine Berechnungen (wie eine Addition) mehr aus.
* **Tail-Call Optimization (TCO):** Ein Feature des Compilers/der Laufzeitumgebung (z. B. Haskell GHC). Der aktuelle Stack-Frame wird bei einem Endaufruf wiederverwendet. Speicherverbrauch: $O(1)$.

### Code-Vergleich: Echte Endrekursion (mit Akkumulator)

#### Haskell (Nutzt TCO optimal)
```haskell
sumTail :: [Int] -> Int -> Int
sumTail [] acc     = acc
sumTail (x:xs) acc = sumTail xs (acc + x)  -- Letzte Aktion ist der reine Aufruf
```

#### Java (Keine TCO-Unterstützung)
```java
public static int sumTail(MyList<Integer> list, int acc) {
    return switch (list) {
        case Nil<Integer> _               -> acc;
        case Cons<Integer>(var x, var xs) -> sumTail(xs, acc + x); // Endrekursiver Aufruf!
    };
}
```

### Das Java-Dilemma:
Obwohl der Java-Code oben syntaktisch perfekt endrekursiv geschrieben ist, besitzt die **Java Virtual Machine (JVM) keine TCO**. 

Für jeden rekursiven Schritt wird ein neuer Stack-Frame geöffnet. Bei grossen Listen (z. B. 20'000 Elemente) führt dieser Code in Java unweigerlich zu einem **`java.lang.StackOverflowError`**.

### Fazit für die Praxis
* **In Haskell:** Nutze Rekursion (bevorzugt endrekursiv) oder Standard-Kombinatoren.
* **In Java:** Vermeide tiefe Rekursionen. Nutze für funktionale Abläufe immer die **Stream API**, da diese intern iterativ (über Schleifen) optimiert ist und den Call-Stack nicht belastet.
