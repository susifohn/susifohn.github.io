---
title: 03. Sicherheit  
categories: [unibe, Vernetzte Systeme und Betriebssysteme]
tags: [sniffing, syn-flood]     # TAG names should always be lowercase
math: true
---

# Sicherheit – Zusammenfassung & Cheat Sheet

Basierend auf den Folien **„vs03_sicherheit“** und den Übungen **„VS_ThEx_3__Sicherheit“**.

---

# 1. Einführung in die Sicherheit

## Zentrale Sicherheitsanforderungen

| Sicherheitsanforderung | Bedeutung | Verhindert |
|---|---|---|
| **Vertraulichkeit (Confidentiality)** | Daten dürfen nur von berechtigten Personen gelesen werden | Abhören / Eavesdropping |
| **Authentizität (Authenticity)** | Kommunikationspartner müssen ihre Identität beweisen | Masquerade / Spoofing |
| **Integrität (Integrity)** | Daten dürfen nicht unbemerkt verändert werden | Nachrichtenmodifikation |
| **Verfügbarkeit (Availability)** | Systeme und Dienste müssen erreichbar bleiben | DoS-Angriffe |

---

# Angriffsmethoden

## Passive Angriffe

Passive Angriffe verändern keine Daten, sondern lesen Informationen mit.

### Beispiele

- **Packet Sniffing**
  - Abhören von Paketen
  - z.B. Passwörter oder Kreditkartendaten

- **Verkehrsanalyse**
  - Analyse von:
    - Adressen
    - Paketgrößen
    - Kommunikationsmustern

### Ziel
- Informationen sammeln, ohne entdeckt zu werden.

---

## Aktive Angriffe

Aktive Angriffe verändern oder stören Kommunikation.

### Beispiele

| Angriff | Beschreibung |
|---|---|
| **Masquerading / Spoofing** | Falsche Identität vortäuschen |
| **Replay Attack** | Abgehörte Daten erneut senden |
| **Nachrichtenmodifikation** | Nachrichten manipulieren |
| **DoS (Denial of Service)** | Netzwerk oder Server blockieren |

### Beispiel SYN-Flood
- Viele SYN-Pakete senden
- Server reserviert Ressourcen
- Ressourcen erschöpfen sich

---

# 2. Verschlüsselung

## Symmetrische Verschlüsselung

### Prinzip
- Sender und Empfänger besitzen **denselben geheimen Schlüssel**.
- Derselbe Schlüssel wird zum:
  - Verschlüsseln
  - Entschlüsseln
  verwendet.

## Eigenschaften

| Vorteil | Nachteil |
|---|---|
| Schnell | Schlüssel muss sicher ausgetauscht werden |
| Effizient | Skalierungsproblem bei vielen Benutzern |

## Beispiel
- AES (Advanced Encryption Standard)

---

## Angriffe auf symmetrische Verfahren

### Kryptoanalyse
- Analyse von:
  - Chiffriertext
  - Mustern
  - bekannten Klartexten

#### Arten
- Known Plaintext Attack
- Chosen Plaintext Attack

### Brute Force
- Alle Schlüssel ausprobieren

---

# AES

## Wichtige Punkte
- Standard für symmetrische Verschlüsselung
- Arbeitet blockweise
- Sehr schnell und sicher

---

# Blockchiffren

## ECB (Electronic Code Book)

### Funktionsweise
- Jeder Block wird unabhängig verschlüsselt.

## Problem
Wenn zwei Klartextblöcke identisch sind:
- entstehen identische Chiffreblöcke
- Muster bleiben sichtbar

### Merksatz
❌ ECB ist unsicher für strukturierte Daten.

---

## CBC (Cipher Block Chaining)

### Prinzip
- Jeder Block hängt vom vorherigen Block ab.
- Nutzt einen Initialisierungsvektor (IV).

## Vorteile
- Gleiche Klartextblöcke erzeugen unterschiedliche Chiffreblöcke.

## Nachteile
- Fehler beeinflussen ganzen Block
- Entschlüsselung benötigt vorherigen Block

### Merksatz
✅ CBC ist sicherer als ECB.

