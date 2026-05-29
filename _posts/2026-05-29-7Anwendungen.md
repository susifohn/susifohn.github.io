---
title: 07. Anwendungen  
categories: [unibe, Vernetzte Systeme und Betriebssysteme]
tags: []     # TAG names should always be lowercase
math: true
---

# 🌍 Anwendungsprotokolle – Cheat Sheet (Kapitel 7)

## 1. Allgemein

### 📌 Idee
- Protokolle auf Anwendungsschicht
- nutzen TCP oder UDP
- ermöglichen Dienste wie:
  - Dateiübertragung
  - E-Mail
  - Remote Login   

---

## 2. Dateitransfer

### 📡 FTP (File Transfer Protocol)

### ⚙️ Eigenschaften
- basiert auf TCP
- **2 Verbindungen:**
  - Kontrollverbindung (Port 21) → dauerhaft
  - Datenverbindung (Port 20) → pro Transfer neu  

👉 für jede Datei eigene TCP-Verbindung   

---

### 📦 Befehle
- `USER`, `PASS`
- `LIST`
- `RETR` (download)
- `STOR` (upload)

---

### 🔄 Modi
- Aktiv: Server initiiert Datenverbindung  
- Passiv: Client initiiert Verbindung (für Firewalls besser)

---

### ⚠️ Besonderheit
- **zustandsbehaftet** (Server speichert Session-Zustand)

---

### 📡 TFTP
- basiert auf UDP
- keine Authentifizierung
- sehr einfach → für Bootstrapping   

---

## 3. Terminal Emulation

### 🖥 TELNET

### ✅ Funktion
- Remote Login
- Terminal wird über Netzwerk emuliert

---

### ❌ Problem
- **keine Verschlüsselung**
- Passwörter im Klartext

---

### ✅ Alternative
- **SSH (Secure Shell)**  
→ sicher (Verschlüsselung)   

---

## 4. E-Mail

### ✉️ Architektur

- **SMTP** → Versand zwischen Servern  
- **POP / IMAP / HTTP** → Zugriff vom Client  

  

---

### 📤 SMTP (Simple Mail Transfer Protocol)

#### 📡 Eigenschaften
- TCP-basiert
- textbasiert

#### 📜 Kommandos
- `HELO`
- `MAIL FROM`
- `RCPT TO`
- `DATA`
- `QUIT`

---

### 📥 Client-Protokolle

#### 📨 POP3
- lädt Mails lokal herunter
- optional löschen vom Server
- ❌ schlecht für mehrere Geräte

#### 📬 IMAP
- Mails bleiben auf Server
- Zugriff von mehreren Geräten
- unterstützt Ordner & Teilabruf

---

### ✅ Vergleich

| Kriterium | POP3 | IMAP |
|----------|------|------|
| Speicherbedarf | gering | höher |
| Multi-Device | schlecht | gut |
| Server-Speicherung | nein | ja |

👉 leichtestes → **POP3**  
👉 mehrere Geräte → **IMAP**   

---

### 📦 MIME

#### Problem:
- SMTP nur ASCII

#### Lösung:
- MIME → unterstützt:
  - Bilder
  - Videos
  - Anhänge

---

### 🔐 Sicherheit
- Authentifizierung
- Verschlüsselung (z.B. PGP)
- Signaturen  

  

---

## 5. Peer-to-Peer-Netze

### 🧩 Typen

#### 1. Zentral
- Server speichert Index + Daten

#### 2. Hybrid
- zentraler Index, verteilte Daten

#### 3. Vollständig verteilt
- kein zentraler Server   

---

### 🔍 DHT (Distributed Hash Table)

- Hash-Funktion:
```

object → objectID
node → nodeID

```
- Speicherung: nächster Knoten im Ring

---

### 📈 Suche
- strukturiertes P2P (DHT):
  - effizient (gerichtet)

👉 Komplexität:
```

O(log N)

```
→ ≈ log(N) Nachrichten (Ringstruktur)

👉 viel besser als Flooding   

---

## 6. Firewalls

### 🔒 Grundidee
- kontrollieren Netzwerkverkehr
- nur erlaubte Verbindungen passieren

---

### 🧱 Typen

#### 1. Paketfilter
- prüft einzelne Pakete (IP, Port)

#### 2. Stateful Firewall
- speichert Verbindungszustand

#### 3. Application Gateway
- Proxy-basierend
- versteht Protokolle  

  

---

## 7. DMZ (Demilitarized Zone)

### 📍 Definition
- separates Netzwerk zwischen intern + Internet

---

### 🖥 Geräte in DMZ
- Web-Server
- Mail-Server
- DNS-Server
- FTP-Server

👉 öffentlich erreichbar  

---

### 🧠 Zweck
- schützt internes Netz
- kein direkter Zugriff von Internet

👉 Kommunikation läuft über Bastion Host   

---

## ⚡ Prüfungs-Tipps

- ✅ FTP → 2 Verbindungen + warum  
- ✅ TELNET vs SSH  
- ✅ SMTP + POP3 + IMAP Unterschiede  
- ✅ POP3 vs IMAP Anwendungsfall  
- ✅ MIME → warum nötig  
- ✅ P2P:
  - strukturiert vs unstrukturiert  
  - DHT Idee  
- ✅ log(N) Suche erklären  
- ✅ Firewall-Typen unterscheiden  
- ✅ DMZ + typische Systeme  

---
