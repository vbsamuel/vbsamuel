# Systems

Most active source lives in private repositories. This page records the engineering substance: ownership boundaries, runtime paths, failure models, and the invariants I am trying to preserve.

## Nhilith — Non-Human Authority Governance

Nhilith exists because a machine identity being authenticated does not answer the harder question: **is this exact operation still authorized, under this exact context, against this exact predecessor state, at this exact authority/revocation epoch?**

Canonical path:

```text
identity / trigger
    ↓
execution + correlation + causation identity
    ↓
resolved execution context
    ↓
authorization + delegated scope
    ↓
policy / authority / projection / revocation epochs
    ↓
replay + idempotency commitment
    ↓
predecessor-bound state transition
    ↓
canonical durable authority transaction
    ↓
external-effect authorization / dispatch
    ↓
observed external outcome + reconciliation
    ↓
evidence / receipt / recovery state
```

The failure cases drive the architecture:

- a valid credential with stale authority;
- child delegation exceeding parent authority;
- revocation racing an already-running operation;
- a process dying after durable commit but before effect observation;
- duplicate/replayed commands;
- old projections appearing current;
- sequence or epoch arithmetic overflow;
- a compatibility path becoming an accidental second writer.

Recent source work reflects those concerns directly: projection writes have been collapsed behind the canonical store owner; policy changes are persisted as canonical domain events rather than audit-only receipts; old writer/error states are being deleted instead of kept as compatibility semantics; mutation-sequence overflow now fails closed.

The core rule is **authority must be reconstructable after failure**. If restart/replay cannot determine the authorized predecessor, committed mutation, unresolved effect, and current revocation state, the design is incomplete.

---

## HOLI — Heterogeneous Output Lineage Indexer

HOLI handles the execution/lineage problem when many agents, tools, processes, and people create work concurrently.

```text
work / causation identity
    ↓
append-only journal + CAS
    ↓
execution admission
    ↓
bounded process supervision
    ↓
work product + causal lineage
    ↓
external-effect reservation
    ↓
effect dispatch
    ↓
readback / settlement
    ↓
obligation closure
    ↓
restart-safe recovery
```

Important mechanics include:

- immutable objects and content-addressed work products;
- causal lineage rather than "last message wins" history;
- durable WIP and execution receipts;
- resource admission and host/process containment;
- currentness and fencing before non-commutative mutation;
- reservation before externally protected effects;
- settlement/reconciliation after observation;
- crash recovery that rebuilds state before permitting duplicate work;
- operator-visible evidence rather than internal-only success.

A historical Linux/WSL baseline was directly qualified at an immutable SHA across Rust compile, strict Clippy, workspace tests, Go tests, systemd recovery tests, DISERI bridge tests, and source-law checks. Later `main` changes intentionally do **not** inherit that evidence; they must be re-qualified at their own exact source identity.

One recent falsification test explicitly verifies invalid systemd recovery configuration and zero revoke timeout fail closed before recovery proceeds. That is the kind of test I value: not "does the happy path run?" but "can unsafe configuration accidentally produce success?"

---

## DISERI / EEF — Event Execution Fabric

DISERI/EEF explores a different systems boundary: how much duplication disappears if message delivery, workflow execution, state transitions, incremental computation, replay, recovery, and external-effect finality share one typed semantic model?

```text
interaction
    ↓
semantic work identity
    ↓
durable event / transition
    ↓
causal + time frontier
    ↓
incremental state operator
    ↓
workflow / service execution
    ↓
external-effect finality
    ↓
semantic commit
    ↓
replay / recovery / distributed handoff
```

Key ideas:

- messages are delivery manifestations; semantic truth is interaction/state transition/evidence;
- equivalent semantic work should execute once within an explicit equivalence scope;
- acknowledgements do not imply execution or business outcome;
- causal/time frontiers make incompleteness explicit;
- incremental operators propagate deltas instead of recomputing full state when the algebra permits it;
- effect finality is separate from transport delivery;
- unknown/stale/contradictory state remains explicit instead of being coerced to success.

A recent ownership refactor moved build-routing policy entirely under `WorkflowDispatchServiceV1`. Routing validation, deterministic ramp selection, pinned-workflow selection, and persisted dispatch binding now live with the dispatch owner instead of being duplicated between runtime and worker layers.

