# ADR-0006: Define task and project sharing authorization

- **Status:** Proposed
- **Date:** 2026-11-22
- **Decision owners:** Not assigned in source material; Neo Kim's approval required before GitHub changes.
- **Related issue(s):** [Issue #6](https://github.com/systemdesign-diagram/Todo-App/issues/6)

## Context

Issue #6 requires sharing tasks and projects with appropriate access control. Notes compare roles with ownership/policy-based rules and list owner/member/viewer only as examples. The overview mentions owners, members, and invited collaborators. The exact role set, inheritance, and task-level exceptions are unresolved.

## Decision drivers

- Correct, understandable resource access.
- Sharing workflows, ownership, memberships, and permission inheritance.
- Testability, revocation behavior, and possible audit requirements.

## Considered options

### Role-based permissions

**Advantages:** A small role matrix is understandable and easy to test when permissions are uniform.

**Disadvantages:** Coarse roles can become unwieldy for resource-specific sharing exceptions.

### Ownership / resource policies

**Advantages:** Expresses relationships and contextual sharing rules directly.

**Disadvantages:** Policies become difficult to reason about if duplicated or scattered.

### Scoped roles plus explicit ownership/membership checks

**Advantages:** Combines a compact action matrix with resource-scope checks.

**Disadvantages:** Still requires precise precedence, inheritance, and exception rules.

## Decision

No option has been approved. **Draft recommendation:** Compare a hybrid of scoped roles and explicit ownership/membership policies against actual workflows. Treat the role names as provisional. Centralize authorization, deny unmatched access, and evaluate permissions for the actual resource rather than trusting the login state. These are review recommendations, not accepted policy.

## Consequences

The following are anticipated consequences of the draft recommendation, not observed implementation results.

### Positive

- A centralized action/resource policy can be tested consistently.
- Resource scoping helps prevent cross-workspace access.

### Negative / trade-offs

- Inheritance and direct-share exceptions can create subtle conflicts.
- Overly broad roles or stale authorization caches can undermine revocation.

### Follow-up work

- Build a role × action × resource matrix, including invitations, ownership transfer, deletion, and member removal.
- Decide whether direct task sharing can extend access beyond project/workspace membership.
- Specify inheritance and precedence explicitly.
- Test denied cross-workspace reads/writes and access after removal; coordinate with ADR-0002.

## References

- [Issue #6](https://github.com/systemdesign-diagram/Todo-App/issues/6), including comments checked on 2026-11-22: none present.
- [Technical design discussions, section 6](https://github.com/systemdesign-diagram/Todo-App/blob/f5adb18f0792685c3900242286551ee62c5ae324/design-discussions.md).
- [Architecture overview](https://github.com/systemdesign-diagram/Todo-App/blob/f5adb18f0792685c3900242286551ee62c5ae324/overview.md).
