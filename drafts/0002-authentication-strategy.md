# ADR-0002: Choose authentication and session management

- **Status:** Proposed
- **Date:** 2026-11-22
- **Decision owners:** Not assigned in source material; Neo Kim's approval required before GitHub changes.
- **Related issue(s):** [Issue #2](https://github.com/systemdesign-diagram/Todo-App/issues/2)

## Context

Issue #2 compares server-side sessions with JWT, including expiration, revocation, security, and multiple devices. Neo specifically requires effective logout and removal of access after workspace membership changes. Required client platforms and deployment topology remain unspecified. Authentication does not by itself establish authorization to a workspace.

## Decision drivers

- Reliable logout, expiration, and revocation.
- Current workspace authorization after membership changes.
- Client compatibility, multiple devices, and deployment complexity.

## Considered options

### Server-side sessions

**Advantages:** Central session state supports revocation and device-session management.

**Disadvantages:** Requires a shared session store when scaling across instances; cookie-based browser sessions require CSRF defenses.

### JWT-based authentication

**Advantages:** Signed tokens allow local validation across independent API consumers.

**Disadvantages:** Immediate revocation requires additional mechanisms; refresh-token rotation, key management, and stale claims add complexity.

## Decision

No option has been approved. **Draft recommendation:** Server-side sessions if the first client is a browser application. Do not require Redis solely for sessions; assess a database-backed store. Reassess JWT if verified client or service requirements justify it. Check current resource permissions separately from identity on protected operations.

## Consequences

The following are anticipated consequences of the draft recommendation, not observed implementation results.

### Positive

- Straightforward centralized invalidation and explicit session lifetimes.
- Avoids assuming a token's membership claims remain current.

### Negative / trade-offs

- Adds session-store availability and lookup costs.
- Cookie, CSRF, secure transport, and multi-instance behavior still require design.

### Follow-up work

- Confirm browser/native clients and deployment topology.
- Specify per-device logout, global logout, expiration, and revocation behavior.
- Define cookie protections and CSRF controls where applicable.
- Test membership removal against active sessions and protected resources.

## References

- [Issue #2](https://github.com/systemdesign-diagram/Todo-App/issues/2), including comments checked on 2026-11-22: none present.
- [Technical design discussions, section 2](https://github.com/systemdesign-diagram/Todo-App/blob/f5adb18f0792685c3900242286551ee62c5ae324/design-discussions.md).
- [Architecture overview](https://github.com/systemdesign-diagram/Todo-App/blob/f5adb18f0792685c3900242286551ee62c5ae324/overview.md).
- [Relevant Slack discussion](https://app.slack.com/archives/C0CL0DFRL1G/p1795370593098819).
