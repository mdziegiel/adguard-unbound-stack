# Architecture

## Purpose

This stack is designed to provide redundant DNS filtering with a separate internal recursive resolver.

The goal is to keep:

- client-facing DNS filtering on AdGuard
- upstream recursive resolution on Unbound
- configuration consistency across both AdGuard nodes

## Components

### AdGuard Primary
- IP: `10.10.10.21`
- Role: primary client-facing DNS filter
- Function: source of truth for AdGuard configuration

### AdGuard Secondary
- IP: `10.10.10.157`
- Role: secondary client-facing DNS filter
- Function: standby filtering node synchronized from the primary

### Unbound
- IP: `10.10.10.23`
- Port: `5335`
- Role: internal upstream recursive resolver
- Function: resolves external DNS queries for AdGuard

### adguardhome-sync
- Role: synchronization service
- Function: replicates AdGuard configuration from primary to secondary

## Traffic Flow

```text
Client
  ↓
AdGuard Primary / Secondary (port 53)
  ↓
Unbound (10.10.10.23:5335)
  ↓
Internet
