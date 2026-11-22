# ADR-0007: Determine whether a dedicated cache is needed

- **Status:** Proposed
- **Date:** 2026-11-22
- **Decision owners:** Not assigned in source material; Neo Kim's approval required before GitHub changes.
- **Related issue(s):** [Issue #7](https://github.com/systemdesign-diagram/Todo-App/issues/7)

## Context

Issue #7 leaves Redis, database-backed caching, invalidation, and consistency open. Neo explicitly asks not to add Redis by convention and to identify operations that need caching versus indexing first. There are no workload measurements or latency targets in the reviewed material.

## Decision drivers

- Measured read/write workload and latency requirements.
- Consistency, invalidation correctness, hosting budget, and operational cost.

## Considered options

### No dedicated cache; optimize primary-store access

**Advantages:** Fewer dependencies and no separate cache invalidation path.

**Disadvantages:** Repeated expensive work still reaches the primary store.

### Database-backed cache/materialization

**Advantages:** Can reuse an existing operational dependency for selected computed results.

**Disadvantages:** Adds expiry/invalidation work and load to the same store.

### Redis

**Advantages:** Fast ephemeral access and reduced repeated computation for suitable workloads.

**Disadvantages:** Adds a service, failure modes, memory management, stale-data risks, and invalidation complexity.

## Decision

No option has been approved. **Draft recommendation:** Start evaluation without a dedicated cache. Define targets, inspect representative queries, and apply appropriate indexes before proposing Redis. This reflects Neo's evaluation constraint, not approval of a final caching architecture or a blanket prohibition on Redis.

## Consequences

The following are anticipated consequences of the draft recommendation, not observed implementation results.

### Positive

- Avoids premature infrastructure and consistency complexity.
- Makes a future cache proposal evidence-driven.

### Negative / trade-offs

- Uncached hotspots could exceed targets and require later work.
- Indexing has storage and write costs and cannot solve every expensive operation.

### Follow-up work

- Identify expensive/frequent reads and establish latency/load targets.
- Inspect query plans and measure representative indexed workloads.
- If targets still fail, document cache keys, TTL/invalidation, failure behavior, and acceptable staleness before selecting a cache.
- Treat authorization-related caches separately because revocation correctness matters.

## References

- [Issue #7](https://github.com/systemdesign-diagram/Todo-App/issues/7), including comments checked on 2026-11-22: none present.
- [Technical design discussions, section 7](https://github.com/systemdesign-diagram/Todo-App/blob/f5adb18f0792685c3900242286551ee62c5ae324/design-discussions.md).
- [Architecture overview](https://github.com/systemdesign-diagram/Todo-App/blob/f5adb18f0792685c3900242286551ee62c5ae324/overview.md).
