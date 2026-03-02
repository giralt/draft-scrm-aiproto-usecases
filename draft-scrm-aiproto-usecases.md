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
    org: Deutsche Telekom
    email: "Roland.Schott@telekom.de"

 -
    fullname: "Julien Maisonneuve"
    org: Nokia
    email: "julien.maisonneuve@nokia.com"

 -
    fullname: "L. M. Contreras"
    org: Telefonica
    email: "luismiguel.contrerasmurillo@telefonica.com"

 -
    fullname: "Jordi Ros-Giralt"
    org: Qualcomm Europe, Inc.
    email: "jros@qti.qualcomm.com"


normative:

informative:

## Informative References

  MCP:
    title:  "Model Context Protocol (MCP) Specification"
    date: 26 March 2025
    target: https://modelcontextprotocol.io/specification/2025-03-26

  MCP-GITHUB:
    title:  "Model Context Protocol – GitHub Organization"
    target: https://github.com/modelcontextprotocol

  A2A:
    title:  "Agent2Agent (A2A) Protocol Specification"
    target: https://a2a-protocol.org/latest/

  A2A-GITHUB:
    title:  "Agent2Agent Protocol – GitHub Repository"
    target: https://github.com/a2aproject/A2A

  ODS:
    title:  "Open Deep Search"
    date: 2025
    target: https://arxiv.org/abs/2503.20201

  ODS-GITHUB:
    title:  "OpenDeepSearch"
    target: https://github.com/sentient-agi/OpenDeepSearch

--- abstract

Agentic AI systems rely on large language models to plan and execute multi-step tasks by interacting with tools and collaborating with other agents, creating new demands on Internet protocols for interoperability, scalability, and safe operation across administrative domains. This document inventories representative Agentic AI use cases and captures the protocol-relevant requirements they imply, with the goal of helping the IETF determine appropriate standardization scope and perform gap analysis against emerging proposals. The use cases are written to expose concrete needs such as long-lived and multi-modal interactions, delegation and coordination patterns, and security/privacy hooks that have protocol implications. Through use case analysis, the document also aims to help readers understand how agent-to-agent and agent-to-tool protocols (e.g., {{A2A}} and {{MCP}}), and potential IETF-standardized evolutions thereof, could be layered over existing IETF protocol substrates and how the resulting work could be mapped to appropriate IETF working groups.

--- middle

# Introduction

Agentic AI systems—software agents that use large language models to reason, plan, and take actions by interacting with tools and with other agents—are seeing rapid adoption across multiple domains. The ecosystem is also evolving quickly through open-source implementations and emerging protocol proposals; however, open source alone does not guarantee interoperability, since rapid iteration and fragmentation can make stable interoperation difficult when long-term compatibility is required. Several protocols have been proposed to support agentic systems (e.g., {{A2A}}, {{MCP}}, ANP, Agntcy), each with different design choices and strengths, targeting different functions, properties, and operating assumptions.

This document inventories a set of representative Agentic AI use cases to help the IETF derive protocol requirements and perform gap analysis across existing proposals, with a focus on Internet-scale interoperability. The use cases are intended to highlight protocol properties that matter in practice—such as long-lived interactions, multi-modal context exchange, progress reporting and cancellation, and safety-relevant security and privacy hooks—and to help the IETF determine appropriate scope as well as how related work should be organized across existing working groups or, if needed, a new effort.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

# Use Cases Requirements

The use cases in this document are intended to inform IETF standardization work on Agentic AI protocols by clarifying scope, enabling gap analysis, and guiding working group ownership. The requirements below define the minimum level of detail and structure expected from each use case so that the IETF can derive actionable protocol requirements and identify where coordination with other SDOs is necessary. Use cases that do not meet these requirements risk being insufficiently precise for protocol design and evaluation.

- **IETF scope guidance**: Use cases MUST clearly indicate which protocol behaviors are expected to fall under the IETF’s domain (e.g., Internet-facing interoperability, transport/session semantics, media/session behavior, congestion and reliability considerations, security and privacy hooks) versus what is out of scope for the IETF (e.g., model internals, proprietary orchestration logic). Use cases SHOULD also identify where coordination with other SDOs or industry initiatives is required to achieve interoperable and scalable outcomes.

