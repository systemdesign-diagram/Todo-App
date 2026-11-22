# ADR-0005: Choose initial backend deployment boundaries

- **Status:** Proposed
- **Date:** 2026-11-22
- **Decision owners:** Not assigned in source material; Neo Kim's approval required before GitHub changes.
- **Related issue(s):** [Issue #5](https://github.com/systemdesign-diagram/Todo-App/issues/5)

## Context

The overview assumes a small team and warns that conceptual components do not imply separate services. Issue #5 leaves monolith versus microservices open. Gencay suggests modules for authentication, tasks, workspaces, and notifications; the discussion acknowledges microservice operational overhead. No owner approval is recorded.

## Decision drivers

- Small-team maintainability and operational capacity.
- Deployment/debugging simplicity and enforceable internal boundaries.
- Demonstrated independent deployment, scaling, and reliability needs.

## Considered options

### Modular monolith

**Advantages:** One application deployment avoids interservice communication for ordinary domain operations while retaining logical separation.

**Disadvantages:** Shared deployment and resource/failure boundaries; module boundaries require enforcement.

### Microservices

**Advantages:** Independent deployment and scaling for components with distinct needs.

**Disadvantages:** Adds network failure modes, data ownership decisions, distributed observability, and deployment coordination.

## Decision

No option has been approved. **Draft recommendation:** A modular monolith for the initial backend, subject to validating the small-team assumption and deployment requirements. Keep authentication/authorization, tasks/projects, workspaces, and notifications as explicit logical responsibilities; exact boundaries remain a design task.

## Consequences

The following are anticipated consequences of the draft recommendation, not observed implementation results.

### Positive

- Reduces initial deployment and distributed-systems overhead.
- Can support straightforward cross-domain transactions in the chosen store.

### Negative / trade-offs

- Scaling and releases are coupled initially.
- Unenforced boundaries can produce a tightly coupled monolith.

### Follow-up work

- Confirm team/hosting constraints and any component requiring independent scale.
- Define module interfaces, dependency direction, and data ownership.
- Revisit extraction only when concrete scaling, ownership, or reliability evidence warrants it.

## References

- [Issue #5](https://github.com/systemdesign-diagram/Todo-App/issues/5), including comments checked on 2026-11-22: none present.
- [Technical design discussions, section 5](https://github.com/systemdesign-diagram/Todo-App/blob/f5adb18f0792685c3900242286551ee62c5ae324/design-discussions.md).
- [Architecture overview](https://github.com/systemdesign-diagram/Todo-App/blob/f5adb18f0792685c3900242286551ee62c5ae324/overview.md).
