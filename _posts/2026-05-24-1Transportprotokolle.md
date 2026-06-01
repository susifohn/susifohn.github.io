---
title: 1. Transportprotokolle 
categories: [unibe, Vernetzte Systeme und Betriebssysteme]
tags: [TCP]     # TAG names should always be lowercase
math: true
---

# TCP & UDP — Exam Notes (Tanenbaum Ch. 6 + VS Slides)

---

## 1. Transport Protocols — Overview

### TCP vs. UDP at a Glance

| Feature | TCP | UDP |
|---------|-----|-----|
| Connection | Connection-oriented | Connectionless |
| Reliability | Reliable (retransmission, <br>ACK) | Unreliable |
| Order | Guaranteed | Not guaranteed |
| Flow/Congestion control | ✅ Yes | ❌ No |
| Communication | 1:1 only | 1:1, Multicast, Broadcast |
| Orientation | Byte stream | Message/Datagram |
| Error detection | Mandatory <br>checksum | Optional (IPv4), <br>mandatory (IPv6) |

> **Key insight:** Flow control protects the **receiver** from overflow. Congestion control protects the **network** — implemented in the transport layer even though it is logically a network layer concern.

### Transport vs. Link Layer Protocols

| | Link Layer | Transport Layer |
|--|------------|-----------------|
| Scope | Adjacent nodes over one link | Remote, non-adjacent systems |
| RTT | Small, stable | Large, highly variable |
| Reordering | None (same path) | Possible (IP may route differently) |
| Rate limit | Fixed by link | Bottleneck on intermediate link |

---

## 2. Transport Addresses (Ports)

- **Transport address** = IP address + Port number
- Port identifies a **process** (OS process IDs are not suitable — every OS does it differently)
- **Well-known ports** ≤ 1023 (require root/privileged access)
- Ports 1024–49151 can be registered with IANA

### Key Well-Known Ports

| Port | Protocol |
|------|----------|
| 13 | NTP |
| 20/21 | FTP (data/control) |
| 22 | SSH |
| 25 | SMTP |
| 53 | DNS |
| 80 | HTTP |
| 143 | IMAP |
| 443 | HTTPS |
| 993 | IMAP secure |

### Sockets

> **Socket** = communication endpoint = IP address + Port (OSI service access point)

Connections identified by the **5-tuple**: `(Protocol, Src-IP, Src-Port, Dst-IP, Dst-Port)`

---

## 3. UDP — User Datagram Protocol

Simple, connectionless, unreliable demultiplex service. The application must handle errors.

### UDP Header (8 bytes)

```
| Source Port | Destination Port |
| Length      | Checksum         |
| Data ...                       |
```

### UDP Checksum (Pseudo-Header)

Checksum is calculated over a **pseudo-header** + UDP header + data:

```
| Source IP Address              |
| Destination IP Address         |
| 0 | Protocol | Segment Length  |
| UDP Header + Data              |
```

> ⚠️ This crosses layer boundaries — IP fields from a lower layer are included. The checksum is **optional in IPv4** but **mandatory in IPv6**.

**Why a separate UDP checksum if IPv4 already has one?**
> The IPv4 checksum only covers the **IP header**. The UDP checksum also covers the **UDP header and payload** — without it, data corruption would go undetected.

---

## 4. TCP — Transmission Control Protocol

### TCP Service Properties
- **Connection-oriented**: setup → data transfer → teardown
- **Full-duplex**, **point-to-point** (no multicast/broadcast)
- **Byte stream** (not message stream — boundaries not preserved)
- **Reliable**: sequence numbers, ACKs, retransmission
- **Flow and congestion control** via sliding window

### TCP Header

```
| Source Port       | Destination Port  |
| Sequence Number                       |
| Acknowledgment Number                 |
| HdrLen | 0 | Flags | Advertised Window|
| Checksum          | Urgent Pointer    |
| Options + Data                        |
```

### Key Header Fields

| Field | Description |
|-------|-------------|
| **Sequence Number** | Byte offset of first byte in this segment |
| **Acknowledgment** | Next byte expected (cumulative) |
| **Advertised Window** | Flow control: how many more bytes sender may send |
| **HdrLen** | Number of 32-bit words in header |

### TCP Flags

| Flag | Use |
|------|-----|
| **SYN** | Connection setup |
| **ACK** | Acknowledgment field is valid |
| **FIN** | Sender has no more data |
| **RST** | Reset / abort connection |
| **PSH (EOM)** | Push data to application immediately |
| **URG** | Urgent pointer in use |
| **CWR / ECE** | Congestion notification (ECN) |

---

## 5. TCP Connection Establishment — 3-Way Handshake

