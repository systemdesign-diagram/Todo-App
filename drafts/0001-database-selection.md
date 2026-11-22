# ADR-0001: Select primary persistent storage

- **Status:** Proposed
- **Date:** 2026-11-22
- **Decision owners:** Not assigned in source material; Neo Kim's approval required before GitHub changes.
- **Related issue(s):** [Issue #1](https://github.com/systemdesign-diagram/Todo-App/issues/1)

## Context

The model includes users, tasks, projects, and workspace memberships. The notes explicitly leave PostgreSQL and MongoDB open. In Slack, Neo leans toward clean relationship handling; Gencay suggests PostgreSQL and Emily requests a relational-query versus document-flexibility comparison. This is a preference, not consensus or approval.

## Decision drivers

- Relationship integrity and consistency across memberships, ownership, and tasks.
- Query/filtering requirements, migrations, maintenance, and hosting fit.

## Considered options

### PostgreSQL

**Advantages:** Foreign keys, transactions, and joins naturally support a relational model; JSON fields can accommodate some variable metadata.

**Disadvantages:** Requires explicit schema evolution, migration discipline, and query/index design.

### MongoDB

**Advantages:** Flexible document structures may suit materially variable task metadata.

**Disadvantages:** Cross-resource relationships require deliberate embedding/reference and consistency design; flexibility alone does not establish a better fit.

## Decision

No option has been approved. **Draft recommendation:** PostgreSQL, conditional on validating representative queries, transaction boundaries, hosting, and team experience. Variable metadata alone is insufficient evidence to choose MongoDB.

## Consequences

The following are anticipated consequences of the draft recommendation, not observed implementation results.

### Positive

- Centralized relational constraints can protect membership and task relationships.
- One primary store can cover the initial domain.

### Negative / trade-offs

- Schema changes need controlled migrations.
- Performance remains workload-dependent; no benchmark exists in the reviewed material.

### Follow-up work

- Sketch the entity model and deletion/ownership invariants.
- Compare task-list, membership, assignment, and sharing queries.
- Confirm hosting and maintenance constraints before acceptance.

## References

- [Issue #1](https://github.com/systemdesign-diagram/Todo-App/issues/1), including comments checked on 2026-11-22: none present.
- [Technical design discussions, section 1](https://github.com/systemdesign-diagram/Todo-App/blob/f5adb18f0792685c3900242286551ee62c5ae324/design-discussions.md).
- [Architecture overview](https://github.com/systemdesign-diagram/Todo-App/blob/f5adb18f0792685c3900242286551ee62c5ae324/overview.md).
- [Relevant Slack discussion](https://app.slack.com/archives/C0CL0DFRL1G/p1795370526539109).
