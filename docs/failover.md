# Failover Behavior

This stack provides redundancy at the DNS filtering layer by using two AdGuard Home instances.

## Filtering Layer Redundancy

Clients are configured to use both AdGuard servers:

- Primary: `10.10.10.21`
- Secondary: `10.10.10.157`

If one AdGuard instance becomes unavailable, clients can continue resolving DNS through the other node.

## Sync Layer Behavior

Configuration is synchronized from the primary AdGuard instance to the secondary using `adguardhome-sync`.

If the sync container fails:

- active DNS resolution still works
- filtering still works
- configuration drift may occur over time if changes are made only on the primary

## Upstream Resolver Behavior

Unbound is the upstream recursive resolver and currently listens on:

- `10.10.10.23:5335`

If Unbound becomes unavailable:

- AdGuard remains reachable
- filtering logic still exists
- upstream recursive resolution fails
- external DNS lookups will not complete

## Important Note About Client Failover

DNS failover behavior is client-dependent.

Some devices switch to the secondary resolver quickly, while others may continue preferring the first configured DNS server until timeout or lease renewal.

## Current Resilience Summary

| Component | Redundant | Notes |
|----------|-----------|-------|
| AdGuard filtering layer | Yes | Dual nodes |
| AdGuard config sync | Partial | Sync failure does not stop DNS, but can create drift |
| Unbound upstream resolver | No | Single point of failure |