- **Ecosystem boundary mapping**: Use cases SHOULD describe the relevant protocol ecosystem and interfaces between components (e.g., agent-to-agent vs. agent-to-tool) so the IETF can understand what can be standardized as Internet protocols and what is better treated as application/framework conventions. Where applicable, use cases SHOULD illustrate complementary roles of protocols such as agent-to-agent interaction (e.g., {{A2A}}) and agent-to-tool interaction (e.g., {{MCP}}).

- **Gap analysis readiness**: Use cases MUST be structured so that an engineer can map them to existing proposals and then identify missing, underspecified, or insufficiently mature protocol capabilities that block deployment. Use cases SHOULD include enough detail to reveal gaps, and MUST distinguish between gaps that plausibly belong in IETF standardization versus gaps that are purely implementation choices.

- **Adoption and layering**: Use cases SHOULD explain how non-IETF protocols that may be brought into the IETF (e.g., an A2A-like protocol) could be layered on top of, and interoperate cleanly with, existing IETF protocols (e.g., HTTP, QUIC, WebRTC, TLS). Use cases MUST identify assumed transport/bindings and the key interoperation points (e.g., discovery, session establishment, streaming, error handling) needed to assess architectural fit and integration impact.

- **Communication mode detail**: Use cases MUST describe the communication modes required between agents and between agents and tools reachable over the Internet, such as interactive request/response, asynchronous workflows, bulk transfer, incremental streaming, and notification patterns. Use cases SHOULD also indicate modality needs (text, audio/video, files, structured artifacts) when relevant.

- **Performance and safety needs**: Use cases SHOULD include explicit performance requirements when meaningful (e.g., latency sensitivity, bandwidth intensity, jitter tolerance, session duration, scalability expectations). Use cases MUST also call out safety-relevant requirements that have protocol implications (e.g., authorization and consent gates, provenance/citation needs, integrity and replay protection, isolation boundaries for tool invocation).

- **WG ownership signals**: Use cases SHOULD be decomposable into protocol functions that can be mapped to existing IETF working groups (e.g., transport, security, applications, operations/management, identity). Use cases MUST highlight cross-area dependencies (e.g., session + media + security) so the IETF can assess whether coordination across existing WGs is sufficient or whether forming a new WG is justified.

- **Operational realism**: Use cases SHOULD reflect real deployment constraints on the Internet. This requirement helps ensure the resulting protocol requirements are implementable and deployable at scale, rather than being tied to a single controlled environment.

- **Trust boundaries explicit**: Use cases MUST identify administrative domains and trust boundaries (e.g., user device, enterprise perimeter, third-party tool providers, external agent providers) and SHOULD summarize the expected security posture at those boundaries (authentication, authorization, confidentiality, and auditability expectations). This helps ensure the IETF does not miss protocol hooks needed to safely operate agentic systems across domains.

# Use Cases

This section inventories representative Agentic AI use cases to make their protocol-relevant requirements explicit and comparable. The use cases are written to expose concrete needs such as multi-step delegation, agent-to-agent coordination, agent-to-tool interactions, and long-lived and multi-modal exchanges that must operate safely and reliably across administrative domains.  By grounding the discussion in specific scenarios, the document supports gap analysis against emerging agent protocols (e.g., agent-to-agent and agent-to-tool approaches such as A2A and MCP) and clarifies how candidate solutions could be layered over existing IETF protocol substrates and mapped to appropriate IETF working groups, including the necessary security and privacy hooks.

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

#### User / Client

The *User / Client* is the entry point to the system. It provides the initial goal or query, along with optional constraints (e.g., scope, freshness, format). The user does not interact directly with tools or agents; all interactions are mediated by the DeepSearch Orchestrator.

#### DeepSearch Orchestrator

The *DeepSearch Orchestrator* acts as the control plane of the system. Its responsibilities include:

- Planning and task decomposition of the user’s request.
- Coordinating agents via Agent-to-Agent (A2A) communication.
- Managing shared state and memory across iterations.
- Controlling iterative execution, including retries and refinements.

The orchestrator does not perform retrieval or computation directly; instead, it delegates work to agents and manages the overall execution flow.

#### A2A Agent Communication Bus

The *A2A Agent Communication Bus* provides a standardized messaging layer that enables agent-to-agent coordination. It supports:

- Task dispatch and response exchange.
- Collaboration among specialized agents.
- Decoupling of agent implementations from orchestration logic.

This bus allows agents to operate independently while still contributing to a coherent end-to-end workflow.

#### Agents Mesh

The *Agents Mesh* block represents a set of specialized, cooperative agents operating over the A2A bus. Typical agent roles include:

