---
title: 09. Betriebssysteme  
categories: [unibe, Vernetzte Systeme und Betriebssysteme]
tags: [pcb, user mode, kernel mode, system call, trap]     # TAG names should always be lowercase
math: true
---

# 💻 Betriebssysteme – Cheat Sheet (Kapitel 9)

## 1. Einführung

### 📌 Definition
- Betriebssystem = Software, die:
  - Programme ausführt und überwacht
  - Hardware verwaltet
  - Schnittstelle zwischen Benutzer und Hardware bildet   

### 🎯 Ziele
- **Benutzersicht**
  - einfache Nutzung (GUI, CLI)
- **Systemsicht**
  - effiziente Ressourcennutzung (CPU, Speicher, I/O)
  - Sicherheit und Stabilität   

---

### 🧩 Dienste eines Betriebssystems
- Programmausführung
- Ein-/Ausgabe (I/O)
- Dateisystemzugriffe
- Prozesskommunikation
- Fehlererkennung
- Benutzerinterfaces   

---

### ⚙️ Funktionen
- Ressourcenzuweisung (CPU, RAM, Geräte)
- Accounting (Statistiken)
- Schutz & Sicherheit   

---

### 📞 System Calls (WICHTIG!)

👉 Schnittstelle zwischen User-Programm und OS

#### Klassen (+ Beispiele)
1. **Prozesskontrolle**
   - create, terminate
2. **Dateiverwaltung**
   - open(), read(), write()
3. **Geräteverwaltung**
   - read_from_device
4. **Kommunikation**
   - send(), receive()

👉 werden über **trap** in Kernel Mode ausgeführt   

---

## 2. Schutzmechanismen

### ⚠️ Problem
- Fehlerhafte Programme → Systemabsturz möglich

---

### 2.1 Dual Mode

- **User Mode**
  - eingeschränkte Rechte
- **Kernel Mode**
  - volle Rechte

👉 Wechsel durch:
- System Call (trap)
- Interrupt   

---

### 2.2 I/O-Schutz
- I/O-Instruktionen sind **privilegiert**
- Zugriff nur über System Calls möglich   

---

### 2.3 Speicherschutz
- jeder Prozess hat eigenen Speicherbereich
- Hardware prüft Zugriff:
  - Base-Register
  - Limit-Register   

---

### 2.4 CPU-Schutz
- Timer-Interrupts verhindern CPU-Monopolisierung
- → **Time-Sharing**   

---

### 🧠 Beispiel: Disk-Zugriff
1. Programm ruft `read()` auf  
2. → System Call (trap)  
3. Wechsel in Kernel Mode  
4. OS führt privilegierte I/O aus  
5. Rückkehr in User Mode   

---

## 3. Prozesse

### 📌 Definition
- Prozess = Programm in Ausführung  
- besteht aus:
  - Code
  - Daten
  - Stack
  - Heap   

---

### 🔄 Prozesszustände

| Zustand     | Bedeutung |
|------------|----------|
| New        | Prozess erstellt |
| Ready      | wartet auf CPU |
| Running    | wird ausgeführt |
| Blocked    | wartet auf Ereignis |
| Terminated | beendet |

  

---

### 📋 Prozessleitblock (PCB)
- speichert Prozessinformationen:
  - Zustand
  - Register
  - Programmzähler
  - Scheduling-Info

---

### 🔁 Kontextwechsel
- Wechsel zwischen Prozessen
- Zustand wird im PCB gespeichert
- neuer Prozess wird geladen   

---

## 4. Prozessinteraktion

### ⚠️ Problem
- parallele Prozesse → Dateninkonsistenz

---

### 🔄 Erzeuger-Verbraucher
- gemeinsamer Puffer
- Probleme:
  - Überlauf
  - Unterlauf

👉 benötigt Synchronisation   

---

### ⚠️ Race Condition
- mehrere Prozesse greifen gleichzeitig auf Daten zu
- Ergebnis abhängig von Reihenfolge   

---

### 🔒 Kritischer Abschnitt

- Codebereich mit Zugriff auf gemeinsame Daten
- Bedingung:
  - **Mutual Exclusion**

👉 nur 1 Prozess gleichzeitig   

---

### 🔧 Lösung: Test-and-Set

```c
while (Test_and_Set(lock))
    ; // warten

// kritischer Abschnitt
lock = false;
````

* atomare Operation
* verhindert Race Conditions [\[vs09_betriebssysteme \| PDF\]](https://gibbch-my.sharepoint.com/personal/kissling_gibb_ch/Documents/Microsoft%20Copilot-Chatdateien/vs09_betriebssysteme.pdf)

***

### 🧠 Klassiker (Übung)

```c
//Prog i - can work if turn = i und übergibt dann an j
while (turn != i) { }
critical section
turn = j;


//Prog j - can work if turn = j und übergibt dann an i
while (turn != i) { }
critical section
turn 
```

👉 Eigenschaften:

* garantiert Mutual Exclusion
* Wechsel strikt abwechselnd

***

## 5. Virtuelle Maschinen

### 🧠 Idee

* mehrere virtuelle Computer auf einer Hardware

👉 jede VM:

* eigenes OS
* eigene Anwendungen [\[vs09_betriebssysteme \| PDF\]](https://gibbch-my.sharepoint.com/personal/kissling_gibb_ch/Documents/Microsoft%20Copilot-Chatdateien/vs09_betriebssysteme.pdf)

***

### ⚙️ Hypervisor

#### Typ 1 (Bare Metal)

* läuft direkt auf Hardware
* effizienter

#### Typ 2

* läuft auf Host-OS
* einfacher, aber langsamer [\[vs09_betriebssysteme \| PDF\]](https://gibbch-my.sharepoint.com/personal/kissling_gibb_ch/Documents/Microsoft%20Copilot-Chatdateien/vs09_betriebssysteme.pdf)

***

### ✅ Vorteile

* Isolation (Sicherheit)
* mehrere OS gleichzeitig
* gute Ressourcennutzung
* Cloud-Computing [\[vs09_betriebssysteme \| PDF\]](https://gibbch-my.sharepoint.com/personal/kissling_gibb_ch/Documents/Microsoft%20Copilot-Chatdateien/vs09_betriebssysteme.pdf)

***

### ❌ Nachteile

* Overhead
* benötigt Unterstützung durch Hardware [\[vs09_betriebssysteme \| PDF\]](https://gibbch-my.sharepoint.com/personal/kissling_gibb_ch/Documents/Microsoft%20Copilot-Chatdateien/vs09_betriebssysteme.pdf)

***

## ⚡ Prüfungs-Tipps

* ✅ System Call + Klassen + Beispiele kennen
* ✅ User vs Kernel Mode erklären
* ✅ Ablauf eines System Calls (trap!)
* ✅ Prozesszustände sicher zuordnen
* ✅ PCB + Kontextwechsel verstehen
* ✅ Race Condition + kritischer Abschnitt
* ✅ Test-and-Set erklären
* ✅ Virtualisierung + Hypervisor Typ 1 vs 2 vergleichen
* ✅ Erzeuger/Verbraucher Problem verstehen

***