---

## Stream Cipher

### Prinzip
- Verschlüsselung erfolgt bitweise oder byteweise.
- Nutzung eines Keystreams.

## Wichtig
⚠️ Keystream darf niemals wiederverwendet werden.

---

# Asymmetrische Verschlüsselung

## Prinzip

Es gibt zwei Schlüssel:

| Schlüssel | Verwendung |
|---|---|
| Öffentlicher Schlüssel | Verschlüsseln |
| Privater Schlüssel | Entschlüsseln |

## Eigenschaften

| Vorteil | Nachteil |
|---|---|
| Kein geheimer Schlüsselaustausch nötig | Langsamer |
| Skalierbar | Rechenintensiv |

---

# Vergleich: Symmetrisch vs. Asymmetrisch

| Eigenschaft | Symmetrisch | Asymmetrisch |
|---|---|---|
| Anzahl Schlüssel | 1 gemeinsamer Schlüssel | Public + Private Key |
| Geschwindigkeit | Schnell | Langsam |
| Schlüsselaustausch | Problematisch | Einfacher |
| Skalierbarkeit | Schlechter | Besser |
| Typische Nutzung | Große Datenmengen | Schlüsselaustausch, Signaturen |
| Beispiel | AES | RSA |

---

# RSA (Rivest-Shamir-Adleman)

## Schlüsselerzeugung

1. Wähle Primzahlen:
   - p
   - q

2. Berechne:

\[
n = p \cdot q
\]

\[
z = (p-1)(q-1)
\]

3. Wähle:
- e
- teilerfremd zu z

4. Berechne d:

\[
e \cdot d \mod z = 1
\]

5. Schlüssel:

| Schlüssel | Inhalt |
|---|---|
| Öffentlich | (e,n) |
| Privat | (d,n) |

---

## RSA-Verschlüsselung

genui{"math_block_widget_always_prefetch_v2":{"content":"c = m^e \\bmod n"}}

- m = Klartext
- c = Chiffretext

---

## RSA-Entschlüsselung

genui{"math_block_widget_always_prefetch_v2":{"content":"m = c^d \\bmod n"}}

---

## Sicherheit von RSA

RSA basiert darauf, dass:

- große Zahlen schwer in Primfaktoren zerlegt werden können.

---

# 3. Datenintegrität

## Ziel
Sicherstellen, dass:

- Nachrichten nicht verändert wurden
- Identitäten echt sind
- Nachrichten nicht abgestritten werden können

---

# Digitale Signaturen

## Prinzip

1. Sender signiert Nachricht mit privatem Schlüssel.
2. Empfänger prüft Signatur mit öffentlichem Schlüssel.

## Garantiert

| Eigenschaft | Bedeutung |
|---|---|
| Authentizität | Sender ist echt |
| Integrität | Nachricht unverändert |
| Non-Repudiation | Sender kann Senden nicht abstreiten |

---

# Message Digests (Hashfunktionen)

## Idee

Anstatt die ganze Nachricht zu signieren:

- wird nur ein kurzer Fingerabdruck signiert.

## Vorteile

✅ Viel schneller

✅ Effizient bei großen Datenmengen

---

# Eigenschaften einer guten Hashfunktion

| Eigenschaft | Bedeutung |
|---|---|
| Deterministisch | Gleiche Eingabe → gleicher Hash |
| Schnell | Effiziente Berechnung |
| Kollisionsresistent | Zwei Eingaben sollen nicht denselben Hash haben |
| Einwegfunktion | Originaldaten schwer rekonstruierbar |

---

# MD5

## Verarbeitungsschritte

1. Padding hinzufügen
2. Länge anhängen
3. Puffer initialisieren
4. Verarbeitung in 512-Bit-Blöcken

## Wichtig
⚠️ MD5 gilt heute als kryptographisch unsicher.

---

# Zertifikate

## Zweck
- Verifikation öffentlicher Schlüssel

## Certificate Authority (CA)

Die CA:
- signiert Zertifikate
- bestätigt die Identität

## Zertifikat enthält

