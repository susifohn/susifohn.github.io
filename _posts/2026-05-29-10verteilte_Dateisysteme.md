---
title: 10. Verteilte Dateisysteme  
categories: [unibe, Vernetzte Systeme und Betriebssysteme]
tags: [raid, ide, scsi, iscsi, sata, san]     # TAG names should always be lowercase
math: true
---

# 💾 Verteilte Dateisysteme – Cheat Sheet (Kapitel 10)

## 1. Speicherstrukturen & Anbindung

### 🔌 Arten der Disk-Anbindung

| Typ | Beschreibung |
|-----|-------------|
| IDE / SATA / SCSI | direkter Zugriff über I/O-Bus (Host-attached) |
| iSCSI | SCSI über IP-Netz |
| Infiniband | Hochleistungsnetzwerk (SAN) |
| NAS | Zugriff über LAN |
| SAN | dediziertes Netzwerk für Speicher |

👉 Wichtige Zuordnung:
- **Host-attached** → lokal am Rechner  
- **NAS** → Zugriff über LAN  
- **SAN** → separates Hochleistungsnetz   

---

## 2. Zuverlässigkeit (RAID)

### 📊 RAID-Überblick
- RAID = Redundanz + Performance
- Striping → paralleler Zugriff  
- Parität → Fehlererkennung / Rekonstruktion  

---

### 🔁 RAID-3 vs RAID-5

| Merkmal | RAID-3 | RAID-5 |
|--------|-------|--------|
| Parität | eigene Paritätsdisk | verteilt auf

