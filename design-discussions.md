# Technical Design Discussions

**Status:** Open questions — not approved decisions  
**Purpose:** Capture the alternatives, constraints, and questions that should be investigated before writing ADRs.

> Important: The options below are starting points for discussion, not recommendations or decisions. Verify project requirements and current implementation evidence before drafting an ADR.

## 1. Database: PostgreSQL vs MongoDB

**Problem:** Choose persistent storage for users, tasks, projects, memberships, and activity data.

**PostgreSQL — questions / potential advantages**
- How naturally do relational constraints and transactions fit task ownership, memberships, and permissions?
- Would relational querying simplify reporting and filtering?
- What schema evolution and migration workflow would the team use?

**MongoDB — questions / potential advantages**
- Would document-oriented records make any domain data materially simpler?
- Are task and project structures expected to vary substantially?
- How would references, consistency, and multi-document updates be handled?

**Evidence needed:** expected data relationships, query patterns, consistency requirements, hosting constraints, team experience.

## 2. Authentication: server sessions vs JWT

**Problem:** Decide how the application represents and validates authenticated users.

**Server-side sessions**
- Where will session state live?
- How will sessions work across multiple application instances?
- What are the cookie, CSRF, expiration, and revocation requirements?

**JWT-based authentication**
- Which claims are needed, and how short-lived will tokens be?
- How will refresh, rotation, revocation, and key management work?
- Does the client need to call multiple independent services?

**Evidence needed:** client types, deployment topology, security requirements, session revocation expectations.

## 3. Real-time updates: WebSockets vs polling

**Problem:** Decide how connected users learn about task changes.

**WebSockets**
- Is bidirectional, low-latency communication actually required?
- How will reconnects, authorization, scaling, and missed events be handled?
- What infrastructure and operational cost does persistent connectivity introduce?

**Polling**
- How frequently would clients poll?
- Is the expected update latency acceptable?
- What request volume would polling create at expected usage?

**Evidence needed:** product latency expectations, concurrent-user estimates, event frequency, hosting capabilities.

## 4. API style: REST vs GraphQL

**Problem:** Choose the API interaction model for the client and server.

**REST**
- Are resource-oriented endpoints sufficient for the client screens?
- How will filtering, pagination, versioning, and over-fetching be handled?
- Can the team keep endpoint conventions consistent?

**GraphQL**
- Do clients need flexible nested data selection or multiple client-specific views?
- How will authorization, query complexity, caching, and observability be managed?
- Is the added schema and resolver layer justified?

**Evidence needed:** number of clients, data-fetching patterns, API evolution needs, team familiarity.

## 5. Deployment architecture: monolith vs microservices

**Problem:** Choose an initial deployment boundary for the application.

**Modular monolith**
- Can clear internal modules preserve separation without network boundaries?
- What deployment and debugging simplicity does one application provide?
- How will module boundaries be enforced?

**Microservices**
- Is independent deployment or scaling needed by a specific component?
- Can the team support service discovery, network failures, distributed tracing, and data ownership?
- What operational overhead is justified by current requirements?

**Evidence needed:** team size, independent scaling needs, deployment frequency, reliability requirements, operational capacity.

## 6. Permissions: role-based vs ownership-based / policy-based

**Problem:** Define who can view and change tasks, projects, and memberships.

**Role-based approach**
- Which roles are needed (for example owner, member, viewer)?
- Are permissions consistent across all resources?
- How will role changes and invitations be handled?

**Ownership / policy-based approach**
- Are permissions primarily determined by resource ownership and membership?
- Are there resource-specific rules that require more flexible policies?
- How will rules remain testable and understandable?

**Evidence needed:** sharing workflows, expected roles, resource hierarchy, audit requirements.

## 7. Caching: Redis vs database-backed / no dedicated cache

**Problem:** Determine whether frequently accessed data needs a separate cache.

**Redis**
- Which reads are demonstrably expensive or frequent?
- How will invalidation, expiration, and stale data be handled?
- Is a separate service operationally justified?

**Database-backed or no dedicated cache initially**
- Can indexes and query optimization meet initial performance needs?
- Would a simple database-backed mechanism be sufficient for the first release?
- What measurements would trigger adding a cache?

**Evidence needed:** read/write patterns, performance measurements, consistency needs, hosting budget.

## 8. Background processing: synchronous vs job queue

**Problem:** Decide how to execute reminders, scheduled work, and tasks that should not block a user request.

**Synchronous processing**
- Which operations are fast and reliable enough to complete within a request?
- What happens if an external dependency is slow?

**Background job queue**
- Which operations need retries, scheduling, or independent execution?
- How will idempotency, duplicate delivery, failure handling, and monitoring work?
- Is a dedicated queue justified for the initial workload?

**Evidence needed:** job types, execution-time expectations, retry requirements, delivery guarantees.

## Information still missing

Before finalizing any ADR, confirm:
- Expected users and concurrent sessions.
- Target deployment / hosting environment and budget.
- Required client platforms.
- Team skills and operational capacity.
- Security, privacy, and data-retention requirements.
- Any implementation already present in the repository.

## Review rule

An ADR must distinguish facts from assumptions, cite the issue or source material that supports the context, compare realistic alternatives, and state consequences. Do not invent a consensus or mark a decision approved without owner review.
