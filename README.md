# Ideas, Proposals & Paper Clips

**Durable · Decomposable · Dynamic · Distributed Systems**  
*Across software, machines, and silicon.*

I work on systems where execution, state, resources, coordination, and failure behavior have to remain explicit under real constraints.

---

## Current Work

### KV cache tiering under long-context pressure

When reusable KV state no longer fits on-device, when is reload cheaper than recomputation?

```text
device cache only, warm TTFT      9.315 s
device + host tier, warm TTFT     0.336 s    27.7×

cold path                         9.377 s → 13.263 s   regression
forced-eviction 30K context       2.871 s → 0.355 s    8.1×
```

The cold regression is part of the result: tiering only pays when reuse amortizes persistence and transfer cost.

[notebook](https://github.com/vbsamuel/llm-learning-kit/blob/main/notebooks/01_kv-cache-tiering-under-pressure.ipynb) · [measurements](https://github.com/vbsamuel/llm-learning-kit/blob/main/data/kv_cache_recorded_run.json)

### Dynamic batching under concurrency

The first batching configuration made the system slower.

```text
naive                         1,438.9 QPS
max_batch=32, concurrency=16  1,106.6 QPS
best measured max_batch=8     2,923.3 QPS
```

Configured batch capacity above offered concurrency can turn queue delay into pure overhead. The implementation includes the batcher, load generator, synchronization, instrumentation, and Pareto analysis.

[notebook](https://github.com/vbsamuel/llm-learning-kit/blob/main/notebooks/02_dynamic-batching-under-concurrency.ipynb) · [measurements](https://github.com/vbsamuel/llm-learning-kit/blob/main/data/dynamic_batching_recorded_run.json)

### Precision × batch frontier

A joint throughput, memory, and numerical-fidelity experiment measured scaling from **3,271.6 to 44,494.9 samples/s**. The largest tested batch was still improving throughput, so it is not presented as a saturation knee.

[notebook](https://github.com/vbsamuel/llm-learning-kit/blob/main/notebooks/03_precision-batch-frontier.ipynb) · [measurements](https://github.com/vbsamuel/llm-learning-kit/blob/main/data/precision_batch_recorded_run.json)

### Optimization under baseline drift

A candidate appeared **5.77%** faster against a cold baseline, but only **0.93%** faster against the stabilized baseline. With repeated runs, quality evidence, and tail-latency evidence missing, the optimizer returns `INSUFFICIENT_EVIDENCE` rather than promoting the flattering number.

[notebook](https://github.com/vbsamuel/llm-learning-kit/blob/main/notebooks/04_adaptive-optimization-under-baseline-drift.ipynb) · [measurements](https://github.com/vbsamuel/llm-learning-kit/blob/main/data/optimizer_recorded_run.json)

**[Inference systems experiments →](https://github.com/vbsamuel/llm-learning-kit)**

---

## ONTEXA

[ONTEXA](https://github.com/vbsamuel/ontexa) is a local-first biomedical discovery system combining graph retrieval, ontology-aware search, ranking, local model orchestration, and an operator surface.

The model is one component of the system rather than the system boundary:

- [AI orchestration](https://github.com/vbsamuel/ontexa/blob/main/services/ai_orchestrator/main.py) — model interface, tool execution, graph access, caching, retries, external scientific-data access, and latency instrumentation
- [graph schema](https://github.com/vbsamuel/ontexa/blob/main/infra/neo4j/schema.cypher)
- [ontology preprocessing](https://github.com/vbsamuel/ontexa/blob/main/scripts/convert-ontology.py)

---

## Systems

### Durable

State, identity, provenance, recovery, and semantics that survive process, machine, and time boundaries.

### Decomposable

Explicit components, interfaces, ownership, state machines, and independently testable behavior.

### Dynamic

Execution that adapts to changing workload, topology, resources, latency, and operating conditions.

### Distributed

Computation, state, authority, and coordination across processes, machines, accelerators, and people.

---

## Working Areas

| Area | Problems |
|---|---|
| **Execution** | scheduling, placement, batching, streaming, backpressure, retry, checkpointing, recovery |
| **State** | authoritative state, journals, memory, causality, reconciliation, durable transitions |
| **Coordination** | routing, delegation, attribution, synchronization, machine-to-machine and agent-to-agent interaction |
| **Compute** | CPU/GPU execution, heterogeneous resources, locality, resource allocation, runtime control |
| **Representation** | intermediate representations, structured content, spatial systems, progressive rendering, interaction surfaces |
| **Verification** | invariants, qualification, falsification, fault injection, reproducibility, measured evidence |

---

## Working Principles

- State should have an owner.
- Work should have a resource cost.
- Boundaries should be explicit.
- Failures should have defined outcomes.
- Distributed actions should retain causality.
- Measurements should include the unfavorable result.
- Claims should terminate in code, tests, measurements, or reproducible evidence.

---

## Repository Roles

**Systems** — substantial implementations  
**Components** — reusable subsystems  
**Experiments** — bounded technical hypotheses  
**Research** — technical investigations and implementations  
**References** — upstream work retained for study  
**Archive** — historical work not representative of current engineering

---

## Earlier Scale

```text
Meta      XR input + developer/platform programs        100+ B2B customers
PayPal    safety / security / risk platforms             300M+ users · $500B+ TPV
          engineering + operations                       500+ org

eBay      separation / personalization / operations      large-scale platform change
```

Work in my scope included reducing reporting latency from roughly **24 hours to under a minute**, reducing cart abandonment by about **10%**, and scaling PayPal Credit from launch to **$1B+ ARR**.

Most current product and infrastructure work is proprietary and in flight, so I keep it off this page until there is something public that can be inspected on its own merits.

[LinkedIn](https://www.linkedin.com/in/bsamuel)