- Research and query expansion.
- Retrieval and summarization.
- Analysis and computation.
- Validation and fact-checking.

Agents are responsible for invoking tools, interpreting results, and returning structured observations to the orchestrator.

#### MCP Tooling Layer

The *MCP Tooling Layer* provides a standardized interface between agents and external tools. It enables:

- Discovery and invocation of tools using a common protocol.
- Consistent input/output schemas across heterogeneous tools.
- Isolation of agent logic from tool-specific details.

MCP acts as an abstraction boundary that simplifies integration and evolution of external capabilities.

#### Web Search & Crawling

The *Web Search & Crawling* component combines content discovery and acquisition. It typically includes:

- Search engine or SERP APIs for identifying relevant sources.
- Focused crawling or fetching to retrieve selected content.

This component supplies raw external data that can be further processed and indexed.

#### Knowledge Base (KB) / Retrieval Augmented Generation (RAG) Index

The *KB / RAG Index* component manages knowledge representation and retrieval. Its responsibilities include:

- Embedding and indexing retrieved content.
- Ranking or re-ranking results based on relevance.
- Supplying context to agents for retrieval-augmented generation (RAG).

This block provides structured, queryable knowledge derived from external sources.

#### Python / Tools

The *Python / Tools* component represents general-purpose computation and evaluation capabilities. Examples include:

- Data processing and transformation.
- Numerical analysis or simulations.
- Quality evaluation, scoring, or consistency checks.

These tools are typically invoked by analysis-oriented agents via the MCP layer.

#### Iterative Improvement Loop

The *Iterative Improvement Loop* captures the system’s ability to refine results over multiple passes and is also implemeted by the DeepSearch Orchestrator. Conceptually, it follows a cycle of:

    Plan -> Act -> Observe -> Refine -> Re-plan

Observations and intermediate results are fed back into the orchestrator, which may adjust plans, agent assignments, or tool usage before producing the final output.

#### Final Answer / Output

The *Final Answer / Output* is the synthesized result returned to the user. It may include:

- A consolidated response or report.
- References or citations to supporting evidence.
- Confidence indicators or stated limitations.

This output reflects the outcome of one or more iterative refinement cycles.

### Why This Use Case Matters

Deep Search is inherently *compositional*: it coordinates *multiple* agents and *many* tools over extended time. Without standard protocols, systems devolve into brittle, one‑off integrations that are hard to test, secure, or reuse. Two complementary interoperability layers in the DeepSearch are especially relevant:

- **Agent‑to‑Tool standardization.**
  The *Model Context Protocol (MCP)* defines a standardized mechanism by which agents and hosts can discover, describe, and invoke tools, resources, and prompts using JSON-RPC over multiple transports (e.g., stdio, HTTP with Server-Sent Events, and WebSocket). MCP enables portable and reusable tool catalogs (including search, crawling, retrieval-augmented generation (RAG), and general-purpose computation) with consistent schemas, capability negotiation, progress reporting, cancellation semantics, and explicit security prompts and user consent. Further details are specified in the MCP specification and related project documentation {{MCP}}{{MCP-GITHUB}}.

- **A2A Agent Communication Bus.**
  The *Agent2Agent (A2A)* protocol focuses on standardized inter-agent collaboration. It defines mechanisms for agent capability discovery (e.g., Agent Cards), task lifecycle management (creation, cancellation, and status reporting), and streaming updates for long-running operations. A2A is designed to support opaque collaboration among agents while avoiding the need to disclose proprietary internal implementations. An overview of the protocol, along with its specification and design rationale, is available from the A2A project documentation {{A2A}}{{A2A-GITHUB}}.

**Implications for Deep Search.** Using A2A and MCP together lets implementers compose portable Deep Search stacks:

- Tools like crawlers, scholarly search, RAG, and Python are exposed via **MCP** with typed inputs/outputs and consent flows.
- Long‑running research tasks, delegation to specialized researcher/verifier agents, background execution, progress streaming, and result handoff occur via **A2A**.
- Provenance (URIs, hashes, timestamps) and citation schemas can also be standardized at the protocol boundary to enable verifiable research traces across vendors.
- Enterprise requirements (authn/z), quotas, observability/tracing, policy enforcement (robots/copyright), and safety reviews—become portable rather than per‑integration glue.


### Example: Open Deep Search (ODS)

Open implementations illustrate agentic architectures for Deep Search.

