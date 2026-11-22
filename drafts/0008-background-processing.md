# ADR-0008: Choose execution for reminders and deferred work

- **Status:** Proposed
- **Date:** 2026-11-22
- **Decision owners:** Not assigned in source material; Neo Kim's approval required before GitHub changes.
- **Related issue(s):** [Issue #8](https://github.com/systemdesign-diagram/Todo-App/issues/8)

## Context

Issue #8 identifies reminders and scheduled notifications as potential work outside API requests. Notes ask about execution time, retries, scheduling, idempotency, duplicate delivery, and failures. The initial job catalogue, delivery guarantees, infrastructure, and volume remain unverified.

## Decision drivers

- Scheduling, reliable retries, user-request latency, and diagnosable failures.
- Idempotency, delivery guarantees, job volume, and operational capacity.

## Considered options

### Synchronous request processing

**Advantages:** Simple for bounded immediate operations; errors return directly to the caller.

**Disadvantages:** Unsuitable by itself for future reminders or work needing durable retries after a request ends.

### Database-backed durable jobs with a worker/scheduler

**Advantages:** Can reuse the primary store while persisting scheduled work and retry state.

**Disadvantages:** Requires safe claiming, concurrency control, retry scheduling, cleanup, and monitoring.

### Dedicated queue with workers and scheduling support

**Advantages:** Separates job execution and may fit larger throughput/reliability needs.

**Disadvantages:** Adds infrastructure; durability, scheduling, duplicate handling, and observability still require explicit design.

## Decision

No option has been approved. **Draft recommendation:** Keep short bounded operations synchronous; evaluate durable asynchronous execution for confirmed scheduled/reminder or retryable jobs. Compare database-backed and dedicated-queue approaches after requirements are known. No queue product or delivery guarantee is selected.

## Consequences

The following are anticipated consequences of the draft recommendation, not observed implementation results.

### Positive

- Durable scheduling can survive user-request completion and permit controlled retries.
- Separating slow external work can protect interactive latency.

### Negative / trade-offs

- Workers introduce eventual completion, duplicate-execution risks, and operational work.
- Database-backed jobs compete with application traffic; a dedicated queue adds a dependency.

### Follow-up work

- Confirm which jobs ship initially, their timing tolerance, volume, and failure impact.
- Define idempotency keys, retry/backoff limits, terminal failures, timeouts, and monitoring.
- Specify how jobs stay consistent with committed application state; evaluate an outbox if needed.
- Check recipient access when notifications execute and choose infrastructure only after comparison.

## References

- [Issue #8](https://github.com/systemdesign-diagram/Todo-App/issues/8), including comments checked on 2026-11-22: none present.
- [Technical design discussions, section 8](https://github.com/systemdesign-diagram/Todo-App/blob/f5adb18f0792685c3900242286551ee62c5ae324/design-discussions.md).
- [Architecture overview](https://github.com/systemdesign-diagram/Todo-App/blob/f5adb18f0792685c3900242286551ee62c5ae324/overview.md).
