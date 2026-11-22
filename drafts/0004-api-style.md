# ADR-0004: Select the client API style

- **Status:** Proposed
- **Date:** 2026-11-22
- **Decision owners:** Not assigned in source material; Neo Kim's approval required before GitHub changes.
- **Related issue(s):** [Issue #4](https://github.com/systemdesign-diagram/Todo-App/issues/4)

## Context

Issue #4 and technical notes compare REST and GraphQL. The product has task, project, workspace, and membership responsibilities, but client count, screen data-fetching patterns, and API evolution needs are unverified. No recorded preference or decision was found.

## Decision drivers

- Fit to actual client views and resource operations.
- Authorization, pagination, filtering, maintainability, observability, and evolution.

## Considered options

### REST

**Advantages:** Resource-oriented endpoints fit common CRUD interactions and use familiar HTTP conventions.

**Disadvantages:** Complex screens may require multiple requests or tailored aggregate endpoints; conventions need discipline.

### GraphQL

**Advantages:** Clients can request nested data shaped to different views.

**Disadvantages:** Requires schema/resolver design, field-level authorization, query-cost controls, and deliberate caching/observability.

## Decision

No option has been approved. **Draft recommendation:** Use REST as the comparison baseline for the initial resource-oriented client. Accept only after mapping real screens to requests. Prefer GraphQL if measured client-fetching needs justify its extra controls. This recommendation originates in this review, not in recorded team agreement.

## Consequences

The following are anticipated consequences of the draft recommendation, not observed implementation results.

### Positive

- A REST baseline can keep initial API operations explicit and narrow.
- Endpoint-level metrics and authorization are straightforward to structure.

### Negative / trade-offs

- Some views may need aggregate endpoints or multiple requests.
- REST does not remove the need for consistent contracts or resource-level authorization.

### Follow-up work

- Inventory initial clients and screen-level data requirements.
- Compare representative payloads, request counts, and evolution scenarios.
- Specify filtering, pagination, error format, and compatibility/versioning conventions before acceptance.

## References

- [Issue #4](https://github.com/systemdesign-diagram/Todo-App/issues/4), including comments checked on 2026-11-22: none present.
- [Technical design discussions, section 4](https://github.com/systemdesign-diagram/Todo-App/blob/f5adb18f0792685c3900242286551ee62c5ae324/design-discussions.md).
- [Architecture overview](https://github.com/systemdesign-diagram/Todo-App/blob/f5adb18f0792685c3900242286551ee62c5ae324/overview.md).