**Open Deep Search (ODS)** is a modular, open-source framework that augments a base large language model with a dedicated Reasoning Agent and an Open Search tool. The framework is designed to support extensible, agentic search workflows in which an agent iteratively plans, invokes search tools, and synthesizes results to answer complex queries. Further details are available in the ODS publication and accompanying reference implementation {{ODS}}{{ODS-GITHUB}}.

ODS exemplifies the building blocks described earlier in this document and is consistent with the proposed interoperability layering, using standardized tool invocation for search and retrieval and agent-centric coordination to manage planning, execution, and refinement.

## Hybrid AI

Hybrid AI generally refers to an *edge–cloud cooperative* inference workflow in which two or more models collaborate to solve a task: (1) a **smaller on‑device model** (typically a few billion parameters) that prioritizes low latency, lower cost, and privacy; and (2) a **larger cloud model** (hundreds of billions to trillion‑scale parameters) that offers higher capability and broader knowledge. The two models coordinate over an agent‑to‑agent channel and may invoke tools locally or remotely as needed. Unlike single‑endpoint inference, Hybrid AI is *adaptive and budget‑aware*: the on‑device model handles as much work as possible locally (classification, summarization, intent detection, light reasoning), and escalates to the cloud model when additional capability is required (multi‑hop reasoning, long‑context synthesis, domain expertise). The models can exchange plans, partial results, and constraints over {{A2A}}, and both sides can discover and invoke tools via {{MCP}}.

### Building Blocks

A Hybrid AI workflow may generally comprise the components shown in the next Figure:

- **On‑device Model (Edge).**
  A compact LLM or task‑specific model (a few billion parameters) running on user hardware (e.g., phone, laptop). Advantages include: low latency for interactive turns, reduced cost, offline operation, and improved privacy by default (data locality). Typical functions: intent parsing, entity extraction, local retrieval, preliminary analysis, redaction/summarization prior to escalation.

- **Cloud Model (Hosted).**
  A large, higher‑capability LLM (hundreds of billions to ~trillion parameters) with stronger reasoning, knowledge coverage, tool‑use proficiency, and longer context windows. Typical functions: complex synthesis, multi‑step reasoning, broad web/KG retrieval, code execution, and advanced evaluation.

- **A2A Inter‑Model Coordination.**
  The edge and cloud models communicate via an **Agent‑to‑Agent** channel to exchange **capabilities**, **cost/latency budgets**, **privacy constraints**, **task state**, and **partial artifacts**. Common patterns include *negotiate‑and‑delegate*, *ask‑for‑help with evidence*, *propose/accept plan updates*, and *critique‑revise* cycles {{A2A}}.

- **MCP Tooling (Edge and Cloud).**
  Both models use the **Model Context Protocol** to discover and invoke tools with consistent schemas (e.g., local sensors/files, calculators, vector indexes on edge; search/crawling, KB/RAG, Python/services in cloud). MCP enables capability discovery, streaming/progress, cancellation, and explicit consent prompts across transports {{MCP}}.

- **Policy, Budget, and Privacy Controls.**
  Guardrails and policies that encode user/enterprise constraints (e.g., do not send raw PII to cloud; enforce token/time budgets; require consent for specific tools). The edge model may redact or summarize data before escalation; both sides log provenance and decisions for auditability.

- **Shared Task State and Provenance.**
  A compact state (goals, sub‑tasks, citations, hashes, timestamps) that both models can read/update to enable reproducibility, debugging, and verifiable traces.

~~~ ascii-art
+--------------------------------------------------------------+
|                        User / Client                         |
|              (Goal, Query, Constraints)                      |
+--------------------------------------------------------------+
                             |
                             v
+--------------------------------------------------------------+
|                 On-Device Model (Edge)                       |
|  - few-B params; low latency, privacy by default             |
|  - local reasoning, redaction/summarization                  |
|  - local tools via MCP (sensors, files, crypto)              |
+--------------------------------------------------------------+
         |                           \
         | local MCP tools            \ when escalation is needed
         v                             \
+----------------------+                \
| Edge MCP Tools       |                 \
+----------------------+                  v
                                   +----------------------------------+
                                   |   A2A Session (Edge <-> Cloud)   |
                                   |   - capability/budget exchange   |
                                   |   - task handoff & updates       |
                                   +----------------------------------+
                                                |
                                                v
+--------------------------------------------------------------+
|                    Cloud Model (Hosted)                      |
|  - 100B–1T+ params; higher capability & breadth              |
|  - complex synthesis, long-context reasoning                 |
|  - cloud tools via MCP (search, KB/RAG, Python)              |
+--------------------------------------------------------------+
                             |
                     cloud MCP tool calls
                             v