> Random initial sequence numbers are chosen to avoid accepting stale (outdated and no longer valid) packets from previous connections on the same port pair.

```
Client (active)                    Server (passive)
    |                                   |
    |--- SYN (SeqNum=x) --------------->|  CONNECT → SYN_SENT
    |                                   |  LISTEN → SYN_RCVD
    |<-- SYN+ACK (SeqNum=y, Ack=x+1) --|
    |--- ACK (Ack=y+1) --------------->|
    |                                   |  → ESTABLISHED
```

### Connection State Machine (Establishment)

```
CLOSED
  ├─(passive)→ LISTEN ──SYN/SYN+ACK──→ SYN_RCVD ──ACK/───→ ESTABLISHED
  └─(active)─→ SYN_SENT ──SYN+ACK/ACK──────────────────────→ ESTABLISHED
```

---

## 6. TCP Connection Teardown — 4-Way Handshake

Both directions must be closed independently. Guarantees all data is delivered before closing.

```
Instanz A                          Instanz B
    |--- FIN (SeqNum=x) ----------->|   (CLOSE)
    |<-- ACK (Ack=x+1) ------------|
    |                               |   (CLOSE)
    |<-- FIN (SeqNum=y) ------------|
    |--- ACK (Ack=y+1) ----------->|
```

### Teardown State Machine

```
ESTABLISHED
  ├─(own side fst)─→ FIN_WAIT_1 ──ACK/─→ FIN_WAIT_2 ─FIN/ACK─→ TIME_WAIT ─tmout─→ CLO
  ├─(oth side fst)─→ CLOSE_WAIT ──Close/FIN──→ LAST_ACK ──ACK/───→ CLd
  └─(simultaneous)─→ FIN_WAIT_1 ──FIN/ACK─→ CLO ─ACK/─→ TME_WAIT → CLO
```

> **TIME_WAIT** = 2 × MSL (Maximum Segment Lifetime) = **240 seconds max**. Ensures no stale packets remain in the network.

---

## 7. TCP Error Correction

### Acknowledgment Strategies

| Type | Description |
|------|-------------|
| **Immediate** | ACK sent for every segment |
| **Delayed** | Wait up to 500ms to piggyback on data |
| **Cumulative** | ACK = highest in-order byte received |
| **Selective (SACK)** | Reports which ranges were received; no Go-Back-N needed |

