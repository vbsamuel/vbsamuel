# Bruno Samuel

I build systems where AI has to operate under real constraints: **latency, memory, concurrency, state, recovery, and evidence**.

Amalgus . Meta · PayPal · eBay  
MBA, Wharton · MS Electrical Engineering, USC · San Francisco Bay Area

## Recent lab work

### KV cache tiering under long-context pressure

A controlled cache-pressure experiment asked a simple question: when reusable KV state no longer fits on-device, when is reload cheaper than recomputation?

```text
device cache only, warm TTFT      9.315 s
device + host tier, warm TTFT     0.336 s    27.7×

cold path                         9.377 s → 13.263 s   regression
forced-eviction 30K context       2.871 s → 0.355 s    8.1×
```

The cold regression is part of the result, not something I hide: tiering only pays when reuse amortizes persistence and transfer cost.

[notebook](https://github.com/vbsamuel/llm-learning-kit/blob/main/notebooks/01_kv-cache-tiering-under-pressure.ipynb) · [measurements](https://github.com/vbsamuel/llm-learning-kit/blob/main/data/kv_cache_recorded_run.json)

### Dynamic batching under concurrency

The first batching configuration made the system slower.

```text
naive                         1,438.9 QPS
max_batch=32, concurrency=16  1,106.6 QPS   ← worse
best measured max_batch=8     2,923.3 QPS
```

The useful result was not “batching is faster.” Configured batch capacity above offered concurrency can turn queue delay into pure overhead. The notebook implements the batcher, load generator, synchronization, instrumentation, and Pareto analysis directly.

[notebook](https://github.com/vbsamuel/llm-learning-kit/blob/main/notebooks/02_dynamic-batching-under-concurrency.ipynb) · [measurements](https://github.com/vbsamuel/llm-learning-kit/blob/main/data/dynamic_batching_recorded_run.json)

### Precision × batch frontier

A joint throughput / memory / numerical-fidelity experiment measured batch scaling from **3,271.6 to 44,494.9 samples/s**. The largest tested batch was still improving throughput, so I do **not** call it the saturation knee.

The notebook also keeps precision selection separate from batching gain and compares candidate arithmetic using the same weights and inputs.

[notebook](https://github.com/vbsamuel/llm-learning-kit/blob/main/notebooks/03_precision-batch-frontier.ipynb) · [measurements](https://github.com/vbsamuel/llm-learning-kit/blob/main/data/precision_batch_recorded_run.json)

### Adaptive optimization under baseline drift

A candidate appeared **5.77%** faster against a cold baseline, but only **0.93%** faster against the stabilized baseline.

Rather than promote the flattering number, the optimizer returns `INSUFFICIENT_EVIDENCE`: repeated runs, quality evidence, and tail-latency evidence are missing.

[notebook](https://github.com/vbsamuel/llm-learning-kit/blob/main/notebooks/04_adaptive-optimization-under-baseline-drift.ipynb) · [measurements](https://github.com/vbsamuel/llm-learning-kit/blob/main/data/optimizer_recorded_run.json)

**[AI Systems Lab →](https://github.com/vbsamuel/llm-learning-kit)**

---

## Public build: ONTEXA

[ONTEXA](https://github.com/vbsamuel/ontexa) is a local-first biomedical discovery system that combines graph retrieval, ontology-aware search, ranking, local model orchestration, and an operator surface.

The implementation is inspectable rather than described abstractly:

- [AI orchestration](https://github.com/vbsamuel/ontexa/blob/main/services/ai_orchestrator/main.py) — local model interface, tool execution, graph access, caching, retries, external scientific-data access, and latency instrumentation;
- [graph schema](https://github.com/vbsamuel/ontexa/blob/main/infra/neo4j/schema.cypher);
- [ontology preprocessing](https://github.com/vbsamuel/ontexa/blob/main/scripts/convert-ontology.py).

The design treats model output as one component in a larger retrieval, graph, ranking, measurement, and operator system.

---

## Earlier scale

```text
Meta      XR input + developer/platform programs        100+ B2B customers
PayPal    safety / security / risk platforms             300M+ users · $500B+ TPV
          engineering + operations                       500+ org

eBay      separation / personalization / operations      large-scale platform change
```

Work in my scope included reducing reporting latency from roughly **24 hours to under a minute**, reducing cart abandonment by about **10%**, and scaling PayPal Credit from launch to **$1B+ ARR**.

Most current product and infrastructure work is proprietary and in flight, so I keep it off this page until there is something public that can be inspected on its own merits.

[LinkedIn](https://www.linkedin.com/in/bsamuel)
