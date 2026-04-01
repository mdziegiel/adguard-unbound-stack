# 🔐 AdGuard + Unbound DNS Stack

A redundant, privacy-focused DNS architecture built for homelab use, combining AdGuard Home for filtering and Unbound for recursive resolution.

---

## 📌 Overview

This stack provides:

- Dual AdGuard instances for redundancy
- Centralized configuration sync
- Internal recursive DNS using Unbound
- Separation of filtering and resolution layers

---

## 🧭 Architecture

### Components

- **AdGuard Primary:** 10.10.10.21  
- **AdGuard Secondary:** 10.10.10.157  
- **Unbound:** 10.10.10.23:5335  

---

## 🔍 DNS Flow

```text
Client
  ↓
AdGuard (Primary / Secondary) [Port 53]
  ↓
Unbound (10.10.10.23:5335)
  ↓
Internet

---

## 📚 Documentation

- [Architecture](docs/architecture.md)
- [Failover Behavior](docs/failover.md)