### Retransmission
- **Standard**: Go-Back-N (retransmit from lost packet onward)
- **Optional**: Selective Repeat (only retransmit what's missing)

---

## 8. Adaptive Retransmission (RTT Estimation)

### Original Algorithm (Slide 20)
Exponentially Weighted Moving Average.

Here the new values, the SampleRTT is only weighted 10-20%. The EstimatedRTT is weighted $$\alpha=0.8-0.9$$. 

```
EstimatedRTT = α × EstimatedRTT + (1-α) × SampleRTT
Timeout = 2 × EstimatedRTT
(0.8 < α < 0.9, typically α = 0.9)
```
### Exam Example (Exercise 1.3)

> EstimatedRTT₀ = 30ms, α = 0.9, samples: 27, 24, 29, 24 ms.
> What is the final estimated RTT according to the Exponentially Weighted Moving Average algorithm?

```
EstimatedRTT₁ = 0.9×30   + 0.1×27 = 29.7 ms
EstimatedRTT₂ = 0.9×29.7 + 0.1×24 = 29.13 ms
EstimatedRTT₃ = 0.9×29.13+ 0.1×29 = 29.117 ms
EstimatedRTT₄ = 0.9×29.117+0.1×24 = 28.605 ms  ← final answer
And Timeout = 2*EstimatedRTT
```

> ⚠️ **Problem (Karn's Algorithm):** When a segment is retransmitted and an ACK arrives, it's ambiguous which transmission the ACK belongs to. Thus don't update EstimatedRTT for retransmitted segments. Rules (Karn/Patrige Algorithmus):

1. Do not sample retransmitted segments. Never update EstimatedRTT using an ACK that could belong to a retransmitted segment. 
2. **exponential backoff** (double timeout on each retry).

### Jacobson/Karels Algorithm (used in practice)

Accounts for **variance** — high variance → larger timeout, low variance → tighter timeout. Intuitively, if the variation
among samples is small, then the EstimatedRTT can be better trusted
and there is no reason for multiplying this estimate by 2 to compute the
timeout.

```
Difference   = SampleRTT − EstimatedRTT
EstimatedRTT = EstimatedRTT + δ × Difference        (δ = 0.5)
Deviation    = (1-δ) × Deviation + δ × |Difference|
Timeout      = µ × EstimatedRTT + φ × Deviation     (µ=1, φ=4)
```


---

## 9. Fast Retransmit

> **Problem:** Waiting for timeout is slow.

**Solution:** Retransmit after **3 duplicate ACKs** (don't wait for timeout).

```
Sender                     Receiver
SEQ=1000 ─────────────────>
         <──────────────── ACK=1100
SEQ=1100 ──────X  (lost)
SEQ=1200 ─────────────────>
         <──────────────── ACK=1100  (dup 1)
SEQ=1300 ─────────────────>
         <──────────────── ACK=1100  (dup 2)
SEQ=1400 ─────────────────>
         <──────────────── ACK=1100  (dup 3) → retransmit SEQ=1100!
SEQ=1100 ─────────────────>
```

---

## 10. TCP Congestion Control

### Core Idea: AIMD
- **Additive Increase**: grow window by 1 MSS per RTT (when no loss)
- **Multiplicative Decrease**: halve window on loss detection

### Slow Start
- Start with small window (1–4 segments)
- **Double** the window each RTT (exponential growth) until threshold reached
- Switch to additive increase when `cwnd ≥ ssthresh`
- On timeout: reset `cwnd = 1`, set `ssthresh = cwnd/2`

### TCP Tahoe vs. TCP Reno

| Event | Tahoe | Reno |
|-------|-------|------|
| Timeout | Slow start from 1 | Slow start from 1 |
| 3 dup ACKs (fast retransmit) | Slow start from 1 | **Fast recovery** (stay near ssthresh) |

> **TCP Reno** avoids slow start after fast retransmit by using **fast recovery**: maintain ack clock using duplicate ACKs until retransmission is acknowledged, then resume from ssthresh linearly.

---

## 11. Sockets API

### Socket Calls

| Call | Description |
|------|-------------|
| `socket()` | Create new endpoint |
| `bind()` | Bind address to socket (server) |
| `listen()` | Mark socket as passive |
| `accept()` | Block and wait for connection |
| `connect()` | Actively open connection |
| `send()` / `recv()` | Send / receive data |
| `close()` | Tear down connection |

### TCP Client/Server Call Sequence

```
Server                          Client
socket()                        socket()
bind()                          connect() ──── 3-Way Handshake ────
listen()
accept() (blocks)
recv()  ←───────────────────── send()
send()  ────────────────────── recv()
recv()  ←───────────────────── close()
close()
```

### UDP Server Call Sequence (Exercise 1.4)

```
socket()
bind()
loop:
  recvfrom()
  sendto()
close()
```

---

## 12. Network Address Translation (NAT)

**Problem:** IPv4 addresses are exhausted.

**Solution:** Use private addresses internally; the NAT box rewrites them to a single public IP, using **port numbers** to distinguish connections.

```
Private Network          NAT Box           Public Internet
192.168.x.x:port_A  →  public_IP:port_X  →  server
192.168.y.y:port_A  →  public_IP:port_Y  →  server
```

The NAT box maintains a **mapping table**: `(private_IP, private_port) ↔ (public_IP, mapped_port)`

### Problems with NAT
- Breaks **end-to-end security** (IPsec headers can't be translated)
- IP addresses embedded in **application data** are wrong (e.g. FTP)
- Encryption hides port numbers → NAT cannot remap
- Operates at **Layer 4** (transport layer)

### NAT Max Hosts Calculation (Exercise 1.5)

> Each host has 100 open TCP connections. NAT has 1 public IP. Max hosts?

```
Total ports:     2^16 = 65536
Reserved ports:  1024
Available ports: 65536 - 1024 = 64512
Max hosts:       floor(64512 / 100) = 645 hosts
```

---

## Exam Cheat-Sheet

| Topic | Key Point |
|-------|-----------|
| Transport address | IP + Port |
| TCP connection | Full-duplex, byte stream, point-to-point only |
| 3-Way Handshake | SYN → SYN+ACK → ACK; random initial seq numbers |
| Connection teardown | 4 segments (FIN+ACK each dir); TIME_WAIT = 2×MSL |
| ACK number | Next **expected** byte (cumulative) |
| Advertised Window | Flow control — receiver's buffer space |
| RTT estimation | EWMA: `EstRTT = α×EstRTT + (1-α)×SampleRTT` (α≈0.9) |
| Karn's algorithm | Don't sample RTT on retransmitted segments; exponential backoff |
| Fast Retransmit | Retransmit after **3 dup ACKs**, don't wait for timeout |
| Slow Start | Exponential growth until ssthresh; then linear (additive) |
| TCP Reno vs Tahoe | Reno adds **fast recovery** (no slow start on dup ACKs) |
| NAT | Maps private IP+port → single public IP+different port |
| UDP checksum | Covers payload too (IPv4 checksum covers only IP header) |
| RTT transport | Large and **highly variable** (≠ link layer) |
