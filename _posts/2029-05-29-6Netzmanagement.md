---
title: 06. Netzmanagement  
categories: [unibe, Vernetzte Systeme und Betriebssysteme]
tags: [fcaps, smi, oid, snmp]     # TAG names should always be lowercase
math: true
---

# 🌐 Netzmanagement – Cheat Sheet (Kapitel 6)

## 1. Einführung

### 🎯 Ziel
- Überwachung, Steuerung und Verwaltung von Netzwerken

---

### 📊 FCAPS-Modell (WICHTIG!)

| Funktion | Bedeutung | Beispiel |
|---------|----------|---------|
| Fault Management | Fehler erkennen & beheben | Detecting failures |
| Configuration Management | Geräte konfigurieren | Setup von Routern |
| Accounting Management | Nutzung erfassen | Tracking resource usage |
| Performance Management | Leistung messen | Measuring throughput |
| Security Management | Zugriff kontrollieren | Controlling access |

👉 Standardaufgaben im Netzwerkmanagement   

---

## 2. Komponenten eines Netzmanagement-Systems

### 🧩 Elemente

#### 🖥 Managed Node
- Gerät im Netzwerk (Router, Switch, PC)

#### ⚙️ Agent
- Software auf dem Gerät
- sammelt lokale Daten und führt Aktionen aus

#### 🧠 Management-Instanz (Manager)
- zentrale Anwendung
- überwacht und steuert das Netzwerk

#### 🗂 MIB (Management Information Base)
- Sammlung von verwaltbaren Objekten (Daten)

---

### 🔄 Interaktion
1. Manager sendet Anfrage (z.B. SNMP GET)  
2. Agent liest Daten aus MIB  
3. Agent sendet Antwort zurück  

👉 Kommunikation über SNMP   

---

## 3. Internet-Netzmanagement-Rahmenwerk

### 🧱 Bausteine

- **MIB**  
  → Datenbank mit Management-Informationen  

- **SMI (Structure of Management Information)**  
  → definiert Datentypen + Struktur  

- **SNMP**  
  → Protokoll für Kommunikation  

---

### ❗ Rolle von SMI
- standardisierte Beschreibung von Daten  
- notwendig für heterogene Systeme  

👉 ohne Standard → inkompatible Datenformate   

---

## 4. ASN.1 (Abstract Syntax Notation One)

### ⚠️ Problem
- unterschiedliche Systeme:
  - Big Endian vs Little Endian
  - unterschiedliche Speicherlayouts

---

### ✅ Lösung: ASN.1 + BER
- **ASN.1** → Beschreibung von Datentypen  
- **BER (Basic Encoding Rules)** → Kodierung  

👉 sorgt für **plattformunabhängigen Datenaustausch**   

---

### 🔢 Beispiel Integer 256 (4 Bytes)

- Big Endian:

```
00 00 01 00
```

- Little Endian:

```
00 01 00 00
```

---

### 📦 BER-Aufbau

```
[Typ] [Länge] [Wert]
```

- Typ: Datentyp (z.B. Integer)
- Länge: Anzahl Bytes
- Wert: tatsächliche Daten

---

## 5. Management Information Base (MIB)

### 🌳 Struktur
- hierarchischer Baum (OID)

### 🔢 Object Identifier (OID)
- eindeutige ID für jedes Objekt

Beispiel:

```
1.3.6.1.2.1
```

→ beschreibt Pfad im Baum   

---

### 📌 Zugriff

#### Skalare Werte

```
OID + .0
```

👉 Beispiel:

```
1.3.6.1.2.1.4.18.0
```
→ einzelner Zählerwert

---

#### Tabellenwerte

```
OID + Index
```

👉 Beispiel:

```
1.3.6.1.2.1.4.34.1.2.1.130.92.65.4
```

- Index = Identifikation der Tabellenzeile (z.B. IP-Adresse)   

---

## 6. SNMP (Simple Network Management Protocol)

### 📡 Eigenschaften
- läuft meist über UDP
- Client = Manager
- Server = Agent   

---

### 📥 Request/Response

- Manager fragt aktiv Daten ab:
  - `get-request`
  - `get-next`
  - `get-bulk`
  - `set-request`

👉 synchron

---

### 🚨 Trap

- Agent sendet Nachricht **ohne Anfrage**
- informiert über Ereignis

👉 asynchron

---

### ✅ Unterschied

| Typ | Verhalten |
|-----|----------|
| Request/Response | Anfrage → Antwort |
| Trap | spontan vom Agent |

👉 Trap = Event-basiert   

---

### 🔐 SNMPv3
- Authentifizierung
- Verschlüsselung
- Zugriffskontrolle   

---

## 7. OpenConfig / modernes Netzmanagement

### ⚠️ Problem mit SNMP
- gut für Monitoring
- schlecht für Konfiguration

---

### ✅ Lösung: OpenConfig

- Datenmodellierung mit **YANG**
- Kommunikation über:
  - **NETCONF**
  - **gRPC (über HTTP)**

---

### 🔄 Vorteile
- strukturierte Daten (XML/JSON)
- Echtzeit-Streaming möglich
- bessere Automatisierung

---

### 📡 NETCONF Operationen
- `get-config`
- `get`
- `edit-config`
- `create-subscription`
- `notification`

---

### ⚡ Vergleich

| SNMP | OpenConfig |
|------|-----------|
| Polling | Streaming |
| begrenzte Konfiguration | volle Konfiguration |
| UDP-basiert | HTTP/gRPC |
| weniger Echtzeit | Echtzeitfähig |

👉 OpenConfig besser für moderne Netze   

---

## ⚡ Prüfungs-Tipps

- ✅ FCAPS auswendig können  
- ✅ Komponenten: Manager, Agent, MIB erklären  
- ✅ SMI → warum notwendig  
- ✅ ASN.1 + BER (Typ-Länge-Wert)  
- ✅ OID Aufbau + Tabellen vs skalare Werte  
- ✅ SNMP:
  - Request vs Trap  
  - v2 vs v3  
- ✅ OpenConfig vs SNMP vergleichen  
- ✅ Szenario: Echtzeit → OpenConfig bevorzugen  

---

