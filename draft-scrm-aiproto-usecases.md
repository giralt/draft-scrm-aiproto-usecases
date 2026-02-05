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

Deep Search refers to an *agentic* information‑seeking workflow in which an AI agent plans, executes, and iteratively refines multi‑step research across heterogeneous sources such as open web, enterprise knowledge bases, APIs, files, and computational tools, among others. Unlike one‑shot retrieval or a single RAG call, Deep Search is *long‑horizon* and *goal‑directed*: the agent decomposes a task into sub‑goals, issues searches and crawls, reads and filters evidence, runs auxiliary computations (e.g., code or math), verifies claims, tracks provenance/citations, and synthesizes a final answer---often over minutes or hours rather than milliseconds. This loop is typically implemented as *think -> act (tool) -> observe -> reflect -> refine plan* until success criteria (e.g., coverage, confidence, cost/time budgets) are met.


### Building Blocks

A Deep Search workflow may generally comprise the components shown in the next Figure:

<!-- - **Base LLM (reasoning + tool use).**
  A model capable of multi‑step reasoning (e.g., chain‑of‑thought/verification, self‑reflection) *and* structured tool invocation (function/tool calling) to plan, call tools, parse results, and revise plans.

- **Planner/Orchestrator.**
  A lightweight controller (can be the LLM itself or a graph/agent runtime) that maintains task state, goals, and budgets (tokens, latency, rate limits), and schedules steps (parallel vs. sequential).

- **Tooling layer (invoked by the agent):**
  The tooling layer includes:
  - **Web search & crawling** such SERP APIs, focused crawlers, HTML/PDF parsers, robots.txt compliance.
  - **Knowledge access** such as knowledge bases (KB), document stores, wikis, code repos.
  - **Retrieval‑Augmented Generation (RAG)** such as indexing, re‑ranking, query rewriting, dedup, chunking.
  - **Computation tools** such as a **Python interpreter** for factual checks, data wrangling, statistics/plots.
  - **Specialized services** such as scholarly search, calculators, geocoders, optical character recognition, table extraction, etc.
  - **Verification/critique** such as fact‑checking, citation validation, deduplication, hallucination detection).
  - **Provenance & citation store** such as source URIs, timestamps, quotes/snippets, hashes.

- **Short‑term memory / working set.**
  Scratchpad to hold the evolving evidence graph: normalized documents, extracted entities/claims, metadata, and confidence scores.

- **Synthesis & reporting.**
  Templates or renderers that compile the final artifact (report/brief/bibliography), with explicit citations to the evidence used.

- **Observability & policy.**
  Logging, traces, and red‑team hooks for auditability; safety filters (PII, ToS, copyright/robots policy), rate limiting, attribution. -->


~~~ ascii-art
+--------------------------------------------------------------+
|                        User / Client                         |
|              (Goal, Query, Constraints)                      |
+--------------------------------------------------------------+
                             |
                             v
+--------------------------------------------------------------+
|                 DeepSearch Orchestrator                      |
|                                                              |
|  - planning & task decomposition                             |
|  - agent coordination (A2A)                                  | <----+
|  - iteration control (re-plan, retry, refine)                |      |
|  - shared state & memory                                     |      |
+--------------------------------------------------------------+      |
                             |                                        |
                    tasks / messages (A2A)                            |
                             v                                        |
+--------------------------------------------------------------+      |
|  A2A Agent Communication (standardized agent communication)  |      |
+--------------------------------------------------------------+      |
                             |                                        |
                             v                                        |
+--------------------------------------------------------------+      |
|                         Agents Mesh                          |      |
|                                                              |      |
|  - research / query expansion                                |      |
|  - retrieval & summarization                                 |      |
|  - analysis / computation                                    |      |
|  - validation / fact-checking                                |      |
|                                                              |      |
+--------------------------------------------------------------+      |
                             |                                        |
                      tool calls (MCP)                                |
                             v                                        |
