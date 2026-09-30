# Ideas, Proposals & Paper Clips

**Durable · Decomposable · Dynamic · Distributed Systems**  
*Across software, machines, and silicon.*

I work on systems where execution, state, resources, coordination, and failure behavior remain explicit under real constraints.

---

## Selected Work

### Inference systems experiments

Controlled experiments in inference serving and accelerator behavior. Each experiment keeps the failure case, the recorded measurements, and the boundary of what the data can support.

| Experiment | Recorded result | What changed |
|---|---:|---|
| **KV cache tiering** | warm TTFT **9.315 s → 0.336 s** | reload wins only after reuse amortizes persistence and transfer; cold path regressed |
| **Dynamic batching** | **1,438.9 → 2,923.3 QPS** at the best measured configuration | oversized configured batches added queueing overhead when offered concurrency could not fill them |
| **Precision × batch** | **3,271.6 → 44,494.9 samples/s** across the measured sweep | throughput was still rising at the largest tested batch; no saturation knee claimed |
| **Baseline drift** | apparent **5.77%** gain became **0.93%** against the stabilized baseline | candidate rejected as `INSUFFICIENT_EVIDENCE` |

[repository](https://github.com/vbsamuel/llm-learning-kit) · [evidence manifest](https://github.com/vbsamuel/llm-learning-kit/blob/main/EVIDENCE_MANIFEST.json)

### ONTEXA

A local-first biomedical discovery system built as a set of explicit services rather than a model wrapper: dataset search, graph retrieval, ontology processing, ranking, local model orchestration, metrics, and an operator surface.

```text
scientific sources
      ↓
ingest / ontology
      ↓
graph + search
      ↓
ranking
      ↓
local model orchestration
      ↓
operator surface
```

The implementation spans Go dataset search, Rust ranking, Python orchestration, Neo4j graph state, PostgreSQL, and a React operator surface.

[repository](https://github.com/vbsamuel/ontexa) · [orchestration](https://github.com/vbsamuel/ontexa/blob/main/services/ai_orchestrator/main.py) · [graph schema](https://github.com/vbsamuel/ontexa/blob/main/infra/neo4j/schema.cypher) · [ontology preprocessing](https://github.com/vbsamuel/ontexa/blob/main/scripts/convert-ontology.py)

> Public work appears here only when the implementation or evidence can be inspected directly. Forks, references, coursework, and unfinished prototypes are not part of this index.

---

## System Properties

**Durable** — state, identity, provenance, recovery, and semantics that survive process, machine, and time boundaries.  
**Decomposable** — explicit components, interfaces, ownership, state machines, and independently testable behavior.  
**Dynamic** — execution that responds to changing workload, topology, resources, latency, and operating conditions.  
**Distributed** — computation, state, authority, and coordination across processes, machines, accelerators, and people.

---

## Working Areas

| Area | Current questions |
|---|---|
| **Execution** | scheduling, placement, batching, streaming, backpressure, retry, checkpointing, recovery |
| **State** | ownership, journals, memory, causality, reconciliation, durable transitions |
| **Coordination** | routing, delegation, attribution, synchronization, machine-to-machine and agent-to-agent interaction |
| **Compute** | CPU/GPU execution, heterogeneous resources, locality, allocation, runtime control |
| **Representation** | intermediate representations, structured content, spatial systems, progressive rendering, interaction surfaces |
| **Verification** | invariants, qualification, falsification, fault injection, reproducibility, measured evidence |

---

## Engineering Rules

```text
state            → one explicit authority
work             → bounded by real resources
mutation         → declared path
failure          → defined outcome
retry            → idempotent effect
coordination     → retained causality
measurement      → workload + environment + counter-result
claim            → code, test, measurement, or reproducible evidence
```

---

## Repository Index

**Systems** — substantial original implementations  
**Components** — reusable subsystems  
**Experiments** — bounded technical hypotheses  
**Research** — technical investigations and implementations  
**References / Forks** — upstream work retained for study  
**Archive** — historical work not representative of current engineering

The public repository count is not the portfolio. The index above is.

---

## Earlier Scale

```text
Meta      XR input + developer/platform programs        100+ B2B customers
PayPal    safety / security / risk platforms             300M+ users · $500B+ TPV
          engineering + operations                       500+ org

eBay      separation / personalization / operations      large-scale platform change
```

Work in my scope included reducing reporting latency from roughly **24 hours to under a minute**, reducing cart abandonment by about **10%**, and scaling PayPal Credit from launch to **$1B+ ARR**.

[LinkedIn](https://www.linkedin.com/in/bsamuel)
