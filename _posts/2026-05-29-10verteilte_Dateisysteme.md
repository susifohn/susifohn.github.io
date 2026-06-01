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
| Parität | eigene dedizierte <br>Paritätsdisk | verteilt auf alle disks |
| Location | fixed | rotated |
| access | every write hits same parity <br> disk -> bottleneck | parity blocks <br>spread evenly | 
| stripe unit| Bit level | Block |
| Sync | all disks <br>spin in sync | operate independently <br>per block | 
| | all disks participate in every <br>single r/w, ideal for large seq transfers | accesses only the relevant <br>data disk + parity disk per operation |

To reconstruct missing data after a single disk failure, all surviving disks must be accessed in both configurations.

## File Block Calculation

- blocks = file_size / block_size
- Metadata entries = Number of blocks

## location-transparent URI

A URI is location-transparent if the name gives no hint about where the resource is physically stored. This URI violates that in multiple ways:

1. The domain www.cds.unibe.ch reveals the location

unibe.ch → University of Bern, Switzerland
cds → a specific department/institute (Center for Data Science)
This directly encodes the organizational and physical location of the server hosting the resource.

2. The path /studies/current_lectures/betriebssysteme/ reveals the directory structure

The path mirrors the physical or logical file structure on the server.
If the file is moved to a different server or folder, the URI breaks — clients must update their links.

Files should be movable to other locations without changing their name.

### Beispiele
- DOIdoi:10.1000/xyz123✅ Yes
- URNurn:uuid:550e8400-e29b-41d4-a716✅ Yes
- DNS alias / CDN URLhttps://resources.example.com/xyz⚠️ Partly




## NFS
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


## Caching
Describe the difference between distributed and non-distributed file systems in terms of time penalty in case of cache miss. Name one scenario in which disk caching is better than memory caching.

Answer

A cache miss in a distributed FS costs more than in a local FS because
it adds network round-trip + RPC overhead on top of the disk access.
Disk caching beats memory caching when data must survive crashes.

- "Disk-Caching: zuverlaessig und robust"
- "Hauptspeicher-Caching: schnellerer Zugriff" -- but less robust.

