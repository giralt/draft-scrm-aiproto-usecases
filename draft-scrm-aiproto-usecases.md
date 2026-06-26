---

title: "Taxonomy for Agentic AI Use Cases"
abbrev: "Taxonomy for Agentic AI Use Cases"
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

  ACM-CCS:
    title:  "ACM Computing Classification System"
    target: https://dl.acm.org/ccs

  DAWN-USE-CASES:
    title:  "Use Cases for the Discovery of Agents, Workloads, and Named Entities"
    target: https://datatracker.ietf.org/doc/draft-kay-dawn-use-cases/

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

# Use Cases Taxonomy {#sec-taxonomy}

This section defines a taxonomy for classifying Agentic AI use cases according to the functional protocol domains they exercise. The taxonomy serves three purposes: (1) it provides a structured vocabulary for describing and comparing use cases, (2) it helps identify the protocol areas that require IETF standardization attention, and (3) it helps map use cases and their requirements to relevant IETF working groups or areas.

The taxonomy is organized into seven top-level functional domains, derived from analysis of the use cases identified in ongoing IETF standardization discussions in the agent protocol space. The domains correspond to distinct clusters of protocol-level concerns. They are not mutually exclusive; most realistic agentic deployments span multiple domains simultaneously, and protocol work in one domain frequently depends on mechanisms defined in another.

The following figure presents the taxonomy in overview:

~~~ ascii-art
Agentic AI Use Cases Taxonomy
|
|-- 1. Transport
|   |-- 1.1. Session Management
|   |-- 1.2. Communication Modes
|   |-- 1.3. Content Multimodality
|   |-- 1.4. Performance
|
|-- 2. Security and Trust
|   |-- 2.1. Authentication
|   |-- 2.2. Authorization
|   |-- 2.3. Accountability
|   |-- 2.4. Integrity
|   |-- 2.5. Confidentiality
|   |-- 2.6. Safety
|
|-- 3. Discovery
|   |-- 3.1. Agent Discovery
|   |-- 3.2. Capability Advertisement
|   |-- 3.3. Tool Discovery
|   |-- 3.4. Service Negotiation
|
|-- 4. Identity
|   |-- 4.1. Agent Naming and Addressing
|   |-- 4.2. Credential Management
|   |-- 4.3. Delegation Chains
|   |-- 4.4. Selective Disclosure
|
|-- 5. Coordination and Orchestration
|   |-- 5.1. Task Lifecycle Management
|   |-- 5.2. Agent-to-Agent Interaction
|   |-- 5.3. Agent-to-Tool Interaction
|   |-- 5.4. Multi-Agent Workflow Orchestration
|   |-- 5.5. Cross-Domain Coordination
|
|-- 6. Data and Context Management
|   |-- 6.1. Context Exchange
|   |-- 6.2. Provenance and Citations
|   |-- 6.3. Knowledge Representation
|   |-- 6.4. Data Minimization
|
|-- 7. Operations and Management
    |-- 7.1. Observability
    |-- 7.2. Policy Enforcement
    |-- 7.3. Lifecycle Management
    |-- 7.4. Human-in-the-Loop