+--------------------------------------------------------------+      |
|       MCP Tooling Layer (standardized tool interfaces)       |      |
+--------------------------------------------------------------+      |
                             |                                        |
                             v                                        |
+-----------------------+   +----------------+   +-----------------+  |
| Web Search & Crawling |   | KB / RAG Index |   |  Python / Tools |  |
|      (SERP APIs)      |-->| (embed/rerank) |-->| (compute, eval) |  |
+-----------------------+   +----------------+   +-----------------+  |
        |                                |                      |     |
        |                                |                      |     |
        +------------- evidence & results returned to agents ---+     |
                             |                                        |
                             v                                        |
+--------------------------------------------------------------+      |
|    DeepSearch Orchestrator: Iterative Improvement Loop       |      |
|                                                              |      |
|   Plan -> Act -> Observe -> Refine -> Re-plan                |------+
|   (query tuning, crawl adjustment, re-ranking, re-eval)      |
+--------------------------------------------------------------+
                             |
                             v
+--------------------------------------------------------------+
|                 Final Answer / Output                        |
|          (synthesis + citations + confidence)                |
+--------------------------------------------------------------+
~~~
{: #fig-deep-search artwork-align="center" title="Deep Search agentic workflow"}

<!-- The loop repeats until success criteria are met (coverage/quality thresholds, budget, or explicit user stop). -->

Each building block in the DeepSearch architecture represents a logical function rather than a specific implementation, and multiple components may be co-located or distributed in practice.

### User / Client

The *User / Client* is the entry point to the system. It provides the initial goal or query, along with optional constraints (e.g., scope, freshness, format). The user does not interact directly with tools or agents; all interactions are mediated by the DeepSearch Orchestrator.

### DeepSearch Orchestrator

The *DeepSearch Orchestrator* acts as the control plane of the system. Its responsibilities include:

- Planning and task decomposition of the user’s request.
- Coordinating agents via Agent-to-Agent (A2A) communication.
- Managing shared state and memory across iterations.
- Controlling iterative execution, including retries and refinements.

The orchestrator does not perform retrieval or computation directly; instead, it delegates work to agents and manages the overall execution flow.

### A2A Agent Communication Bus

The *A2A Agent Communication Bus* provides a standardized messaging layer that enables agent-to-agent coordination. It supports:

- Task dispatch and response exchange.
- Collaboration among specialized agents.
- Decoupling of agent implementations from orchestration logic.

This bus allows agents to operate independently while still contributing to a coherent end-to-end workflow.

### Agents Mesh

The *Agents Mesh* block represents a set of specialized, cooperative agents operating over the A2A bus. Typical agent roles include:

- Research and query expansion.
- Retrieval and summarization.
- Analysis and computation.
- Validation and fact-checking.

Agents are responsible for invoking tools, interpreting results, and returning structured observations to the orchestrator.

### MCP Tooling Layer

The *MCP Tooling Layer* provides a standardized interface between agents and external tools. It enables:

- Discovery and invocation of tools using a common protocol.
- Consistent input/output schemas across heterogeneous tools.
- Isolation of agent logic from tool-specific details.

MCP acts as an abstraction boundary that simplifies integration and evolution of external capabilities.

### Web Search & Crawling

The *Web Search & Crawling* component combines content discovery and acquisition. It typically includes:

- Search engine or SERP APIs for identifying relevant sources.
- Focused crawling or fetching to retrieve selected content.

This component supplies raw external data that can be further processed and indexed.

### Knowledge Base (KB) / Retrieval Augmented Generation (RAG) Index

The *KB / RAG Index* component manages knowledge representation and retrieval. Its responsibilities include:

- Embedding and indexing retrieved content.
- Ranking or re-ranking results based on relevance.
- Supplying context to agents for retrieval-augmented generation (RAG).

This block provides structured, queryable knowledge derived from external sources.

### Python / Tools

The *Python / Tools* component represents general-purpose computation and evaluation capabilities. Examples include:

- Data processing and transformation.
- Numerical analysis or simulations.
- Quality evaluation, scoring, or consistency checks.

These tools are typically invoked by analysis-oriented agents via the MCP layer.

### Iterative Improvement Loop

The *Iterative Improvement Loop* captures the system’s ability to refine results over multiple passes and is also implemeted by the DeepSearch Orchestrator. Conceptually, it follows a cycle of:

    Plan -> Act -> Observe -> Refine -> Re-plan

Observations and intermediate results are fed back into the orchestrator, which may adjust plans, agent assignments, or tool usage before producing the final output.

### Final Answer / Output

The *Final Answer / Output* is the synthesized result returned to the user. It may include:

- A consolidated response or report.
- References or citations to supporting evidence.
- Confidence indicators or stated limitations.

This output reflects the outcome of one or more iterative refinement cycles.

### Why this use case matters in the context of protocol standards

Deep Search is inherently *compositional*: it coordinates *multiple* agents and *many* tools over extended time. Without standard protocols, systems devolve into brittle, one‑off integrations that are hard to test, secure, or reuse. Two complementary interoperability layers in the DeepSearch are especially relevant:

- **Agent‑to‑Tool standardization.**
  The *Model Context Protocol (MCP)* defines a common way for agents/hosts to discover, describe, and invoke tools, resources, and prompts over JSON‑RPC across transports (stdio, HTTP/SSE, WebSocket). MCP enables portable tool catalogs (search, crawler, RAG, Python) with consistent schemas, capability negotiation, progress/cancellation, and security prompts/consent. [Model Context Protocol specification](https://modelcontextprotocol.io/specification/2025-03-26) • [MCP GitHub org](https://github.com/modelcontextprotocol).

- **A2A Agent Communication Bus.**
  The *Agent2Agent (A2A)* protocol focuses on inter‑agent collaboration—capability discovery (Agent Cards), task lifecycle (create/cancel/status), streaming updates for long‑running jobs, and opaque collaboration without sharing proprietary internals. See the overview/spec and announcement: [A2A protocol site/spec](https://a2a-protocol.org/latest/) • [Google Developers announcement](https://developers.googleblog.com/en/a2a-a-new-era-of-agent-interoperability/) • [A2A GitHub](https://github.com/a2aproject/A2A).

**Implications for Deep Search.** Using A2A and MCP together lets implementers compose portable Deep Search stacks:

- Tools like crawlers, scholarly search, RAG, and Python are exposed via **MCP** with typed inputs/outputs and consent flows.
- Long‑running research tasks, delegation to specialized researcher/verifier agents, background execution, progress streaming, and result handoff occur via **A2A**.
- Provenance (URIs, hashes, timestamps) and citation schemas can also be standardized at the protocol boundary to enable verifiable research traces across vendors.
- Enterprise requirements (authn/z), quotas, observability/tracing, policy enforcement (robots/copyright), and safety reviews—become portable rather than per‑integration glue.

### Example: Open Deep Search project

Open implementations illustrate agentic architectures for Deep Search:

- **Open Deep Search (ODS).** A modular open‑source framework that augments a base LLM with a *Reasoning Agent* and an *Open Search Tool*, reporting state‑of‑the‑art results on benchmarks like SimpleQA and FRAMES. [Alzubi et al., 2025 (arXiv)](https://arxiv.org/abs/2503.20201) • [sentient‑agi/OpenDeepSearch (GitHub)](https://github.com/sentient-agi/OpenDeepSearch).

- **Open Deep Research (LangChain).** An agentic “deep research” reference built on LangGraph that works across model providers, search tools, and *MCP servers*; includes supervisor/sub‑agent patterns and evaluation harnesses. [Project blog](https://www.blog.langchain.com/open-deep-research/) • [langchain‑ai/open_deep_research (GitHub)](https://github.com/langchain-ai/open_deep_research).

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