That change is small in lines of code and large architecturally: duplicate policy ownership is how distributed systems accumulate contradictory truth.

---

## Amalgus — AI-native workbench and execution substrate

Amalgus is the product-level system that asks a broader question: can AI work operate on **real artifacts and real objectives** instead of collapsing everything into a chat transcript?

Input can be a repository, model, data set, document corpus, video, device, investigation, or technical problem.

```text
objective + constraints + artifacts
    ↓
reconstruct current work state
    ↓
acquire only relevant context
    ↓
generate bounded candidate actions / transformations
    ↓
execute
    ↓
measure / observe
    ↓
independent evaluator or source readback
    ↓
verified result / explicit unresolved state
```

Three state machines are deliberately separate:

```text
Work    — lifecycle of the user objective
Step    — one bounded unit of execution
Effect  — one externally visible state change
```

The separation matters. A `Step` can finish while its `Effect` remains ambiguous. The runtime must then enter reconciliation, not mark the `Work` complete and not blindly retry.

Other core mechanics:

- stable semantic identities for effects and retries;
- append-only execution/effect evidence;
- leases/fencing for active execution ownership;
- compensation only where a true inverse exists;
- multi-device work-state synchronization;
- privacy zones separating ephemeral/private/shared/system-of-record state;
- local/deterministic/specialized/private/remote model routing;
- proof packages that bind artifact revision, inputs, evaluator, metrics, environment, and unresolved uncertainty.

The product rule is equally important: **Amalgus must create value before the customer changes anything.** No mandatory process DSL, no required workflow migration, no replacement source of truth, and no requirement to open Amalgus just to learn what Jira/ServiceNow/GitHub already say.

---

## UDRL — Universal Dynamic Representation Layer

UDRL is the representation substrate for state that is continuously changing, multimodal, partially observed, and projected into multiple product surfaces.

The main concern is not another graph model; it is preventing representation from becoming another competing truth owner.

Core rules:

```text
one semantic concept      → one canonical owner
one configuration domain  → one configuration authority
one mutation domain       → one authoritative write path
projection                → read/derived view, never hidden authority
unknown                   → remains unknown
fallback                  → explicit and typed, never silent success
```

The implementation program also uses unusually strict source rules: bounded externally influenced resources, checked failure paths, explicit configuration authority, no fabricated completion, and independent closure/falsification rather than source-presence claims.

---

## Engineering patterns across the systems

### 1. Single ownership beats synchronization of duplicate truth

If two modules independently own the same policy, retry state, configuration, authority, or durable journal, the real problem is not synchronization — it is ownership. I prefer deleting one owner over inventing reconciliation between both.

### 2. Idempotency is semantic

```text
same HTTP request twice        ≠ necessarily same work
same semantic effect identity  = candidate for safe dedupe/reconcile
```

Idempotency keys must represent the effect the domain considers identical, not merely the transport attempt.

### 3. External effects need a journal

For consequential effects:

```text
reserve effect identity durably
→ dispatch
→ observe/read back
→ settle success / failure / unknown
→ reconcile unknown before retry
```

A timeout after dispatch is not proof that nothing happened.

### 4. Fencing is required when old owners can survive

Leases alone do not stop an old worker from waking up. State-changing operations carry a fence/epoch so stale actors are rejected at the mutation boundary.

### 5. Evidence belongs to the same source identity as the claim

Passing tests on SHA `A` do not qualify SHA `B`. A release claim binds source revision, configuration, toolchain, platform, test/evaluator versions, and evidence artifacts.

### 6. Recovery is part of the normal state machine

Crash/restart is not an exception bolt-on. Durable systems should be able to reconstruct:

- accepted work;
- current owner/fence;
- committed state;
- unsettled effects;
- replay cursor/frontier;
- obligations still open;
- evidence needed to continue safely.

### 7. "Success" is typed

These are different states:

```text
accepted
dispatched
acknowledged
committed
observed
verified
settled
```

Collapsing them into one boolean is how systems lie to their operators.

---

## Background

My product/engineering background spans Meta, PayPal, eBay, XR/spatial systems, payments, security/risk, developer platforms, data products, and large cross-functional launches. I also hold an MBA from Wharton and an MS in Electrical Engineering from USC.

The systems above are where I spend my engineering time now.