~~~
{: #fig-taxonomy artwork-align="left" title="Agentic AI Use Cases Taxonomy"}

The subsections below describe each domain and its constituent categories in detail.

## Transport

The *Transport* domain encompasses the protocol mechanisms required to carry agent communications reliably, efficiently, and with appropriate quality-of-service characteristics over Internet paths. Agent interactions introduce transport requirements that are more demanding than those of classical client-server applications: sessions may persist for minutes or hours, exchanges may involve heterogeneous data types, and the failure modes of multi-step workflows differ fundamentally from those of single-request transactions.

### Session Management

*Session Management* covers the establishment, maintenance, and termination of communication sessions between agents and between agents and tools. Agentic workflows are frequently long-lived, spanning multiple request-response cycles and potentially crossing network interruptions or endpoint restarts. Protocol mechanisms in this category include session establishment and capability negotiation, keep-alive and heartbeat signaling, graceful cancellation with defined cleanup semantics, progress reporting to prevent spurious timeouts at intermediaries, and state recovery after transient failures. Without robust session management, long-running agent tasks are vulnerable to silent failure, resource leakage, and loss of partial results that may be expensive to recompute.

### Communication Modes

*Communication Modes* covers the diversity of interaction patterns that agents and tools must support. Unlike classical RPC-style interfaces, agentic systems require a range of modes depending on the nature of the task: synchronous request/response for low-latency exchanges, asynchronous task submission and status polling for long-running work, incremental streaming for partial results (e.g., token-by-token generation or iterative search updates), server-initiated notifications for event-driven workflows, and bulk data transfer for large artifacts. Different protocols serve different modes — for example, HTTP/REST for request/response, Server-Sent Events or WebSocket for streaming, and Media over QUIC Transport (MOQT) for low-latency bidirectional flows — and a given use case may require multiple modes within a single session.

### Content Multimodality

*Content Multimodality* covers the protocol implications of exchanging heterogeneous content types within a single agentic interaction. Agents may need to process and transmit plain text, structured data (e.g., JSON), binary files, audio streams, video streams, images, and computation artifacts within the same session and sometimes within the same message. Protocol design must account for content negotiation, MIME type handling, framing, and ordering of heterogeneous payloads, particularly when modality switches occur mid-session or when different modalities carry conflicting quality-of-service requirements (e.g., low-latency audio alongside bulk file transfer).

### Performance

*Performance* covers the quantitative operating requirements that agentic use cases impose on transport protocols. Relevant dimensions include end-to-end latency (particularly for interactive agent sessions where user-facing response times matter), bandwidth requirements for multi-modal exchanges, jitter tolerance for real-time audio/video modalities, session scalability (the number of concurrent agent sessions a deployment must support), and cost or resource budget constraints that may govern escalation decisions (e.g., in edge-cloud hybrid architectures). Performance requirements vary widely across use cases and must be made explicit so that protocol designers can assess the fitness of candidate transport mechanisms.

## Security and Trust

The *Security and Trust* domain covers the protocol mechanisms needed to ensure that agentic interactions are conducted by verified entities, operate within authorized boundaries, produce auditable records, and preserve the integrity and confidentiality of exchanged data. Agentic systems introduce qualitatively new challenges compared to classical service architectures: agents act autonomously on behalf of users or other agents, authority is routinely delegated across multiple hops and organizational boundaries, and the consequences of unauthorized or erroneous actions can propagate and amplify through complex multi-agent pipelines.

### Authentication

*Authentication* covers the verification of the identity of agents, users, and tools prior to substantive interaction. In agentic systems, the principals that require authentication include not only human users but also software agents themselves — which may be ephemeral, dynamically spawned, or operated by third parties. Authentication mechanisms must therefore be applicable to workload identities (e.g., service accounts, container identities, or platform-attested credentials) as well as to individual agent instances. Cross-domain authentication is particularly challenging when agents operate across administrative boundaries that lack pre-established trust relationships.

### Authorization

*Authorization* covers the mechanisms by which an authenticated agent is permitted to perform specific operations on behalf of a principal. Agentic use cases typically involve multi-step delegation: a user authorizes an orchestrator agent, which in turn delegates authority to sub-agents, which invoke tools. Each link in this chain requires that the scope of authority be bounded and that delegation not introduce privilege escalation beyond what the delegating principal possessed. Relevant mechanisms include OAuth 2.0-based delegation flows, structured operation proposals subject to explicit authorization grants, user consent gates before sensitive operations are executed, and token formats that carry verifiable delegation chains.

### Accountability

*Accountability* covers the protocol mechanisms needed to produce auditable records of agent actions, traceable from a user's original intent to each action taken by an agent on that user's behalf. In regulated environments — and increasingly in general deployments — it is necessary to establish a reliable evidence chain that can be inspected after the fact. Relevant mechanisms include structured audit logs with cryptographic integrity guarantees, verifiable conversation records, proof-of-process tokens that attest to the sequence of steps executed, and non-repudiation properties that prevent an agent from disavowing actions it performed. Accountability is closely coupled with authentication and authorization, since the evidentiary value of an audit record depends on the strength of the identity and authorization evidence it references.

### Integrity

*Integrity* covers the protocol mechanisms that ensure agent messages and artifacts have not been modified in transit or at rest. Agentic pipelines can involve many hops across untrusted intermediaries, and the consequences of undetected tampering — incorrect tool invocations, falsified search results, corrupted planning state — can compound through the pipeline. Relevant mechanisms include end-to-end message authentication codes or digital signatures, replay protection (to prevent an attacker from re-injecting previously valid messages into a new context), and content-addressing schemes (e.g., hash-based artifact references) that allow recipients to verify data provenance independently of the transport channel.

### Confidentiality

*Confidentiality* covers the mechanisms that protect agent communications and data from unauthorized disclosure. Agentic workflows may involve sensitive user data, proprietary model outputs, or business-critical information that must not be exposed to intermediaries, peer agents, or tool providers outside their authorized scope. Protocol mechanisms include transport-layer encryption (e.g., TLS), end-to-end encryption for multi-hop exchanges, and data-minimization protocols by which an agent redacts or summarizes sensitive information before forwarding it to a less-trusted tier (for example, before escalating from an on-device edge model to a cloud model).

### Safety

*Safety* covers the protocol-level mechanisms that constrain agent behavior to prevent harmful, unintended, or policy-violating actions. While model-level safety measures are outside the IETF's scope, protocols can provide safety hooks that bound what agents are permitted to do: isolation boundaries around tool invocations that prevent access to resources outside an agent's authorized scope, explicit human-in-the-loop consent gates before high-impact or irreversible operations, and rate or quota enforcement mechanisms that limit the blast radius of malfunctioning or compromised agents. Safety mechanisms often interact with authorization mechanisms but are distinguished by their focus on operational constraints rather than identity-based access control.

## Discovery

The *Discovery* domain covers the protocol mechanisms by which agents, tools, and services locate and learn about one another at runtime. Effective discovery is a prerequisite for interoperability in open, decentralized agentic ecosystems where agents from different providers must collaborate without prior bilateral configuration. Discovery encompasses both the retrieval of existence information (where is a given agent or tool reachable?) and the retrieval of capability information (what can it do, and under what constraints?).

### Agent Discovery

*Agent Discovery* covers the mechanisms by which a client or orchestrator locates agents that can fulfill a given role. Discovery may operate via well-known URI conventions (analogous to `/.well-known/` resources in HTTP), DNS-based service discovery (extending established patterns such as DNS-SD to the agent domain), or registry-based approaches in which agents register with and are queried through centralized or federated directories. Discovery mechanisms must be scalable, resilient to partial failures, and designed to avoid creating single points of control or failure in open deployments.

### Capability Advertisement

*Capability Advertisement* covers the mechanisms by which an agent communicates its available skills, accepted input formats, produced output types, supported interaction modes, and applicable security constraints to potential callers. Standardized capability descriptor formats (such as Agent Cards in the A2A protocol) enable automated capability matching — allowing orchestrators to select appropriate agents dynamically — and eliminate the need for bespoke, bilateral configuration between agents. Capability descriptors must be versioned to support evolution and must be structured to allow partial matching when a caller requires only a subset of an agent's capabilities.

### Tool Discovery

*Tool Discovery* covers the mechanisms by which agents locate and enumerate tools available for invocation. The Model Context Protocol (MCP) provides a concrete example: agents query an MCP server for its list of available tools, each described with a typed schema, before deciding which tool to invoke for a given sub-task. Tool discovery must support dynamic registration and de-registration of tools, capability versioning, and schema negotiation to accommodate heterogeneous tool ecosystems that evolve independently of the agents that use them.

### Service Negotiation

*Service Negotiation* covers the protocol exchanges by which an agent and a peer (another agent or a tool) agree on interaction parameters before substantive communication begins. Negotiation may cover supported protocol versions, content types, authentication and authorization mechanisms, quality-of-service parameters, resource quotas, and privacy constraints. Explicit service negotiation reduces the frequency of mid-session failures caused by incompatible assumptions and provides a natural point at which policies can be enforced at capability boundaries before potentially expensive work is initiated.

## Identity

The *Identity* domain covers the mechanisms that establish and manage the stable, verifiable identities of agents throughout their operational lifecycle. Identity is a prerequisite for authentication, authorization, and accountability, but it raises distinct protocol concerns: agents may be spawned and decommissioned dynamically, may migrate across infrastructure, and may delegate their authority to other agents in ways that must remain traceable. The Identity domain is closely related to the Security and Trust domain but focuses specifically on the naming, addressing, and credential infrastructure that underpins identity-based security mechanisms.

### Agent Naming and Addressing

*Agent Naming and Addressing* covers the syntactic and semantic conventions used to identify agents in protocol messages. A naming scheme must be unambiguous within its intended scope, stable enough to support persistent references and audit records, and structured to enable efficient lookup or routing. Names and addresses may be decoupled — an agent may carry a stable logical name (identifier) and a separately resolved network address (locator) — analogous to the distinction between domain names and IP addresses in the Internet architecture. Standardized naming conventions are a prerequisite for interoperable discovery, delegation, and accountability.

### Credential Management

*Credential Management* covers the mechanisms by which agents are provisioned with credentials (e.g., private keys, tokens, or certificates) and by which those credentials are rotated, revoked, and verified. In agentic deployments, credentials must be deliverable to dynamically created agent instances without manual pre-configuration, and must be scoped to the specific agent, task, or deployment context in order to limit the impact of credential compromise. Relevant mechanisms include platform-attested identity (e.g., SPIFFE/SPIRE), short-lived token issuance by trusted authorities, and automated rotation procedures that operate without disrupting in-flight agent sessions.

### Delegation Chains

*Delegation Chains* covers the protocol representations of transitive authority: the sequence of principals through which a user's original authorization has been conveyed to a given agent. In multi-agent pipelines, an action by a leaf agent may reflect authority delegated through several intermediate agents, each of which may have further constrained the delegated scope. Delegation chain representations must be compact, cryptographically verifiable, and structured to enforce monotonic scope narrowing — i.e., a delegate cannot acquire authority beyond what its delegator possessed. Cross-domain delegation is particularly challenging when the delegating and receiving agents are operated by different organizations under different identity management systems.

### Selective Disclosure

*Selective Disclosure* covers mechanisms that allow an agent to reveal only the subset of its identity and capability attributes that are relevant and appropriate for a given interaction. Full disclosure of an agent's capabilities during discovery can create information-leakage risks (revealing proprietary service details) and linkability risks (enabling cross-context tracking of agent identity). Selective disclosure mechanisms — such as those based on SD-JWT — allow agents to present minimal, context-appropriate credential subsets while maintaining cryptographic verifiability of the disclosed attributes.

## Coordination and Orchestration

The *Coordination and Orchestration* domain covers the protocol mechanisms that govern how agents and tools are composed into coherent multi-step workflows. Agentic systems are inherently distributed: a user request is decomposed into sub-tasks, assigned to specialized agents, executed — potentially in parallel and across administrative domains — and the results are synthesized into a final response. Coordination protocols must support the full lifecycle of this process reliably and in a manner that is interoperable across agents from different providers.

### Task Lifecycle Management

*Task Lifecycle Management* covers the protocol operations that govern a delegated task from its creation to its completion or cancellation. A well-specified task lifecycle includes: task creation with a structured description of the work to be performed; acknowledgment and queuing semantics; status inquiry and progress reporting; mid-execution cancellation with defined cleanup guarantees; and delivery of the final result or a structured error response. The lifecycle must be robust to partial failures, network interruptions, and agent restarts, and must provide sufficient progress visibility to allow orchestrators to make informed re-planning decisions without relying on timeouts alone.

### Agent-to-Agent Interaction

*Agent-to-Agent Interaction* covers the protocol patterns by which peer agents exchange messages, negotiate task assignments, and share partial results. The Agent2Agent (A2A) protocol is an example of a specification targeting this category. Key interaction patterns include synchronous request/response for sub-task delegation, streaming for incremental result delivery, asynchronous task handoff for long-running work, and critique/revision cycles in which one agent reviews and refines the output of another. Agent-to-agent protocols must accommodate opaque agent implementations — neither agent need expose its internal model or reasoning process — and must support collaboration across organizational boundaries where one agent's internals are proprietary.

### Agent-to-Tool Interaction

*Agent-to-Tool Interaction* covers the protocol patterns by which agents invoke external capabilities exposed as tools. The Model Context Protocol (MCP) is an example of a specification targeting this category. Tools differ from agents in that they are typically stateless (or near-stateless), do not reason autonomously, and expose well-defined, typed input/output interfaces. Agent-to-tool protocols must support capability schema description, consistent error reporting, streaming of partial outputs, and cancellation of in-progress invocations. Isolation semantics — ensuring that a tool invocation cannot access resources beyond its declared scope — are also a concern in this category.

### Multi-Agent Workflow Orchestration

*Multi-Agent Workflow Orchestration* covers the higher-level coordination mechanisms needed to manage complex workflows involving multiple cooperating agents. Orchestration concerns include task decomposition strategies (sequential, parallel, or conditional branching), shared working memory accessible to all agents in a workflow, synchronization primitives (fan-out, fan-in, barrier synchronization), and iterative refinement loops in which agents re-plan and re-execute based on intermediate results. Orchestration protocols must be composable — allowing sub-workflows to be embedded within larger workflows — and must expose sufficient visibility and control hooks for human operators to monitor and intervene in executing workflows.

### Cross-Domain Coordination

*Cross-Domain Coordination* covers the additional protocol mechanisms required when agent workflows span multiple administrative domains. Cross-domain coordination introduces challenges absent in single-domain deployments: heterogeneous trust models must be reconciled, authorization scopes must be negotiated across domain boundaries, and attribution or billing mechanisms may be required. Protocols in this category must support federation — allowing domains to recognize and honor each other's identity and authorization assertions without requiring pre-established bilateral relationships — while preserving each domain's ability to enforce its own local policies.

## Data and Context Management

The *Data and Context Management* domain covers the protocol mechanisms governing how agents represent, exchange, persist, and protect the information they operate on. Agentic workflows generate and consume substantial intermediate state: partial results, retrieved artifacts, reasoning traces, and provenance records. Protocol-level conventions for managing this data are essential for reproducibility, auditability, and interoperability across agents from different providers.

### Context Exchange

*Context Exchange* covers the mechanisms by which agents share the task-relevant state needed for coherent collaboration. Context may include the original user request, accumulated intermediate results, agent-specific observations, constraints, and shared working memory. Protocol mechanisms must define efficient representations for context (to avoid overwhelming bandwidth or model context window limits), versioning and conflict resolution for concurrently updated shared state, and access controls that limit each agent's visibility into context to what is necessary for its assigned sub-task.

### Provenance and Citations

*Provenance and Citations* covers the protocol mechanisms that link agent-produced outputs to the source materials from which they were derived. In research, regulatory, and high-stakes decision-making contexts, it is essential that the claims made by an agent can be traced to verifiable sources. Relevant protocol mechanisms include structured citation formats that capture source URI, retrieval timestamp, content hash, and the relevant excerpt; provenance chains that attribute composite outputs to their constituent sources; and integrity checks that allow a third party to independently verify that cited content has not been altered since retrieval.

### Knowledge Representation

*Knowledge Representation* covers the data formats and schemas used by agents to represent structured domain knowledge in a form that peer agents can interpret consistently. Standardized information models are particularly important in multi-agent systems where different agents must share and act upon the same domain-specific data — such as network topology models in AIOps deployments (e.g., YANG-based models in the IETF operations area) or structured bibliographic records in research workflows. Consistent knowledge representations reduce the need for per-agent data translation and enable semantic interoperability across agents from different providers.

### Data Minimization

*Data Minimization* covers the protocol mechanisms that limit the exposure of sensitive data as it flows through multi-agent pipelines. In workflows spanning administrative domains or involving untrusted intermediaries, agents should transmit only the minimum information necessary for a peer to perform its sub-task. Relevant mechanisms include structured redaction (replacing sensitive fields with anonymized tokens), summarization-before-escalation (e.g., in hybrid edge-cloud architectures where a local model redacts data before forwarding to a cloud model), and privacy-preserving context encoding. Data minimization is simultaneously a privacy mechanism and a safety mechanism, as limiting information exposure also bounds the consequences of a compromised intermediary.

## Operations and Management

The *Operations and Management* domain covers the protocol mechanisms that support the operational lifecycle of agentic systems: monitoring their behavior, enforcing policies at runtime, managing agent lifecycles, and enabling human oversight. These mechanisms are essential for deploying agentic systems safely and reliably in production environments, and they are particularly prominent in network management use cases (such as AI-based troubleshooting) where agents must operate across multi-vendor, multi-domain infrastructure.

### Observability

*Observability* covers the protocol mechanisms that expose the runtime behavior of agentic systems to operators and auditors. Relevant mechanisms include distributed tracing (propagating trace context across agent hops to reconstruct end-to-end execution traces), structured logging of agent decisions and tool invocations, and telemetry export for performance monitoring and anomaly detection. Observability mechanisms must be designed to operate across administrative domain boundaries, enabling an operator to trace a workflow that spans agents from multiple providers, while respecting each provider's data governance and confidentiality constraints.

### Policy Enforcement

*Policy Enforcement* covers the protocol mechanisms that ensure agents operate within defined behavioral and resource boundaries. Policies may govern resource consumption (e.g., rate limits, quota caps, cost budgets), data handling (e.g., prohibition on forwarding certain data categories to external providers), and operational scope (e.g., restricting an agent to read-only access to a particular system). Policy enforcement mechanisms must be expressible in standard formats, enforceable at protocol boundaries (e.g., at agent gateways or authorization servers), and auditable so that violations can be detected, attributed, and remediated.

### Lifecycle Management

*Lifecycle Management* covers the protocol mechanisms for deploying, registering, updating, and decommissioning agents in dynamic environments. In practice, agents may be instantiated on demand, scaled horizontally to handle concurrent tasks, updated to new model or capability versions, and eventually decommissioned — ideally without disrupting in-flight workflows. Relevant protocol mechanisms include agent registration and capability publication, health checking and readiness signaling, graceful drain and shutdown procedures, and version negotiation to ensure that newly deployed agents can interoperate with peers running earlier versions of the same protocol.

### Human-in-the-Loop

*Human-in-the-Loop* covers the protocol mechanisms that insert human oversight and decision-making into agentic workflows at appropriate points. Not all agent decisions can or should be made autonomously: high-impact, irreversible, or ambiguous actions may require explicit human approval before execution. Relevant protocol mechanisms include structured approval request formats that surface proposed actions and their anticipated consequences to a human operator, consent flows with well-defined timeout and default-action semantics for unanswered requests, and escalation paths that route exceptional cases to human reviewers. Human-in-the-loop mechanisms are closely related to both authorization (which governs what requires approval) and safety (which enforces that unapproved high-risk actions are blocked at the protocol level).

# Applying the Taxonomy: A Worked Example

## Overview

The taxonomy defined in {{sec-taxonomy}} is intended to be used as a classification instrument for individual Agentic AI use case documents. Its role is analogous to the ACM Computing Classification System (CCS), which authors use to attach a structured set of descriptors to a research paper so that readers and indexing systems can quickly understand its scope and locate related work {{ACM-CCS}}.

The taxonomy can be visualized as a tree: each top-level domain (e.g., Transport, Security and Trust, Discovery) is a primary branch, and each subcategory (e.g., Session Management, Authentication, Agent Discovery) is a leaf. Given an Agentic AI use case, its *taxonomy profile* is the set of root-to-leaf paths that apply to it. For example, a use case that focuses exclusively on session management and mutual authentication between agents would carry the profile:

~~~
Transport => Session Management
Security and Trust => Authentication
~~~

Authors of individual use case Internet-Drafts are encouraged to include a dedicated taxonomy section that enumerates the applicable paths using this notation. Doing so serves three purposes. First, it makes the protocol scope of the use case explicit and comparable to other use cases in a machine-readable way. Second, it guides gap analysis by mapping each path to the protocol area where standardization work is needed. Third, it facilitates working group ownership decisions by surfacing cross-area dependencies — a use case with paths into both Transport and Security and Trust, for instance, signals that coordination between working groups in those areas will be required.

When constructing a taxonomy profile, authors should include all paths that represent substantive protocol-level requirements of the use case, and omit paths that are merely incidental or out of scope for IETF standardization. Authors should also note whether a given path represents a "gap" (no adequate existing IETF mechanism) or "covered" (an existing IETF protocol that the agentic use case builds upon).

## Example: Discovery of Agents, Workloads, and Named Entities

This section illustrates the taxonomy methodology by applying it to the Internet-Draft "Use Cases for the Discovery of Agents, Workloads, and Named Entities" {{DAWN-USE-CASES}}, which describes scenarios in which distributed entities — including AI agents, software services, compute workloads, network functions, and application endpoints — must locate and obtain minimum descriptive information about peer entities before communication or selection can begin.

The DAWN use cases span four discovery categories: capability-oriented discovery (locating entities that offer a needed function), resource-oriented discovery (locating data sources, compute pools, or inference services), administrative scope extensions (discovery across organizational or tenancy boundaries), and operational discovery (supporting human operators or management systems in locating entities across heterogeneous infrastructure). The draft explicitly excludes registration procedures, authentication, naming governance, candidate selection, and task orchestration from its scope; however, it identifies security and privacy considerations — including authenticity and integrity of discovery metadata, authorization controls on discovery queries, and privacy risks arising from the exposure of requester intent or deployment topology — as important constraints on any protocol that addresses the in-scope scenarios.

The taxonomy profile for this use case is as follows.

**Transport ->**

- **Session Management.** Discovery interactions range from simple single-round-trip queries to multi-step lookups that cross administrative boundaries. The discovery protocol must define session semantics sufficient to support these patterns reliably, including error recovery and cancellation for multi-hop discovery flows.

- **Communication Modes.** The DAWN use cases require at minimum a synchronous request/response mode for point-in-time capability lookups. Operational discovery scenarios — where management systems monitor dynamic entity populations — additionally motivate asynchronous notification or subscription patterns so that observers receive timely updates when entity availability or metadata changes.

**Security and Trust ->**

- **Authentication.** The DAWN draft identifies the authenticity and integrity of discovery metadata as a key security consideration: a requester must be able to verify that the metadata returned by a discovery lookup genuinely describes the entity it purports to describe, and has not been tampered with in transit or at the registry. This motivates cryptographic authentication of discovery responses, distinct from — but complementary to — the authentication of the discovered entity itself.

- **Authorization.** Discovery queries may themselves require authorization: not all requesters should be permitted to enumerate all entities within a registry, particularly across organizational boundaries. The DAWN use cases motivate authorization controls on discovery query scope, consistent with the principle that the ability to discover an entity should be governed by policy rather than assumed to be universally open.

- **Accountability.** Operational discovery scenarios, in which management systems or AIOps agents enumerate and monitor entities across a production infrastructure, motivate audit logging of discovery queries and responses. Accountability mechanisms allow operators to reconstruct which systems queried which entities at what time, supporting both forensic analysis and compliance with data governance requirements.

**Discovery ->**

- **Agent Discovery.** This is the primary focus of the DAWN document. All four discovery categories directly address the problem of locating agents or agent-equivalent entities (workloads, services, network functions) that satisfy a stated requirement. The use cases motivate standardized lookup mechanisms operating at multiple scopes: within an administrative domain, across organizational boundaries, and across heterogeneous multi-vendor infrastructure.

- **Capability Advertisement.** Capability-oriented discovery presupposes that entities publish structured descriptions of their offered functions. The DAWN use cases identify the need for a "minimum discoverable information" set — a standardized baseline of metadata that any discoverable entity must expose — to enable consistent matching across heterogeneous registries or lookup services.

- **Service Negotiation.** Administrative scope extension scenarios involve discovery queries that cross organizational or tenancy boundaries, which requires agreement on the scope, format, and authorization of discovery requests between domains. While the DAWN draft does not define a negotiation protocol, it motivates one by exposing the need for scoped discovery views and cross-domain metadata exchange.

**Identity ->**

- **Agent Naming and Addressing.** The DAWN document explicitly addresses named entities and identifies naming as a prerequisite for discovery. A discovery protocol must be able to resolve both human-readable names and machine-oriented identifiers to network-reachable endpoints. The use cases surface the need for naming conventions that are stable enough to support persistent references while remaining resolvable across administrative boundaries.

- **Selective Disclosure.** As noted in the Data and Context Management domain above, the DAWN privacy considerations motivate mechanisms by which discoverable entities can reveal capability attributes selectively — exposing different levels of detail to different requesters based on authorization context — without compromising the cryptographic verifiability of the disclosed information.

**Data and Context Management ->**

- **Provenance and Citations.** The DAWN draft notes that dynamic properties (such as load, price, and availability) change too rapidly to be reliably embedded in discovery records, and recommends that stable discovery metadata include a pointer to a separate service for current state. This architectural pattern motivates a provenance convention by which discovery responses carry verifiable references to the authoritative source of each metadata field, enabling requesters to distinguish stable identity attributes from volatile operational properties and to verify the freshness of each.

- **Data Minimization.** The DAWN draft identifies privacy risks arising from the exposure of requester intent through discovery queries and from the publication of deployment topology or model provenance through entity metadata. These considerations motivate data minimization at both ends: requesters should be able to issue queries without disclosing more about their intent than is necessary, and entities should be able to publish capability descriptors that reveal only the information required for matching without exposing proprietary implementation details. This overlaps with the Selective Disclosure subcategory of the Identity domain.

The following figure summarizes the taxonomy profile in compact notation:

~~~ ascii-art
DAWN Use Cases Taxonomy Profile
|
|-- Transport
|   |-- Session Management   (covered)
|   |-- Communication Modes  (gap)
|
|-- Discovery
|   |-- Agent Discovery           [primary focus; gap]
|   |-- Capability Advertisement  (gap)
|   |-- Service Negotiation       (gap)
|
|-- Identity
|   |-- Agent Naming and Addressing  (gap)
|   |-- Selective Disclosure         (gap)
|
|-- Security and Trust
|   |-- Authentication  (gap)
|   |-- Authorization   (gap)
|   |-- Accountability  (covered: existing audit log conventions)
|
|-- Data and Context Management
    |-- Provenance and Citations  (gap)
    |-- Data Minimization         (gap)
~~~
{: #fig-dawn-taxonomy artwork-align="left" title="Taxonomy profile for draft-kay-dawn-use-cases"}

Paths annotated (gap) identify areas where no adequate existing IETF protocol mechanism covers the stated requirement. Paths annotated (covered) identify areas where existing IETF mechanisms provide a foundation that the agentic use case extends or constrains.

# Security Considerations

TODO Security


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