- User-ID
- Öffentlichen Schlüssel
- Signatur der CA

---

# Verifikation eines Zertifikats

1. Hash des Zertifikats berechnen
2. CA-Signatur mit öffentlichem CA-Schlüssel entschlüsseln
3. Werte vergleichen

Wenn gleich:

✅ Zertifikat ist gültig

---

# Trust Chains (Vertrauenskette)

## Problem
- Unterschiedliche Certificate Authorities

## Lösung
- Hierarchische Vertrauenskette
- Root CAs sind in Systemen vorinstalliert

---

# Wichtige Prüfungsfragen

## Frage 1
### Welche Sicherheitsanforderung verhindert welchen Angriff?

| Angriff | Sicherheitsanforderung |
|---|---|
| Message eavesdropping | Vertraulichkeit |
| Masquerade attack | Authentizität |
| Message modification | Integrität |
| DoS attack | Verfügbarkeit |

---

## Frage 2
### Unterschied zwischen symmetrischer und asymmetrischer Verschlüsselung

### Symmetrisch
- Ein gemeinsamer Schlüssel
- Schnell
- Problem: Schlüsselaustausch

### Asymmetrisch
- Öffentlicher + privater Schlüssel
- Langsamer
- Einfachere Schlüsselverteilung

---

## Frage 3
### Kann eine Hashfunktion mit Eingabelänge R > L kollisionsfrei sein?

## Antwort
❌ Nein.

## Warum?

Wenn:
- Eingabegröße größer als Ausgabegröße ist
- mehrere Eingaben auf dieselbe Ausgabe abgebildet werden müssen

Dann gilt nach dem:

### Schubfachprinzip (Pigeonhole Principle)

- Es müssen Kollisionen existieren.

### Wichtig
Eine gute Hashfunktion ist:
- nicht kollisionsfrei
- sondern kollisionsresistent. Das heisst, es ist parktisch unmöglich eine Kollision zu finden/erzeugen. 

---

# Prüfungs-Merksätze

## Grundlagen
- Vertraulichkeit → Schutz vor Abhören
- Integrität → Schutz vor Veränderung
- Authentizität → Schutz vor Identitätsfälschung
- Verfügbarkeit → Schutz vor Ausfällen

---

## Verschlüsselung
- AES = symmetrisch
- RSA = asymmetrisch
- Symmetrisch = schnell
- Asymmetrisch = sicherer Schlüsselaustausch

---

## Blockmodi
- ECB → unsicher
- CBC → sicherer durch Verkettung

---

## Integrität
- Digitale Signaturen garantieren:
  - Authentizität
  - Integrität
  - Non-Repudiation

---

## Hashfunktionen
- Gleiche Eingabe → gleicher Hash
- Kleine Änderung → komplett anderer Hash
- Kollisionen theoretisch unvermeidbar

---

# Lernstrategie für die Prüfung

## Unbedingt können

### Definitionen
- Vertraulichkeit
- Integrität
- Authentizität
- Verfügbarkeit

### Vergleiche
- Symmetrisch vs. asymmetrisch
- ECB vs. CBC

### Verstehen
- RSA-Grundprinzip
- Digitale Signaturen
- Hashfunktionen
- Zertifikate

### Typische Prüfungsaufgaben
- Angriff ↔ Sicherheitsziel zuordnen
- RSA-Rechnung verstehen
- Kollisionsfrage erklären
- Vorteile/Nachteile vergleichen

---

# Ultra-Kurzzusammenfassung

| Thema | Wichtigster Punkt |
|---|---|
| Vertraulichkeit | Schutz vor Abhören |
| Integrität | Schutz vor Änderungen |
| Authentizität | Identität prüfen |
| Verfügbarkeit | Dienste erreichbar halten |
| AES | Symmetrisch, schnell |
| RSA | Asymmetrisch, Public/Private Key |
| ECB | Unsicher wegen Mustern |
| CBC | Sicherer durch Verkettung |
| Digitale Signatur | Authentizität + Integrität |
| Hashfunktion | Fingerabdruck der Nachricht |
| Zertifikat | Verifiziert öffentliche Schlüssel |

