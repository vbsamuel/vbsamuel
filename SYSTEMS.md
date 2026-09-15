# Systems I’m Building

Most of my active systems work lives in private repositories. This page describes the engineering problems and architectural boundaries without exposing proprietary source.

## Amalgus — AI-native workbench and execution substrate

**Problem:** complex technical and product work is fragmented across code, models, documents, data, video, tools, devices, and interrupted human context. Most AI products answer questions or generate artifacts but stop before measured, independently verified completion.

**System:** Amalgus is a local-first AI-native workbench designed to take real work from objective and evidence through investigation, transformation, execution, measurement, verification, and reusable learning.

**Representative architecture:**

- persistent work-state and context reconstruction;
- evidence-bearing reasoning rather than answer-only generation;
- deterministic/local/specialized/private/remote intelligence routing;
- artifact and candidate search with explicit constraints and evaluators;
- durable Work / Step / Effect finite-state machines;
- semantic idempotency identities, effect journals, reconciliation, compensation, leases and fencing;
- multi-device synchronization and privacy-zone boundaries;
- proof-carrying outputs with independent read-back verification.

---

## Nhilith — Non-Human Authority Governance

**Problem:** autonomous software can authenticate successfully and still perform an action with stale, over-broad, replayed, inherited, revoked, or otherwise invalid authority.

**System:** Nhilith makes identity, delegation, authorization, revocation, execution context, state transition, durable effects, evidence and recovery explicit across authority-bearing software execution.

**Representative architecture:**

- verifiable machine/workload identity and attestation;
- canonical authorization evidence and scoped delegation;
- policy, authority, projection and revocation epochs;
- predecessor-bound state transitions;
- replay/idempotency commitment before governed mutation;
- durable authority transactions and external-effect authorization;
- signed human-control / break-glass paths;
- recovery, quarantine, kill, expiry and operator evidence.

---

## HOLI — Heterogeneous Output Lineage Indexer

**Problem:** many agents, tools, workers, processes and humans can create outputs concurrently, but conventional workflow systems lose attribution, causal lineage, recoverability and exact effect history as work forks, retries and converges.

**System:** HOLI is a coordination, lineage, persistence, recovery and execution substrate for heterogeneous AI workloads and workflows.

**Representative architecture:**

- immutable objects, snapshots and causal lineage;
- concurrent work, branching, alternatives and fan-in/fan-out;
- durable journals, CAS, registries and recovery authority;
- exact execution admission and bounded supervision;
- obligation, settlement and closure semantics;
- external-effect reservation and reconciliation;
- resource accounting, currentness and fencing;
- crash/restart recovery across Linux/WSL and Windows-oriented execution paths.

---

## DISERI / EEF — Event Execution Fabric

**Problem:** event transport, queues, workflow engines, state machines, incremental computation, replay, recovery and external-effect handling are usually separate systems with duplicate state and amplification across each boundary.

**System:** DISERI is the broader data integration, streaming and event-relay program. Its first product, EEF, is a typed execution fabric unifying durable event transport, trigger execution, workflow/state-machine execution, stateful incremental computation, replay/recovery, service interaction, unique-work collapse and effect reconciliation.

**Representative architecture:**

- interaction and state-transition semantics above raw message delivery;
- exact unique-work reservation;
- incremental operators and causal/time frontiers;
- typed event/workflow execution contracts;
- durable effect finality and replay/recovery;
- embedded, edge, enterprise-cell and federated deployment profiles;
- governed integration with Nhilith where full non-human authority is required.

---

## UDRL — Universal Dynamic Representation Layer

**Problem:** continuously changing multimodal work cannot be represented well by static documents, one-off prompt context, or product-specific UI models.

**System:** UDRL is a product-independent computational representation substrate for evolving multimodal state, reasoning, interaction, materialization and native surfaces.

**Representative architecture principles:**

- one canonical owner per semantic concept and configuration domain;
- configuration-defined operating policy instead of hard-coded behavior;
- bounded externally influenced resources;
- explicit state and semantic contracts;
- independent validation and falsification;
- product-independent SDK/capability boundaries.

---

## Common engineering themes

Across these systems, the recurring concern is not "adding an agent." It is making autonomous or AI-assisted software behave like production infrastructure:

- explicit ownership and authority;
- deterministic state transitions;
- bounded resource use;
- semantic idempotency;
- durable recovery and replay;
- external-effect reconciliation;
- local/private execution where appropriate;
- evidence, falsification and exact-source qualification;
- real operator surfaces and end-to-end vertical closure.

I treat product definition, architecture, runtime semantics, testability, operability, performance, failure behavior and release evidence as one engineering problem.