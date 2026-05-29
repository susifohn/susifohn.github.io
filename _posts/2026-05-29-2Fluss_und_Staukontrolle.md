---
title: 02. Fluss und Staukontrolle  
categories: [unibe, Vernetzte Systeme und Betriebssysteme]
tags: [mptcp, reno, vegas, advertised window]     # TAG names should always be lowercase
math: true
---
# 📡 Fluss- und Staukontrolle – Cheat Sheet (Kapitel 2)

## 1. Grundidee

### 🎯 Ziele
- **Flusskontrolle**: Empfänger vor Überlast schützen  
- **Staukontrolle**: Netzwerk (Router) vor Überlast schützen   

---

## 2. Flusskontrolle (TCP)

### 🔁 Grundprinzip
- basiert auf **Sliding Window**
- Empfänger steuert Datenrate

### 📦 Wichtige Felder

#### ✅ Acknowledgment (ACK)
- bestätigt alle Bytes **bis zu einer Sequenznummer**
- Sender weiss, was erfolgreich empfangen wurde   

#### ✅ Advertised Window (rwnd)
- wie viele Bytes der Empfänger noch aufnehmen kann
- wird im TCP-Header gesendet   

### 🧮 Regel
- Sender darf senden bis:
```

ACK + AdvertisedWindow

```

---

## 3. Staukontrolle (TCP)

### 🎯 Ziel
- Vermeidung von Überlast im Netzwerk (Router-Buffers)

### 📉 Problem
- Paketverluste → Wiederholungen → verstärken Stau   

---

### 3.1 Congestion Window (cwnd)
- steuert Sendefenster basierend auf Netzlast

---

### 3.2 Slow Start

- Start mit:
```

cwnd = 1 MSS

```
- Wachstum:
- **exponentiell (Verdopplung pro RTT)**
- bis Schwelle erreicht

👉 gut für schnellen Verbindungsaufbau

---

### 3.3 Additive Increase (Congestion Avoidance)

- nach Slow Start:
- **lineares Wachstum**
```

+1 MSS pro RTT

```
- Ziel: stabile Nutzung

---

### 3.4 Verhalten bei Verlust

#### ⏱ Timeout
- starkes Zeichen von Stau
- Reaktion:
```

cwnd = 1 MSS

```
👉 konservativ → Netzwerk kann sich erholen   

#### 📉 Multiplicative Decrease
- cwnd wird halbiert
```

cwnd = cwnd / 2

```

👉 ergibt **Sägezahnverlauf**

---

### 3.5 Explicit Congestion Notification (ECN)

- Router markieren Pakete bei Stau (ToS-Bits)
- Sender reduziert Fenster **ohne Paketverlust**   

---

## 4. Kombination Fluss + Staukontrolle

### 🧮 Wichtige Formeln

```

MaxWindow = min(cwnd, AdvertisedWindow)
EffectiveWindow = MaxWindow - (LastByteSent - LastByteAcked)

```

👉 Sender darf senden:
```

EffectiveWindow Bytes

```

### ✅ Interpretation
- cwnd → Netzwerkgrenze  
- rwnd → Empfängergrenze  
- **Minimum bestimmt tatsächliches Limit**   

---

## 5. TCP-Varianten

### 🟢 Tahoe
- Basic Slow Start + Timeout

### 🔵 Reno
- Fast Retransmit + Fast Recovery  
- reagiert auf Paketverlust

### 🟣 Vegas
- erkennt Stau über **RTT-Anstieg**
- reduziert Rate **vor Paketverlust**

👉 erkennt Stau früher als Reno   

---

## 6. Bandbreiten-Verzögerungs-Produkt (BDP)

### 📐 Definition
```

BDP = Bandbreite × RTT

```

### 🎯 Bedeutung
- wie viele Daten im Netz „in flight“ sein können

---

### ⚠️ Problem
- 32-bit Sequenznummern können überlaufen
- bei hohen Bandbreiten sehr schnell (z.B. Sekunden)   

---

## 7. TCP-Optimierungen

### 📈 Window Scaling
- Advertised Window (16 Bit) → max 64 KB  
- Lösung: Skalierungsfaktor

### ⏱ RTT-Messung
- Timestamps für genauere RTT

### 🔐 Schutz vor Seq-Überlauf
- Kombination aus Timestamp + Sequenznummer

---

### 🚀 CUBIC

- speziell für hohe BDP-Netze

#### Eigenschaften
- Wachstum:
```

W(t) = C (t - K)^3 + Wmax

```
- unabhängig von RTT
- schnelles Zurück zu Wmax
- lange stabile Phase   

---

### 🧠 BBR
- basiert auf:
  - Bandbreite (Bottleneck)
  - RTT
- schätzt optimalen Betriebspunkt

---

## 8. Multi-Path TCP (MPTCP)

### 🎯 Ziel
- mehrere Netzwerkpfade gleichzeitig nutzen

### ✅ Vorteile
- höhere Robustheit
- bessere Auslastung
- z.B. WLAN + 5G gleichzeitig   

---

### 🔧 Struktur
- eine Verbindung (Connection)
- mehrere Subflows (TCP-Verbindungen)

---

### 🔢 Sequenznummern
- auf 2 Ebenen:
  - Subflow
  - Verbindung

---

### 📊 Kontrolle
- Flusskontrolle:
  - pro Subflow + global
- Staukontrolle:
  - pro Subflow
  - fair gegenüber normalem TCP

---

## ⚡ Prüfungs-Tipps

- ✅ Unterschied: **Flow vs Congestion Control**
- ✅ ACK + AdvertisedWindow erklären
- ✅ cwnd + Slow Start + AIMD verstehen
- ✅ Timeout → cwnd = 1 MSS (Begründung!)
- ✅ Formeln:
  - MaxWindow
  - EffectiveWindow
- ✅ Reno vs Vegas (Loss vs RTT)
- ✅ BDP Bedeutung erklären
- ✅ CUBIC vs Reno
- ✅ MPTCP → Vorteile + Aufbau

