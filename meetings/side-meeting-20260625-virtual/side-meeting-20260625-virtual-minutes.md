# IETF Virtual Side Meeting, June 25, 2026 – Use Cases Taxonomy  
## Full Meeting Recap

## 1. Objective and Context
- Introduce and refine a proposed taxonomy for **agentic AI use cases in the IETF**: https://datatracker.ietf.org/doc/draft-scrm-aiproto-usecases/
- Follows prior outcome (IETF 125) identifying the need for:
  - A **common structure** across drafts
  - A way to **organize and compare use cases**
  - A foundation for **gap analysis and standardization**
- A **strawman taxonomy proposal** was presented to trigger feedback

## 2. Core Concept: What the Taxonomy Is (and Isn’t)

### What it is
- A framework to classify **capabilities required by use cases**
- A tool to:
  - Enable **gap analysis vs. existing protocols**
  - Identify **relevant working groups**
  - Provide **consistent structure across drafts**

### What it is not
- Not a strict classification into single buckets
- Not mutually exclusive
  - A use case can map to **multiple taxonomy elements**

> **Key clarification:**  
> The taxonomy behaves as a **multi-dimensional classifier**, not a strict hierarchy

## 3. Taxonomy Structure

- Defines **7 top-level domains**:
  - Transport  
  - Security & Trust  
  - Discovery  
  - Identity  
  - Coordination & Orchestration  
  - Data & Context Management  
  - Operations & Management  

- Each domain contains **subcategories**
- A use case selects **all applicable branches**

### Design Principle
- Maintain the right level of abstraction:
  - Avoid being **too granular**
  - Avoid being **too broad**

## 4. Taxonomy Application Model (“Taxonomy Profile”)

- Each use case draft may include a **taxonomy profile**
- Specifies:
  - Applicable taxonomy elements
  - Labels:
    - **Gap** → not covered by existing protocols  
    - **Covered** → already supported  
    - Optional: **Primary focus**

### Purpose
- Enable **structured gap analysis**
- Provide **clear positioning** of drafts
- Improve mapping to **IETF workstreams**

## 5. Worked Example: Discovery Use Cases Draft

Kehan presented draft as a concrete example: https://datatracker.ietf.org/doc/draft-kay-dawn-use-cases/

### AI Agent Lifecycle
- Registration → Discovery → Communication → Monitoring/Auditing

### Focus
- **Discovery phase**

### Key Characteristics
- **Target:** specific entity vs. class-based  
- **Scope:** single-domain vs. cross-domain  
- **Data:** static vs. dynamic  
- **Architecture:** must support **decentralized discovery**

### Discovery Workflow
- Intent-based discovery  
- Retrieve minimal information  
- Select candidate entity  
- Transition to communication phase  
- Possibly iterate discovery  

### Use Case Categories
- Capability-oriented  
- Resource-oriented (e.g., compute discovery)  
- Administrative scope extensions  
- Operational scenarios (e.g., troubleshooting, telemetry)

### Security & Privacy
- **Risks:**
  - Malicious discovery data  
  - Incorrect entity selection  
  - Data leakage  
- **Requirements:**
  - Authenticity  
  - Integrity  
  - Access control  

## 6. Key Discussion Themes

### A. Entities (Central Concept)
- Include: agents, workloads, data, services, tools, models
- Characteristics:
  - **Heterogeneous**
  - Share a **minimal common structure**

> Emerging idea: **Minimum Discoverable Information (MDI)** / common “entity object”

### B. One Taxonomy vs. Multiple
- Strong preference for:
  - **Single unified taxonomy**
- Extensions via:
  - Subclasses or abstraction

### C. Variability Across Entities
- Entities differ significantly (e.g., ephemeral vs. persistent)
- Need:
  - Better **entity profiles**
  - Possibly structured classification

### D. Abstraction and Generality
- Consensus:
  - Keep taxonomy **general and extensible**
- Avoid:
  - Early over-specialization
- Goal:
  - Work for **future unknown entity types**

### E. Missing Dimensions / Extensions
- Add **entity-focused classification**
- Consider **industry sector dimension**
- Capture **cross-sector interactions**

### F. Practical Adoption Challenges
- Concern:
  - Taxonomy profiles may be **burdensome**
- Suggestion:
  - Develop **tooling / automation**

### G. Evaluation and Metrics
- Taxonomy should enable:
  - **Evaluation frameworks**
  - **Performance analysis**
- Critical for:
  - Making taxonomy **actionable**

### H. Tooling and Ecosystem Integration
- Suggestions:
  - Use **GitHub** for collaboration
  - Integrate with:
    - IETF Datatracker
    - Classification tooling (e.g., ACM taxonomy)

- Goal:
  - Make taxonomy a **usable tool**, not just documentation

### I. External Coordination
- Align with:
  - Other SDOs
  - Open-source communities (e.g., Linux Foundation)
- Goal:
  - Avoid **ecosystem fragmentation**

## 7. Process and Next Steps

### Current Phase
- **Open feedback / iteration**

### Planned Actions
- Continue feedback collection:
  - Mailing list  
  - GitHub contributions  
- Refine taxonomy collaboratively  
- Present at **IETF 126**
- Organize follow-up **side meeting at IETF 126**

## Summary

- Convergence toward a **capability-based, multi-dimensional taxonomy**
- Key properties:
  - **Flexible (multi-label)**
  - **Abstract and extensible**
  - Supports **gap analysis and standardization planning**

### Open Challenges
- Modeling of **entities**
- Ensuring **usability and adoption**
- Adding **evaluation frameworks and tooling**

> The effort is **collaborative, evolving**
