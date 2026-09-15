# Bruno Samuel

I build **runtime infrastructure for AI-native systems**: durable execution, machine authority, causal lineage, event fabrics, multimodal state, recovery, and independently verifiable effects.

Most of the work below is in private repositories. I describe the mechanics here because the interesting part is not the project name — it is the invariants the system has to preserve when processes crash, work retries, authority changes, effects become ambiguous, or multiple actors mutate state concurrently.

`Rust` · `Go` · `Python` · `TypeScript` · `WASM` · Linux/WSL · Windows · local/edge inference

---

## Current engineering

### Nhilith — authority for non-human execution

Authentication is not enough for software that can change real state. Nhilith treats identity, delegation, policy, revocation, predecessor state, replay protection, durable mutation, external effects, and recovery as one authority path.

```text
subject + execution context
        ↓
canonical authorization / delegation
        ↓
epoch + revocation + predecessor checks
        ↓
replay / idempotency commitment
        ↓
durable authority mutation
        ↓
external effect authorization
        ↓
observed outcome / reconciliation
        ↓
evidence + recovery state
```

Recent implementation work includes collapsing policy mutation onto a canonical store owner, persisting policy changes as durable domain events, removing compatibility-owned writers, and making mutation-sequence overflow fail closed rather than wrap or silently clamp.

The design question is simple: **after a restart, revocation, retry, or partial failure, can the system still prove who had authority to do what — and whether the effect actually happened?**

### HOLI — lineage, execution, and recovery for heterogeneous work

HOLI coordinates outputs created concurrently by agents, tools, processes, and humans without losing attribution or effect history as work forks, retries, and converges.

```text
work identity
   ↓
journal / CAS / lineage
   ↓
admission + bounded supervision
   ↓
reservation of externally visible effect
   ↓
execution
   ↓
readback / settlement
   ↓
closure + recoverable evidence
```

The runtime spans Rust and Go and includes durable journals, content-addressed storage, execution admission, process supervision, resource accounting, external-effect reservation/settlement, currentness/fencing, crash recovery, and operator readback. One Linux/WSL baseline has been qualified at an immutable SHA across compile, strict Clippy, workspace tests, Go tests, recovery tests, and source-law checks; later source is deliberately not allowed to inherit that evidence.

### DISERI / EEF — event execution fabric

EEF is an attempt to stop treating transport, queues, workflow execution, state machines, incremental computation, replay, and effect reconciliation as unrelated layers with duplicate state and duplicate ownership.

```text
interaction
   ↓
exact work identity
   ↓
durable event / state transition
   ↓
incremental operator + causal/time frontier
   ↓
workflow / service execution
   ↓
external effect finality
   ↓
replay / recovery / handoff
```

A current refactor moved build-routing policy under the dispatch owner instead of allowing worker/runtime layers to duplicate it. The dispatch path now owns validation, deterministic ramp selection, pinned workflow routing, and the persisted binding. That kind of ownership cleanup matters more to me than adding another abstraction layer.

### Amalgus — AI-native workbench

Amalgus is the product layer I am designing around the same runtime concerns: take an actual body of work — repository, model, dataset, document corpus, video, device, or technical problem — and move it to a measured result rather than stop at generation.

Its execution model separates **Work**, **Step**, and **Effect** state, uses stable semantic identities for idempotency, treats ambiguous external effects as a reconciliation problem rather than a blind-retry problem, and keeps proof/evidence separate from model output.

```text
objective + artifacts
      ↓
understand / investigate
      ↓
generate bounded alternatives
      ↓
execute / measure
      ↓
independent evaluator or readback
      ↓
verified result + reproducible evidence
```

### UDRL — dynamic multimodal representation

UDRL is a product-independent substrate for continuously evolving multimodal state. The important design constraint is ownership: one canonical owner for a semantic concept, configuration domain, and mutation path; product surfaces consume typed capability contracts instead of creating parallel truth.

[Deeper system notes](SYSTEMS.md)

---

## Invariants I keep coming back to

These are the kinds of distinctions that determine whether an AI/system demo survives contact with production:

```text
authenticated caller        ≠ authorized operation
message delivered           ≠ work executed
command returned success    ≠ external effect observed
retry                       ≠ idempotency
model confidence            ≠ evidence
projection                  ≠ authority
source exists               ≠ production path is reachable
compile passes               ≠ runtime is qualified
current main                ≠ an older SHA's test evidence
```

For state-changing paths I prefer:

- **one writer / one semantic owner** instead of mirrored policy or state;
- explicit FSM transitions instead of implicit lifecycle flags;
- stable semantic effect IDs before dispatch;
- append-only evidence where mutation history matters;
- fencing/epochs where stale actors can still be alive;
- readback before claiming a real-world effect;
- `UNKNOWN_EFFECT` / reconciliation rather than unsafe retry;
- bounded resources and checked arithmetic on correctness-bearing paths;
- restart/replay/fault behavior designed with the happy path, not after it;
- qualification bound to the exact source/config/platform being claimed.

A system is not finished because a prompt, API call, connector, test stub, or dashboard exists. I want the real vertical to survive interruption and still converge to a state that can be independently explained.

---

## Background

I have worked across product and engineering at **Meta, PayPal, and eBay**, including XR/developer platforms, payments, security/risk, data products, and large cross-functional launch and operating programs. My formal background is an **MBA from Wharton** and an **MS in Electrical Engineering from USC**.

The through-line is the same as the systems work above: technically consequential products where architecture, product behavior, operating constraints, and business outcomes cannot be separated cleanly.

---

This account also contains forks, courses, and reference repositories used in research. I do not present those as original authorship.

[LinkedIn](https://www.linkedin.com/in/bsamuel)