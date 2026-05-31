---
title: 04. Netzsicherheit  
categories: [unibe, Vernetzte Systeme und Betriebssysteme]
tags: [ipsec, wpa, tkip]     # TAG names should always be lowercase
math: true
---


# 🛡️ Netzsicherheit – Cheat Sheet (Kapitel 4)

## 1. Schlüsselverwaltung

### 🔑 Methoden
- Öffentliche Schlüssel → Zertifikate  
- Key Distribution Center (KDC)  
  - erzeugt Sitzungsschlüssel  
  - verteilt ihn verschlüsselt an Parteien  
- Diffie-Hellman (DH)  
  - Ziel: gemeinsamer Schlüssel ohne vorherigen Austausch  
  - Ablauf:  
    - A: $$n = g^x \mod p $$  
    - B: $$ m = g^y \mod p $$  
    - Schlüssel: $$ z = g^{xy} \mod p $$

#### Beispiel
$$
p=47, g=3, A:x=8, B:y=10 \\
A \rightarrow B: (47,3,n=28 (=3^8 \mod 47)) \\
B \rightarrow A: (47,3,m=17 (=3^{10} \mod 47)) \\
\text{Key } z=17^8 \mod 47 = 28^{10} \mod 47 = 3^{80} \mod 47 = 4
$$

### 🚨 Sicherheitsproblem
- Man-in-the-Middle (MITM)
  - keine Authentifizierung bei DH  
  - Angreifer (M) sitzt zwischen A und B  
  - baut zwei getrennte Schlüsselverbindungen auf  

### ✅ Gegenmassnahmen
- Zertifikate (PKI)
- Authentifizierter DH (z.B. Fixed DH)
- Pre-shared Keys

---

## 2. Authentifizierungsprotokolle

### 🎯 Ziel
- Verifikation der Identität des Kommunikationspartners  
- zusätzlich: oft Schlüsselaustausch  

### 🚨 Replay Attack
- Alte Nachricht wird erneut gesendet  
- Ziel: System täuschen / Aktionen wiederholen  

### ✅ Schutzmechanismen

#### 1. Nonces
- einmalige Zufallszahl pro Nachricht  
- Empfänger speichert alle bisher verwendeten Nonces  
- doppelte Nonce → Angriff erkannt  

#### 2. Zeitstempel
- Nachricht enthält Zeit  
- Empfänger prüft Gültigkeitszeit  
- benötigt Zeitsynchronisation  

#### 3. Challenge-Response
- Empfänger sendet Challenge (Nonce)  
- Sender muss korrekt antworten  

### Authentifizierungsprotokoll mit geheimen Key - Needham-Schroeder

### it solves what?
Alice, Bob want to communicate, but never met. They share only a secret key with a KDC. With never sending the secret key over the network.

### Remaining weakness
Eve records an old session and somehow obtained an old session Key SK. Bob has no way to detect it's old. This is the *replay attack vulnerability* and was fixed later with adding timestamp e.g. in Kerberos. 

---

## 3. Allgemeine Netzsicherheit

### 📍 Platzierung im Stack
- Anwendungsschicht → TLS  
- Netzwerkschicht → IPSec  
- Link Layer → WLAN Security  

---

## 4. WLAN-Sicherheit

### 🔓 Schwächen
- ESSID wird im Klartext gesendet  
- MAC-Adressen leicht spoofbar  

### 🔐 Verfahren
- WEP  
  - unsicher (Plaintext-Angriffe)  
- WPA / IEEE 802.11i  
  - TKIP (Schlüsselwechsel)  
  - AES (sicher)  
- 802.1X + RADIUS  
  - zentrale Authentifizierung  

---

## 5. IPSec Grundlagen

### 📦 Protokolle
- AH (Authentication Header)  
  → Authentizität + Integrität  
- ESP (Encapsulating Security Payload)  
  → Verschlüsselung + optional Authentizität  

### 🔗 Security Association (SA)
- logische, unidirektionale Verbindung  
- definiert Sicherheitsparameter  
- identifiziert durch:  
  - IP-Adresse  
  - SPI (Security Parameter Index)  
  - Protokoll (AH/ESP)  

---

## 6. Schutz vor Replay-Angriffen (IPSec)

### 🔢 Sequenznummern
- jedes Paket hat eindeutige Nummer  
- wird im Header mitgeschützt  

### 🪟 Sliding Window
- Fensterbereich: N - W + 1 ... N  
  - N = höchste empfangene Sequenznummer  
  - W ≈ 64  

#### Verhalten
- Sequenznummer < Fenster → verwerfen  
- innerhalb Fenster, aber schon gesehen → verwerfen  
- neue Sequenznummer → akzeptieren + Fenster verschieben  

### 🔚 Limit
- 32-bit Sequenznummer  
- bei Erreichen:  
  → neue Security Association notwendig  

---

## 7. IPSec Modi

### 🚇 Tunnel-Modus
- gesamtes IP-Paket wird verschlüsselt  
- neues IP-Paket wird erstellt  
- Einsatz: VPN  

### 📦 Transport-Modus
- nur Nutzdaten verschlüsselt  
- Ende-zu-Ende Kommunikation  

---

## 8. TLS (Transport Layer Security)

### 🔐 Funktionen
- Client- & Server-Authentifizierung  
- symmetrische Sitzungsschlüssel  

### 🔄 Ablauf
1. Handshake  
   - Zertifikate austauschen  
   - Sitzungsschlüssel erzeugen  
2. Record Protocol  
   - Fragmentierung  
   - (optional) Kompression  
   - Verschlüsselung  

---

## ⚡ Prüfungs-Tipps

- Diffie-Hellman → MITM + Gegenmassnahmen kennen  
- Replay Attack → Nonce + Timestamp erklären  
- IPSec → Sequenznummer + Sliding Window verstehen  
- AH vs ESP unterscheiden  
- SA + SPI sicher erklären  
- Tunnel vs Transport Modus vergleichen  