+----------------------+   +------------------+   +------------------+
| Web Search & Crawl   |-->| KB / RAG Index   |-->| Python / Services|
+----------------------+   +------------------+   +------------------+
                             ^
                             |
                 results/evidence via A2A to edge/cloud
                             |
                             v
+--------------------------------------------------------------+
|                 Final Answer / Output                        |
|   (synthesis + citations + privacy/consent notes)            |
+--------------------------------------------------------------+
~~~

Each building block in the Hybrid AI architecture represents a logical function rather than a specific implementation, and components may be co‑located or distributed in practice.

### Interaction Model

A typical Hybrid AI session proceeds as follows:

1. **Local First.** The on‑device model interprets the user goal, applies local tools (e.g., retrieve snippets, parse files), and attempts a low‑cost solution within configured budgets.
2. **Escalate with Minimization.** If the local model estimates insufficient capability (e.g., confidence below threshold, missing evidence), it **redacts/summarizes** sensitive data and **escalates** the task—along with compact evidence and constraints—over **{{A2A}}**.
3. **Cloud Reasoning + Tools.** The cloud model performs deeper reasoning and may invoke **{{MCP}}** tools (web search/crawl, KB/RAG, Python) to gather evidence and compute results.
4. **Refine & Return.** Intermediate artifacts and rationales flow back over **{{A2A}}**. The edge model may integrate results, perform final checks, and produce the user‑facing output.
5. **Iterate as Needed.** The models repeat plan‑act‑observe‑refine until success criteria (quality, coverage, cost/time budget) are met.

### Why This Use Case Matters

Hybrid AI is inherently *trade‑off aware*: it balances **privacy**, **latency**, and **cost** at the edge with **capability** and **breadth** in the cloud. Without standard protocols, inter‑model negotiations and tool interactions become bespoke and hard to audit. Two complementary interoperability layers are especially relevant:

- **Inter‑Model Coordination (A2A).**
  A2A provides a structured channel for **capability advertisement**, **budget negotiation**, **task handoffs**, **progress updates**, and **critique/revision** between edge and cloud models. This enables portable escalation policies (e.g., “do not send raw PII”, “cap tokens/time per turn”, “require human consent for external web calls”) and consistent recovery behaviors across vendors {{A2A}}.

- **Tool Invocation (MCP).**
  MCP standardizes tool discovery and invocation across both environments (edge and cloud), supporting consistent schemas, streaming/progress, cancellation, and explicit consent prompts. This allows implementers to swap local or remote tools—search, crawling, KB/RAG, Python/services—without rewriting agent logic or weakening privacy controls {{MCP}}.

**Implications for Hybrid AI.** Using standardized protocols lets implementers compose portable edge–cloud stacks:

- Edge‑first operation with **escalation** only when needed, guided by budgets and confidence.
- **Data minimization** (local redaction/summarization) and **consent** workflows at protocol boundaries.
- Consistent **provenance** (URIs, hashes, timestamps) and **observability** across edge and cloud for verifiable traces.
- Seamless **tool portability** (local/remote) and **policy enforcement** that travel with the task rather than the deployment.

## AI-based Troubleshooting and Automation

Telecom networks have significantly increased in scale, complexity, and heterogeneity. The interplay of technologies such as Software-Defined Networking (SDN), virtualization, cloud-native architectures, network slicing, and 5G/6G systems has made infrastructures highly dynamic. While these innovations provide flexibility and service agility, they also introduce substantial operational challenges, particularly in fault detection, diagnosis, and resolution.

Traditional troubleshooting methods, based on rule engines, static thresholds, correlation mechanisms, and manual expertise, struggle to process high-dimensional telemetry, multi-layer dependencies, and rapidly evolving conditions. Consequently, mean time to detect (MTTD) and mean time to repair (MTTR) may increase, impacting service reliability and user experience.

Artificial Intelligence (AI) and Machine Learning (ML) offer new capabilities to enhance troubleshooting. AI-driven approaches apply data-driven models and automated reasoning to detect anomalies, determine root causes, predict failures, and recommend or execute corrective actions, leveraging telemetry, logs, configuration, topology, and historical data.

Beyond troubleshooting, it is essential to further exploit network and service automation to enable coordinated, policy-based actions across multi-technology (e.g., RAN, IP, optical, virtualized), multi-layer, and dynamic environments. As degradations and faults often span multiple devices, domains, and layers, effective handling requires intelligent and increasingly autonomous mechanisms, ranging from proactive service assurance to automated fault-triggered workflows.

