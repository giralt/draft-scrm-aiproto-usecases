---

title: "Agentic AI Use Cases"
abbrev: "Agentic AI Use Cases"
category: info

docname: draft-scrm-aiproto-usecases-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: AREA
workgroup: WG Working Group
keyword:
 - AI Agents
 - Use cases
venue:
  # group: WG
  # type: Working Group
  # mail: WG@example.com
  # arch: https://example.com/WG
  github: https://github.com/giralt/draft-scrm-aiproto-usecases
  # latest: https://example.com/LATEST

author:
 -
    fullname: "Roland Schott"
    organization: Deutsche Telekom
    email: "Roland.Schott@telekom.de"

 -
    fullname: "Julien Maisonneuve"
    organization: Nokia Bell Labs
    email: "julien.maisonneuve@nokia.com"

 -
    fullname: "L. M. Contreras"
    organization: Telefonica
    email: "luismiguel.contrerasmurillo@telefonica.com"

 -
    fullname: "Jordi Ros-Giralt"
    organization: Qualcomm Europe, Inc.
    email: "jros@qti.qualcomm.com"


normative:

informative:

...

--- abstract

TODO Abstract


--- middle

# Introduction

TODO Introduction


# Conventions and Definitions

{::boilerplate bcp14-tagged}

# Use Cases


## Deep Search

Deep Search refers to an *agentic* information‑seeking workflow in which an AI agent plans, executes, and iteratively refines multi‑step research across heterogeneous sources such as open web, enterprise knowledge bases, APIs, files, and computational tools, among others. Unlike one‑shot retrieval or a single RAG call, Deep Search is *long‑horizon* and *goal‑directed*: the agent decomposes a task into sub‑goals, issues searches and crawls, reads and filters evidence, runs auxiliary computations (e.g., code or math), verifies claims, tracks provenance/citations, and synthesizes a final answer—often over minutes or hours rather than milliseconds. This loop is typically implemented as *think → act (tool) → observe → reflect → refine plan* until success criteria (e.g., coverage, confidence, cost/time budgets) are met.


### Building Blocks

A conformant Deep Search workflow generally comprises the following components:

- **Base LLM (reasoning + tool use).**
  A model capable of multi‑step reasoning (e.g., chain‑of‑thought/verification, self‑reflection) *and* structured tool invocation (function/tool calling) to plan, call tools, parse results, and revise plans.

- **Planner/Orchestrator.**
  A lightweight controller (can be the LLM itself or a graph/agent runtime) that maintains task state, goals, and budgets (tokens, latency, rate limits), and schedules steps (parallel vs. sequential).

- **Tooling layer (invoked by the agent):**
  - **Web search & crawling** (SERP APIs, focused crawlers, HTML/PDF parsers, robots.txt compliance).
  - **Knowledge access** (enterprise KBs, document stores, wikis, code repos).
  - **Retrieval‑Augmented Generation (RAG)** (indexing, re‑ranking, query rewriting, dedup, chunking).
  - **Computation tools** (e.g., **Python interpreter** for factual checks, data wrangling, statistics/plots).
  - **Specialized services** (scholarly search, calculators, geocoders, OCR, table extraction, etc.).
  - **Verification/critique** (fact‑checking, citation validation, deduplication, hallucination detection).
  - **Provenance & citation store** (source URIs, timestamps, quotes/snippets, hashes).

- **Short‑term memory / working set.**
  Scratchpad to hold the evolving evidence graph: normalized documents, extracted entities/claims, metadata, and confidence scores.

- **Synthesis & reporting.**
  Templates or renderers that compile the final artifact (report/brief/bibliography), with explicit citations to the evidence used.

- **Observability & policy.**
  Logging, traces, and red‑team hooks for auditability; safety filters (PII, ToS, copyright/robots policy), rate limiting, attribution.


~~~ ascii-art
                          +--------------------------------------+
                          |              Planning Loop           |
                          |  (reason over goals, budgets, time)  |
                          +--------------------------------------+
                                          |
                                   (1) User Goal / Query
                                          |
                                          v
+------------------+     plan / subgoals     +------------------------+
|  Reasoning LLM   |<----------------------->|   Orchestrator /       |
| (tool-capable)   |  reflect / revise plan  |   Task Controller      |
+------------------+                         +------------------------+
        |                                               |
        | (2) tool calls                                | (3) schedule/monitor
        v                                               v
+------------------+    +------------------+    +------------------+    +------------------+
| Web Search /     |    |  Crawlers        |    |  KB / RAG Index  |    |  Python / Tools |
| SERP APIs        |--->|  (focused fetch) |--->|  (embed/rerank)  |--->|  (compute, eval) |
+------------------+    +------------------+    +------------------+    +------------------+
        \___________________________  |  _______________________________________________/
                                      v
                          +-----------------------------+
                          |   Evidence / Working Set    |
                          | (docs, snippets, claims,    |
                          |  citations, confidence)     |
                          +-----------------------------+
                                          |
                                 (4) verify / critique
                                          v
                               +---------------------+
                               |  Verifier/Critic    |
                               | (consistency,       |
                               |  provenance checks) |
                               +---------------------+
                                          |
                                   (5) synthesize
                                          v
                          +--------------------------------------+
                          |  Report / Answer with Citations      |
                          |  (export, stream, or hand off)       |
                          +--------------------------------------+
