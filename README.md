# 🔐 AdGuard + Unbound DNS Stack

A redundant, privacy-focused DNS setup using AdGuard for filtering and Unbound for recursive DNS resolution.

---

## 🖼 Architecture Diagram

```text
Clients (PCs, IoT)
        |
        v
+-----------------------+
|   AdGuard Primary     |
|     10.10.10.21       |
+-----------------------+
        |
        |\
        | \ (Failover)
        |  \
        |   v
        | +-----------------------+
        | | AdGuard Secondary     |
        | |   10.10.10.157        |
        | +-----------------------+
        |
        v
+-----------------------+
|     Unbound DNS       |
|   10.10.10.23:5335    |
+-----------------------+
        |
        v
     Internet
---

## 📌 Overview

- Dual AdGuard servers for redundancy
- Sync between nodes using adguardhome-sync
- Internal recursive DNS using Unbound
- Separation of filtering and resolution layers

---

## 🧭 Architecture

AdGuard Primary: 10.10.10.21  
AdGuard Secondary: 10.10.10.157  
Unbound: 10.10.10.23 (Port 5335)

---

## 🔍 DNS Flow

Client  
→ AdGuard (Primary or Secondary) on Port 53  
→ Unbound (10.10.10.23:5335)  
→ Internet  

---

## 🔁 Redundancy

- Clients use both AdGuard servers
- If one fails, the other continues working
- Sync keeps both configurations aligned

---

## 🔄 Sync

Using `adguardhome-sync`:

- Primary → Secondary
- Runs every minute
- Syncs filters, clients, and settings

---

## 🛡 Design Notes

- Clients only query AdGuard
- Unbound is internal-only
- Recursive DNS stays inside the network
- Filtering and resolution are separated by design

---

## 📚 Documentation

- [Architecture](docs/architecture.md)
- [Failover](docs/failover.md)

---

## 🎯 Purpose

This project documents a homelab DNS stack as a designed and operated system.

## 🎯 Purpose

This project documents a homelab DNS stack as a designed and operated system.