This use case envisions a multi-agent AI framework that enhances network and service automation. Agents perform diagnosis and root cause analysis (RCA), while also supporting anomaly prediction, intent-based protection, and policy-driven remediation. The proposed multi-agent interworking autonomously maintains the network in an optimal operational state by correlating heterogeneous data sources, performing collaborative reasoning, and interacting with network elements and operators through standardized protocols, APIs, and natural language interfaces.

AI agents form a distributed and scalable ecosystem leveraging advanced AI/ML, including generative AI (Gen-AI), combined with domain expertise to accelerate RCA, assess impact, and propose corrective actions. Each agent encapsulates capabilities such as data retrieval, hypothesis generation, validation, causal analysis, and action recommendation. Designed as composable and interoperable building blocks, agents operate across diverse domains (e.g., RAN, Core, IP, Optical, and virtualized infrastructures), while supporting lifecycle management, knowledge bases, and standardized interfaces.

### Building Blocks

The use case relies on decentralized multi-agent coordination, where agents exchange structured, context-enriched information to enable dynamic activation and collaborative troubleshooting workflows. A resource-aware orchestration layer manages agent deployment, scaling, and optimization across the network–cloud continuum. Policy frameworks ensure security, compliance, trustworthiness, and explainability, supporting resilient AI-driven network operations.

### Why This Use Case Matters

This use case highlights the need for interoperable, protocol-based integration of AI-driven troubleshooting and automation components within heterogeneous, multi-vendor environments. Telecom networks are inherently composed of equipment and control systems from different providers, spanning multiple administrative and technological domains. A multi-agent AI framework operating across such environments requires standardized mechanisms for data modeling, telemetry export, capability advertisement, and control interfaces. In particular, consistent information models (e.g., YANG-based models), secure transport protocols, and well-defined APIs are needed to ensure that AI agents can reliably discover, interpret, and act upon network state information across vendor boundaries.

Service discovery and capability negotiation are also critical. AI agents must be able to dynamically identify available data sources, peer agents, orchestration functions, and control points, as well as understand their supported features and policy constraints. This implies the need for standardized discovery procedures, metadata descriptions, and context exchange formats that enable composability and coordinated workflows in decentralized environments. Without such interoperability mechanisms, multi-agent troubleshooting systems risk becoming vertically integrated and operationally siloed.

Furthermore, governance, security, and trust frameworks are fundamental considerations. AI-driven agents capable of recommending or executing remediation actions introduce new requirements for authentication, authorization, accountability, and auditability. Secure communication channels, role-based access control, policy enforcement, and explainability mechanisms are necessary to prevent misuse, contain faults, and ensure compliance with operational and regulatory constraints.

## AI-Based Operation Models

### Agentic AI for Improved User Experience

AI agents have the potential to enhance future user experience by being integrated—individually or as collaborating groups—into telecom networks to deliver user-facing services. Such services may include autonomous multi-level Internet/Intranet search, coordination of calendar and email tasks, and execution of multi-step workflows involving multiple agents, as well as pre-built domain agents (e.g., HR, procurement, finance). This shift can fundamentally change enterprise operating models: agents can support decision-making and, where authorized, act on behalf of employees or the organization. In multi-agent scenarios, agents from different vendors communicate over networks and must interoperate. These interactions require coordinated communication flows and motivate a standardized agent communication protocol and framework. Given the need to comply with regulatory requirements (beyond network regulation), an open, standardized approach is preferable to proprietary implementations. Interoperability across operators and vendors implies an open ecosystem; therefore, a standardized AI agent protocol is required.

#### Building Blocks

TODO

#### Why this Use Case Matters

TODO

### Voice-Based Human-to-Agent Communication

With the integration of AI and AI agents into networks, voice services may see renewed importance as a natural, low-friction interface for interacting with agents. Voice-based human-to-agent communication can complement text-based chat interfaces and enable rapid task initiation and conversational control. This use case introduces additional considerations, including security, authorization/permissions, and charging/accounting. Because voice services are regulated in many jurisdictions, this further motivates a standardized framework and standardized AI agent protocol. Network-integrated AI agents can assist users through voice interaction and improve overall user experience.

#### Building Blocks

TODO

#### Why this Use Case Matters

TODO

# Security Considerations

TODO Security


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
