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
| NAS | Zugriff über LAN. (NFS is a filesystem used here) |
| SAN | dediziertes Netzwerk für Speicher |

👉 Wichtige Zuordnung:
- **Host-attached** → lokal am Rechner  
- **NAS** → Zugriff über LAN, nicht Host I/O  
- **SAN** → separates Hochleistungsnetz, connecting servers to strorage dev at block level.
  

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

### NFS
1. Stateful file service
    - Tracking of files accessed by each client
    - Faster access to file through server caching and prefetching. Because the server knows which files a client has open and tracks the current read position, it can predict what data the client will need next and prefetch it proactively.
    - Server loads file in main memory after a client opens it, and returns a unique file identifier. The server processes the open() call, creates an entry in its open-file table, and returns a file handle or identifier to the client. The client then uses this identifier in subsequent read(), write(), and close() calls — exactly like a local OS file descriptor. A stateless server would have no such open/close concept; 
    - Uses the file identifiers when a client wants to access a file again
    - keeps table of open files needed on the server

2. Stateless
    - No table of open files needed on the server. Every request must be self-contained, carrying all necessary information (file path, offset, length) — the server has no memory of previous interactions.
    - Robust against unexpected client or server crash. Because the server stores no state, a crash and restart leaves nothing to recover or clean up.
    - Simply returns the requested blocks to the client




