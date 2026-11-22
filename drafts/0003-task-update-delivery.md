# ADR-0003: Choose task-update delivery

- **Status:** Proposed
- **Date:** 2026-11-22
- **Decision owners:** Not assigned in source material; Neo Kim's approval required before GitHub changes.
- **Related issue(s):** [Issue #3](https://github.com/systemdesign-diagram/Todo-App/issues/3)

## Context

Issue #3 requires updates when tasks change. WebSockets and polling remain alternatives. Neo does not want persistent connections if polling is sufficient. Concurrent users, event frequency, acceptable staleness, and hosting limits are not specified.

## Decision drivers

- User-visible update latency and correctness.
- Concurrent active clients, event frequency, server load, and operational cost.
- Recovery after disconnects and current authorization.

## Considered options

### Polling

**Advantages:** Simple HTTP request lifecycle and reconnect behavior; no long-lived connection infrastructure.

**Disadvantages:** Latency depends on interval; idle polling can create unnecessary requests.

### WebSockets

**Advantages:** Low-latency server push and bidirectional communication.

**Disadvantages:** Requires connection lifecycle, reconnect/catch-up, authorization, fan-out, and capacity management.

## Decision

No option has been approved. **Draft recommendation:** Evaluate polling first, not an unconditional selection. Choose it only if an agreed interval satisfies freshness and measured server capacity; otherwise evaluate WebSockets. Do not invent a polling interval or concurrency target.

## Consequences

The following are anticipated consequences of the draft recommendation, not observed implementation results.

### Positive

- Can avoid persistent-connection operations for an initial workload that tolerates polling.
- Keeps a conventional request/response path.

### Negative / trade-offs

- Changes are not instantaneous.
- Request volume may dominate at high client counts even when changes are rare.

### Follow-up work

- Establish freshness expectations, concurrent active clients, and change frequency.
- Model polling request rate as active polling clients / interval in seconds, then include tabs, retries, and response costs.
- Compare representative polling load with WebSocket connection and event-delivery costs.
- Specify reconnect catch-up, duplicate/out-of-order handling as relevant, and permission removal behavior.

## References

- [Issue #3](https://github.com/systemdesign-diagram/Todo-App/issues/3), including comments checked on 2026-11-22: none present.
- [Technical design discussions, section 3](https://github.com/systemdesign-diagram/Todo-App/blob/f5adb18f0792685c3900242286551ee62c5ae324/design-discussions.md).
- [Architecture overview](https://github.com/systemdesign-diagram/Todo-App/blob/f5adb18f0792685c3900242286551ee62c5ae324/overview.md).
- [Relevant Slack discussion](https://app.slack.com/archives/C0CL0DFRL1G/p1795370598899719).
