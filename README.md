Bruno Samuel
**AI systems · distributed runtimes · developer platforms · XR / spatial computing · payments**  
San Francisco Bay Area · MBA, Wharton · MS Electrical Engineering, USC

I work at the boundary where product architecture becomes operating software: state, execution, data, infrastructure, reliability, controls, developer experience, and the economics of running the system at scale.

```text
Amalgus   national research lab projects and real-time systems covering data streams, realtime interactions, and workflow automations covering multimodel spatial interaction systems
Meta      central platform monetization + release gate standardization + developer/platform programs       100+ B2B customers
PayPal    global safety / security / risk platforms    300M+ users · $500B+ TPV
          engineering + operations                     500+ org
eBay      separation / personalization / operations    large-scale platform change, data center migration, and end-2-end day -1, 0, 100 ops stability and cutover execution
```

Work in my scope included reducing reporting latency from roughly **24 hours to under a minute**, reducing cart abandonment by about **10%**, and scaling PayPal Credit from launch to **$1B+ ARR**. I worked across software, co-design hardware, infrastructure, OpenXR/API surfaces, privacy, legal and NPI/launch dependencies and developer capabilities.

---

## Public system: ONTEXA

[ONTEXA](https://github.com/vbsamuel/ontexa) is a public biomedical discovery prototype built as a heterogeneous local-first system rather than a single-model wrapper.

```text
PubMed / PMC / GEO / SRA            ChEMBL / PubChem
            │                              │
            └────────── ingest / normalize ┘
                           │
                 GO · MONDO · UBERON
                           │
                    Neo4j + PostgreSQL
                           │
          ┌────────────────┼────────────────┐
          │                │                │
     dataset search    ranking/eval     graph discovery
          │                │                │
          └──────────── AI orchestration ───┘
                           │
                    local Ollama models
                           │
                  React operator surface
```

The implementation is directly inspectable:

- [`services/ai_orchestrator/main.py`](https://github.com/vbsamuel/ontexa/blob/main/services/ai_orchestrator/main.py) — FastAPI orchestration, local Ollama model interface, tool registry, Neo4j access, caching, rate-limit retry, ChEMBL/PubChem integration, RDKit hooks and request-latency measurement;
- [`infra/neo4j/schema.cypher`](https://github.com/vbsamuel/ontexa/blob/main/infra/neo4j/schema.cypher) — graph schema/index definition;
- [`scripts/convert-ontology.py`](https://github.com/vbsamuel/ontexa/blob/main/scripts/convert-ontology.py) — ontology preprocessing;
- repository data includes GO, MONDO and UBERON ontology material plus graph/search infrastructure;
- the repo contains separate service boundaries for search, ranking and AI orchestration rather than putting every concern behind one prompt.

The point of the project is visible in the source: **model output is one component in a larger retrieval, graph, ranking, measurement and operator system.**

---

## The systems questions I care about

```text
request accepted          != work completed
message delivered         != effect committed
provider success          != observed external state
retry                     != idempotency
retrieved context         != authoritative state
model confidence          != evidence
projection                != ownership
compile success           != production qualification
```

Those distinctions change architecture.

A state-changing path needs an identity before dispatch. A retry needs to know whether it is repeating computation or duplicating an external effect. A process restart cannot infer completion from the absence of an error. A fast model is not useful if retrieval, memory movement, serialization, synchronization, or readback dominates the critical path. An operator surface should expose the state that matters without becoming another source of truth.

That is the level at which I tend to work: **the whole vertical, including the failure path**.

---

## Engineering range

Recent hands-on work spans **Rust, Go, Python, TypeScript, WebAssembly, Linux/WSL, Windows, local GPU inference, graph/data systems, async services, event-driven execution and multimodal interfaces**.

The language is rarely the hard part. The harder questions are usually:

```text
Who owns this state?
What is the durable identity of this operation?
What happens after the process dies between dispatch and acknowledgement?
How do we know the external effect actually occurred?
What is authoritative when two systems disagree?
What resource is on the critical path?
What evidence would falsify the claim that this is done?
```

---

## Current work

Current product and infrastructure work is proprietary and in flight. I do not publish project names, internal architecture, or unfinished claims here. When something becomes a public artifact, the code and evidence should carry the argument.

This account also contains older experiments, courses, forks and reference repositories. They are part of the working history of the account, not a claim of authorship or a curated product catalog.

[LinkedIn](https://www.linkedin.com/in/bsamuel)