~~~
{: #fig-deep-search artwork-align="center" title="Deep Search agentic workflow"}

The loop repeats until success criteria are met (coverage/quality thresholds, budget, or explicit user stop).

### Why this use case matters in the context of protocol standards

Deep Search is inherently **compositional**: it coordinates *multiple* agents and *many* tools over extended time. Without standard protocols, systems devolve into brittle, one‑off integrations that are hard to test, secure, or reuse. Two complementary interoperability layers are especially relevant:

- **Agent‑to‑Tool standardization (inside an agent).**
  The **Model Context Protocol (MCP)** defines a common way for agents/hosts to discover, describe, and invoke tools, resources, and prompts over JSON‑RPC across transports (stdio, HTTP/SSE, WebSocket). MCP enables portable tool catalogs (search, crawler, RAG, Python) with consistent schemas, capability negotiation, progress/cancellation, and security prompts/consent. [Model Context Protocol specification](https://modelcontextprotocol.io/specification/2025-03-26) • [MCP GitHub org](https://github.com/modelcontextprotocol).

- **Agent‑to‑Agent standardization (between agents/systems).**
  The **Agent2Agent (A2A)** protocol focuses on inter‑agent collaboration—capability discovery (Agent Cards), task lifecycle (create/cancel/status), streaming updates for long‑running jobs, and opaque collaboration without sharing proprietary internals. A2A complements MCP (A2A connects *agents*; MCP connects an agent to its *tools*). See the overview/spec and announcement: [A2A protocol site/spec](https://a2a-protocol.org/latest/) • [Google Developers announcement](https://developers.googleblog.com/en/a2a-a-new-era-of-agent-interoperability/) • [A2A GitHub](https://github.com/a2aproject/A2A). The IETF has also created a non‑WG **agent2agent** list to scope standardization work in this space ([IETF mail archive](https://mailarchive.ietf.org/arch/msg/agent2agent/a6ORMj_pMlYZI_KU3gPMhB-K9Ng/)).

**Implications for Deep Search.** Using A2A and MCP together lets implementers compose portable Deep Search stacks:

- Tools like crawlers, scholarly search, RAG, and Python are exposed via **MCP** with typed inputs/outputs and consent flows.
- Long‑running research tasks, delegation to specialized researcher/verifier agents, background execution, progress streaming, and result handoff occur via **A2A**.
- Provenance (URIs, hashes, timestamps) and citation schemas can be standardized at the protocol boundary to enable verifiable research traces across vendors.
- Enterprise requirements—authn/z, quotas, observability/tracing, policy enforcement (robots/copyright), and safety reviews—become portable rather than per‑integration glue.

### Example: Open Deep Search project

Open implementations illustrate agentic architectures for Deep Search:

- **Open Deep Search (ODS).** A modular open‑source framework that augments a base LLM with a **Reasoning Agent** and an **Open Search Tool**, reporting state‑of‑the‑art results on benchmarks like SimpleQA and FRAMES; paper and code are available. [Alzubi et al., 2025 (arXiv)](https://arxiv.org/abs/2503.20201) • [sentient‑agi/OpenDeepSearch (GitHub)](https://github.com/sentient-agi/OpenDeepSearch).

- **Open Deep Research (LangChain).** An agentic “deep research” reference built on LangGraph that works across model providers, search tools, and **MCP servers**; includes supervisor/sub‑agent patterns and evaluation harnesses. [Project blog](https://www.blog.langchain.com/open-deep-research/) • [langchain‑ai/open_deep_research (GitHub)](https://github.com/langchain-ai/open_deep_research).

These systems exemplify the building blocks described earlier and are consistent with the interoperability layering (MCP for tools; A2A for inter‑agent collaboration).

### Informative References

- Model Context Protocol (spec & docs): [modelcontextprotocol.io](https://modelcontextprotocol.io/specification/2025-03-26), [GitHub organization](https://github.com/modelcontextprotocol).
- Agent2Agent Protocol (A2A): [a2a‑protocol.org](https://a2a-protocol.org/latest/), [spec](https://a2a-protocol.org/latest/specification/), [Google Developers announcement](https://developers.googleblog.com/en/a2a-a-new-era-of-agent-interoperability/), [Linux Foundation news](https://developers.googleblog.com/en/google-cloud-donates-a2a-to-linux-foundation/).
- IETF “agent2agent” list (standardization discussion): [mailarchive.ietf.org](https://mailarchive.ietf.org/arch/msg/agent2agent/a6ORMj_pMlYZI_KU3gPMhB-K9Ng/).
- Open Deep Search (ODS): [arXiv:2503.20201](https://arxiv.org/abs/2503.20201), [GitHub repository](https://github.com/sentient-agi/OpenDeepSearch).
- LangChain Open Deep Research: [project blog](https://www.blog.langchain.com/open-deep-research/), [GitHub](https://github.com/langchain-ai/open_deep_research).
``



# Security Considerations

TODO Security


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
