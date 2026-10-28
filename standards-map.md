# Standards crosswalk

*For security leaders and auditors who arrive with their own framework. Current as of 28 October
2026. Terms are defined in [glossary.md](glossary.md).*

## In short

- **What it is.** For each item of three external standards, MITRE ATLAS, the OWASP Top 10 for
  Agentic Applications and the NIST Secure Software Development Framework (SSDF): the method's
  controls that relate to it, how strongly, and from which profile. It is non-normative: it adds no
  control and changes no profile.
- **What it is not.** It is not a compliance claim, a certification or an attestation, and it must
  not be used to support one. Most of the method's proof is self-attested, its High-profile
  controls are specified but not yet operated in practice, and its evidence base is a single
  product (see the Maturity and Origin paragraphs in [README.md](README.md)). A relation says what a
  control is designed for, not that it has been shown to work.
- **How to use it.** Jump to your standard, find the item's row by searching the page for its ID,
  read the relation and the note, and follow the link to the control in
  [control-reference.md](control-reference.md).

**Jump to:** [MITRE ATLAS](#mitre-atlas) ·
[OWASP Top 10 for Agentic Applications](#owasp-top-10-for-agentic-applications) ·
[NIST SSDF](#nist-ssdf) · [Gaps](#gaps-the-crosswalk-shows) · [Sources](#sources)

## Which standards are included

Standards enter by one rule: a standard from a recognised standards body (ISO/IEC, NIST), or a
widely used community framework (MITRE, OWASP). Vendor frameworks are not included.
Organisation-level AI governance standards, such as the NIST AI RMF and ISO/IEC 42001, sit outside
what the method governs; [related-work.md](related-work.md) places them, and they are not mapped.
ISO/IEC 27090:2026, published in October 2026, is a candidate that is not yet mapped.

## How to read the tables

Each row is one item of the standard, and the direction is always from the standard to the method.
The **Method controls** cell lists what relates to the item, most relevant first:

- an entry with a layer (L1 to L8) and a capital letter is a control, named as in its row in
  [control-reference.md](control-reference.md), shortened to its first words where the name is
  long, and linked to its layer;
- a lower-case entry, such as "frozen-lockfile installs", is one part of a control, linked to where
  it is described;
- an entry without a layer, such as "Sustain mode", is a part of the method or the practice, linked
  to [method.md](method.md) or [practice.md](practice.md).

The layers are
[L1 Infrastructure isolation](control-reference.md#l1-infrastructure-isolation),
[L2 Session isolation](control-reference.md#l2-session-isolation),
[L3 Tool control](control-reference.md#l3-tool-control),
[L4 Permission sets and delegation](control-reference.md#l4-permission-sets-and-delegation),
[L5 Repository controls](control-reference.md#l5-repository-controls),
[L6 Message controls](control-reference.md#l6-message-controls),
[L7 Process controls](control-reference.md#l7-process-controls) and
[L8 State and audit](control-reference.md#l8-state-and-audit).

The **Relation** cell takes one of four values:

| Relation | Meaning |
|---|---|
| **Addresses** | A control kept by a setting or a boundary is designed to stop or contain the item, and no risk the method accepts at that profile undoes it |
| **Partly** | A control reduces or detects the item, or covers only part of it |
| **Related** | A control touches the item but does not mitigate it |
| **Outside** | The method has no control for the item: it is out of scope, or left to the adopter's normal process |

- **Detection and expectations are Partly at most.** Scans, classifiers and reviews detect; a rule
  in a brief is followed or not. Neither makes a relation Addresses. A deny rule that matches
  command text holds against mistakes, not against an adversary, so it counts the same way.
- **By containment** means the control limits what a misled agent can reach, not the step itself,
  such as the injection.
- **The profile in brackets** is the lowest profile at which the relation holds. Where it grows
  with the profile, the cell gives both, for example "Partly (Standard); Addresses (High)". A
  relation can be narrowed to part of the item ("for agent tooling") or split into parts, each with
  its own profile. Outside takes no profile.
- **For SSDF practices**, which are outcomes rather than threats, Addresses means a control's
  purpose is the practice's outcome for the agent part of the work, and Partly means it serves some
  of the practice's tasks, or only at some profiles. A Partly row is not partial conformance.

The profiles are chosen per layer ([method.md](method.md#12-assurance-profiles), §12), and what a
lower profile requires also holds at the higher ones, unless a higher profile replaces it, as split
environments replace the one shared environment and signed grants replace prompt answers:

- **Lite:** one shared isolated environment or a dedicated machine, deny rules, read-only
  enforcement material, the owner performing every outward action, PRs and host-enforced CI.
- **Standard:** adds environments split by authority and input trust, an egress allow-list,
  per-environment credentials, role permission sets, grants through the tool's permission prompt,
  authenticated messaging and a security review.
- **High:** adds a VM per session, per-ref push enforcement, signed grants, a private message bus,
  the classifiers, signed commits, reproducible builds and human reading of high-risk slices.

The **Note** is one sentence in our own words. No text of the standards is copied beyond item IDs
and names.

**Terms used in the notes.** Full definitions are in [glossary.md](glossary.md).

- **Owner:** the human who holds authority: approves plans, merges, releases and performs every
  outward action.
- **Orchestrator:** the agent that plans, assigns, reviews and integrates the workers' branches.
- **Session:** one running agent in its environment. A **worker** is a session that builds a
  **slice**, one planned unit of work.
- **Ingestion session:** a session assigned to read external content, such as web pages, issues and
  advisories. From Standard it runs in its own environment.
- **Environment authority:** the right to change what other or later sessions get by default, or to
  act on lab resources. From Standard it is assigned to named sessions at kickoff; at Lite, to none.
- **Internal remote:** the git remote the agent sessions push to; the orchestrator's integration
  branches and the ledger live there.
- **Ledger:** the orchestrator's record of state and history, kept on its own git ref.
- **Protected paths:** files whose changes reach the owner as the diff, such as permission settings,
  hooks, CI workflows and agent instruction files.
- **High-risk slice:** a slice the plan classes as high-risk. At High, a human reads its diff.
- **Residual:** a risk the method accepts and records at a profile
  ([method.md](method.md#12-assurance-profiles), §12).

## MITRE ATLAS

MITRE ATLAS is a knowledge base of adversary tactics and techniques against AI systems, maintained
by MITRE. This crosswalk uses content release 2026.09 of 15 September 2026, read on 28 October 2026
from the atlas-data repository (`dist/v6/ATLAS-2026.09.yaml`), the data that atlas.mitre.org
renders.

Rows are grouped by theme rather than by tactic, because many techniques serve more than one
tactic. Every technique in release 2026.09 has either a row or an entry in
[Outside the method's scope](#outside-the-methods-scope): search the page for its ID, or for its
parent's ID if it is a sub-technique. A parent technique is covered by the rows and entries of its
sub-techniques. Where techniques share a mapping, they share a row, and every ID is in the ID cell.
ATLAS mitigations are not mapped. ATLAS publishes monthly and IDs can move between releases, so the
rows hold for release 2026.09.

**Themes:** [Injected or manipulated input](#injected-or-manipulated-input) ·
[Persistence in agent context, memory and configuration](#persistence-in-agent-context-memory-and-configuration) ·
[Tool invocation, execution and escape](#tool-invocation-execution-and-escape) ·
[Credential access and use](#credential-access-and-use) ·
[Collection, exfiltration and command-and-control](#collection-exfiltration-and-command-and-control) ·
[Agent tooling and software supply chain](#agent-tooling-and-software-supply-chain) ·
[Agent autonomy and communication between agents](#agent-autonomy-and-communication-between-agents) ·
[Discovery and reconnaissance](#discovery-and-reconnaissance) ·
[Model-level evasion](#model-level-evasion) ·
[Outside the method's scope](#outside-the-methods-scope)

*Technique IDs and names from MITRE ATLAS™ release 2026.09, © 2021–2026 The MITRE Corporation, used
under the [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0). MITRE ATLAS™ and MITRE
ATT&CK® are a trademark and registered trademark of The MITRE Corporation. This crosswalk is not
produced or endorsed by MITRE.*

### Injected or manipulated input

| ID | Name | Method controls | Relation | Note |
|---|---|---|---|---|
| AML.T0051.000 | LLM Prompt Injection: Direct | L6 [Authority never relayed](control-reference.md#l6-message-controls); L4 [Grants as the owner's answer to the tool's own permission prompt](control-reference.md#l4-permission-sets-and-delegation), [Owner-signed, instance-bound, time-bound grants](control-reference.md#l4-permission-sets-and-delegation) | Partly (Lite) | A prompt from the orchestrator or a peer can steer work inside the session's scope but cannot widen its authority. |
| AML.T0051.001 | LLM Prompt Injection: Indirect | L1 [Isolated environment](control-reference.md#why-sessions-are-separated-and-how-far) split by input trust, [Fetch tools denied outside ingestion environments](control-reference.md#l1-infrastructure-isolation), [Egress allow-list](control-reference.md#l1-infrastructure-isolation); L2 [Per-ref push enforcement](control-reference.md#l2-session-isolation); L6 [Filtering and provenance on untrusted content](control-reference.md#l6-message-controls), [context-sharing rules](control-reference.md#context-sharing) | Partly (Lite); Addresses (High), by containment of the ingesting session, for fetched content | From Standard only ingestion sessions read external content, but injected text that survives into their committed output reaches the orchestrator second-hand, where L4 bounds what it can make it do; below High their push credential reaches every ref on the internal remote, and text in dependencies, logs and tool output reaches every session at every profile. |
| AML.T0051.002 | LLM Prompt Injection: Triggered | L1 [Isolated environment](control-reference.md#why-sessions-are-separated-and-how-far) split by input trust; L6 [inbound message policy](control-reference.md#the-message-bus) | Partly (Standard) | Sessions start when the owner or the orchestrator launches them, and the method has no rule for agents triggered by outside events. |
| AML.T0093 | Prompt Infiltration via Public-Facing Application | L1 [Isolated environment](control-reference.md#why-sessions-are-separated-and-how-far) split by input trust, [Fetch tools denied outside ingestion environments](control-reference.md#l1-infrastructure-isolation); L2 [Per-ref push enforcement](control-reference.md#l2-session-isolation); L6 [Filtering and provenance on untrusted content](control-reference.md#l6-message-controls) | Partly (Standard); Addresses (High), by containment | Content planted in an issue tracker or an advisory feed reaches only ingestion sessions, but below High their push credential reaches every ref on the internal remote. |
| AML.T0078 | Drive-by Compromise | L1 [Fetch tools denied outside ingestion environments](control-reference.md#l1-infrastructure-isolation), [Egress allow-list](control-reference.md#l1-infrastructure-isolation); L2 [Per-ref push enforcement](control-reference.md#l2-session-isolation); L6 [Filtering and provenance on untrusted content](control-reference.md#l6-message-controls) | Partly (Standard); Addresses (High), by containment | Only ingestion sessions browse, but below High their push credential reaches every ref on the internal remote, and Lite has neither a fetch restriction nor egress control. |
| AML.T0099 | AI Agent Tool Data Poisoning | L1 [Isolated environment](control-reference.md#why-sessions-are-separated-and-how-far) split by input trust; L5 [No commits on main; PRs](control-reference.md#l5-repository-controls); L7 [Deliverable review on the final diff](control-reference.md#l7-process-controls); L3 [Action classifier](control-reference.md#l3-tool-control) | Partly (Lite) | Poisoned content in repository files, dependency source and tool results is a residual at every profile. |
| AML.T0100 | AI Agent Clickbait | L1 [Fetch tools denied outside ingestion environments](control-reference.md#l1-infrastructure-isolation), [Isolated environment](control-reference.md#why-sessions-are-separated-and-how-far) split by input trust; L2 [Per-ref push enforcement](control-reference.md#l2-session-isolation); L7 [Deliverable review on the final diff](control-reference.md#l7-process-controls) | Partly (Standard); Addresses (High), by containment | Only ingestion sessions browse and code from a lure reaches the product only through review, but below High the session's push credential reaches every ref on the internal remote. |
| AML.T0134 | AI Targeted Cloaking | L1 [Isolated environment](control-reference.md#why-sessions-are-separated-and-how-far) split by input trust; [capability gates](method.md#7-the-capability-library) | Partly (Standard) | Containment holds, but no rule requires a source to be fetched again by a different client before its content is promoted. |
| AML.T0068 | LLM Prompt Obfuscation | L6 [Decision classifier](control-reference.md#l6-message-controls), [Content as repository references](control-reference.md#l6-message-controls); L3 [Action classifier](control-reference.md#l3-tool-control) | Partly (High) | Obfuscation defeats detection, not boundaries, and the classifiers are not a bound against an adaptive attacker. |
| AML.T0129 | Triggers in Multimodal Inputs | L1 [Isolated environment](control-reference.md#why-sessions-are-separated-and-how-far) split by input trust | Related (Standard) | No filter, classifier or calibration set covers images, documents or file metadata. |
| AML.T0094 | Delay Execution of LLM Instructions | L1 [Isolated environment](control-reference.md#why-sessions-are-separated-and-how-far) split by input trust; L6 [context-sharing rules](control-reference.md#context-sharing); L3 [Action classifier](control-reference.md#l3-tool-control) | Partly (Lite) | Limits on untrusted input apply to a session for its whole life, so a delay does not escape them. |
| AML.T0061 | LLM Prompt Self-Replication | L6 [context-sharing rules](control-reference.md#context-sharing), [Content as repository references](control-reference.md#l6-message-controls), [Filtering and provenance on untrusted content](control-reference.md#l6-message-controls); L5 [Protected-path review](control-reference.md#l5-repository-controls) | Partly (Lite) | Spread across environments is limited from Standard, but spread inside one shared environment is a residual. |
| AML.T0130 | AI Agent Response Biasing | L7 [Plan and design review](control-reference.md#l7-process-controls), [Refute passes and blind judges](control-reference.md#l7-process-controls) | Related (Lite) | A biased recommendation, such as a library choice, gets plan review, but no control checks whether its sources are neutral. |
| AML.T0073 | Impersonation | L6 [Authority never relayed](control-reference.md#l6-message-controls), [Authenticated messaging](control-reference.md#l6-message-controls); L4 [Grants as the owner's answer to the tool's own permission prompt](control-reference.md#l4-permission-sets-and-delegation), [Owner-signed, instance-bound, time-bound grants](control-reference.md#l4-permission-sets-and-delegation) | Partly (Lite); Addresses (Standard) for authority claimed in conversation | Conversation text is never a grant, whoever it seems to come from, but an owner who is deceived is guarded only by attention. |

### Persistence in agent context, memory and configuration

| ID | Name | Method controls | Relation | Note |
|---|---|---|---|---|
| AML.T0080.000 | AI Agent Context Poisoning: Memory | L5 [Protected-path review](control-reference.md#l5-repository-controls); L3 [Enforcement material read-only to the session](control-reference.md#l3-tool-control); L8 [Ledger on its own ref in the repository](control-reference.md#l8-state-and-audit); L1 [ephemeral environments](control-reference.md#l1-infrastructure-isolation) | Partly (Lite) | Committed agent instruction files reach the owner as the diff, but memory a tool keeps outside the repository does not. |
| AML.T0080.001 | AI Agent Context Poisoning: Thread | L6 [context-sharing rules](control-reference.md#context-sharing); L4 [grant refusal rules](control-reference.md#grants-across-profiles); L3 [Enforcement material read-only to the session](control-reference.md#l3-tool-control) | Partly (Lite) | A poisoned thread can steer work in scope until the session ends, but cannot change its limits or its authority. |
| AML.T0081 | Modify AI Agent Configuration | L3 [Enforcement material read-only to the session](control-reference.md#l3-tool-control), [Agent tooling only from an owner-approved list](control-reference.md#l3-tool-control); L2 [No standing approvals](control-reference.md#l2-session-isolation); L5 [Protected-path review](control-reference.md#l5-repository-controls); L4 [Owner performs all outward actions](control-reference.md#l4-permission-sets-and-delegation) | Partly (Lite); Addresses (Standard) for the settings that enforce limits, for sessions not assigned environment authority | A session cannot change what enforces its own limits unless it holds environment authority or, at Lite, reaches a container runtime, and it can still change instruction files and user-level files that later sessions in its environment read before any review. |
| AML.T0092 | Manipulate User LLM Chat History | L8 [Ledger on its own ref in the repository](control-reference.md#l8-state-and-audit), [Message audit](control-reference.md#l8-state-and-audit), [Tamper-evident audit trail](control-reference.md#l8-state-and-audit) | Partly (Lite) | The method keeps its ledger and message audit, tamper-evident at High, but not the tool's own transcripts. |
| AML.T0070, AML.T0071 | RAG Poisoning; False RAG Entry Injection | [capability gates](method.md#7-the-capability-library) | Related (Standard) | The method does not name retrieval indexes given to coding agents as a source of untrusted input. |

### Tool invocation, execution and escape

| ID | Name | Method controls | Relation | Note |
|---|---|---|---|---|
| AML.T0053 | AI Agent Tool Invocation | L4 [Role permission sets](control-reference.md#l4-permission-sets-and-delegation), [Owner performs all outward actions](control-reference.md#l4-permission-sets-and-delegation); L3 [Deny push, publish, secret reads](control-reference.md#l3-tool-control), [Allow-list only](control-reference.md#l3-tool-control), [Action classifier](control-reference.md#l3-tool-control); L1 [Isolated environment](control-reference.md#l1-infrastructure-isolation) | Partly (Lite); Addresses (Standard) for tools outside the role, for sessions not assigned environment authority | Below High nothing judges whether a permitted call serves the task, and the High classifier catches some misuse, not all. |
| AML.T0050 | Command and Scripting Interpreter | L1 [Isolated environment](control-reference.md#l1-infrastructure-isolation); L3 [Pre-execution hooks](control-reference.md#l3-tool-control), [Wrapped commands](control-reference.md#l3-tool-control), [Action classifier](control-reference.md#l3-tool-control) | Partly (Lite) | Running commands is the agents' job, so the method bounds where they run and what they reach, not which ones run. |
| AML.T0101 | Data Destruction via AI Agent Tool Invocation | L1 [Isolated environment](control-reference.md#l1-infrastructure-isolation), [Destructive and adversarial tests only against stubbed copies or throwaway stores](control-reference.md#l1-infrastructure-isolation), [Throwaway data stores per session](control-reference.md#l1-infrastructure-isolation), [Lab separate from production](control-reference.md#l1-infrastructure-isolation); L5 [Owner-only merge and tag](control-reference.md#l5-repository-controls); L2 [Per-ref push enforcement](control-reference.md#l2-session-isolation) | Partly (Lite); Addresses (High), by containment | Below High, the push credential still reaches every branch on the internal remote, including the orchestrator's integration branches and its ledger; a session assigned environment authority can still destroy data in the lab targets its brief names, a residual at every profile. |
| AML.T0105 | Escape to Host | L1 [Isolated environment](control-reference.md#l1-infrastructure-isolation), [Environment changes through the owner](control-reference.md#l1-infrastructure-isolation); L3 [Enforcement material read-only to the session](control-reference.md#l3-tool-control) | Partly (Lite); Addresses (Standard) for escape through agent configuration, for sessions not assigned environment authority; Addresses (High) for runtime or kernel escape | From Standard, sessions not assigned environment authority get no root and cannot change the tool's configuration, but Standard allows containers on a machine the owner attends, so only High gives each session its own VM. |
| AML.T0112.000 | Machine Compromise: Local AI Agent | L1 [Isolated environment](control-reference.md#l1-infrastructure-isolation), [No host or production credentials mounted or forwarded](control-reference.md#l1-infrastructure-isolation); L4 [Owner performs all outward actions](control-reference.md#l4-permission-sets-and-delegation); L2 [Per-ref push enforcement](control-reference.md#l2-session-isolation) | Partly (Lite); Addresses (High), by containment | A misled agent compromises its own environment, not the owner's workstation or production, but at Lite that environment has no egress control and holds every session, and below High its push credential reaches every ref. |
| AML.T0011.003 | User Execution: Malicious Link | L1 [Isolated environment](control-reference.md#l1-infrastructure-isolation), [Segmented network](control-reference.md#l1-infrastructure-isolation); L4 [the owner's own checkout](control-reference.md#where-the-owner-acts) | Partly (Lite) | A link opened in the owner's browser can reach listeners in agent environments on the same machine, such as local tool servers, and no rule requires them to authenticate or check the origin. |
| AML.T0103 | Deploy AI Agent | L1 [Isolated environment](control-reference.md#l1-infrastructure-isolation), [Per-environment, least-scope credentials](control-reference.md#l1-infrastructure-isolation); [What the tool must provide](control-reference.md#what-the-tool-must-provide) | Partly (Lite) | Agent processes a session starts get no more reach than its environment, but they appear in no launch record. |
| AML.T0034.002 | Cost Harvesting: Agentic Resource Consumption | L1 [credential scope](control-reference.md#credential-scope) (spend cap); [capability budgets](method.md#7-the-capability-library) | Partly (Standard) | A spend cap bounds the cost per environment, but nothing detects a runaway loop in an ordinary session. |

### Credential access and use

| ID | Name | Method controls | Relation | Note |
|---|---|---|---|---|
| AML.T0083 | Credentials from AI Agent Configuration | L1 [No host or production credentials mounted or forwarded](control-reference.md#l1-infrastructure-isolation), [Per-environment, least-scope credentials](control-reference.md#l1-infrastructure-isolation); L2 [Per-ref push enforcement](control-reference.md#l2-session-isolation); L3 [Deny push, publish, secret reads](control-reference.md#l3-tool-control), [hook on credential reads](control-reference.md#environment-rules-worth-a-hook) | Partly (Lite); Addresses (High), by containment, for the credentials the method issues; Partly (Lite) for tool-server credentials | The method treats every credential it issues as readable and limits its reach and lifetime, but credentials kept in tool-server configuration for other services are not among them; a model key read there still works from anywhere until teardown, within its spend cap. |
| AML.T0055 | Unsecured Credentials | L1 [No host or production credentials mounted or forwarded](control-reference.md#l1-infrastructure-isolation); L3 [Deny push, publish, secret reads](control-reference.md#l3-tool-control); L5 [Required CI, enforced by the host](control-reference.md#l5-repository-controls) (secret scan); L8 [ledger hygiene](control-reference.md#ledger-hygiene) | Partly (Lite) | The secret scan finds committed secrets before merge but after they are pushed, ledger excerpts are scanned before commit, and a credential inside an environment is readable by its session. |
| AML.T0098 | AI Agent Tool Credential Harvesting | L4 [Role permission sets](control-reference.md#l4-permission-sets-and-delegation); L1 [Fetch tools denied outside ingestion environments](control-reference.md#l1-infrastructure-isolation), [Per-environment, least-scope credentials](control-reference.md#l1-infrastructure-isolation); L5 [Required CI, enforced by the host](control-reference.md#l5-repository-controls) (secret scan) | Partly (Lite) | Roles limit which tools and sources a session reaches, but credentials already in those sources are caught only by the scan of new commits. |
| AML.T0090 | OS Credential Dumping | L1 [Isolated environment](control-reference.md#l1-infrastructure-isolation), [No host or production credentials mounted or forwarded](control-reference.md#l1-infrastructure-isolation), [Per-environment, least-scope credentials](control-reference.md#l1-infrastructure-isolation); L2 [Per-ref push enforcement](control-reference.md#l2-session-isolation) | Partly (Lite); Addresses (High), by containment | Dumping is not prevented and what can be dumped is scoped to the environment, but below High the push credential it yields reaches every ref on the internal remote. |
| AML.T0091.000 | Use Alternate Authentication Material: Application Access Token | L1 [Per-environment, least-scope credentials](control-reference.md#l1-infrastructure-isolation); L5 [Owner-only merge and tag](control-reference.md#l5-repository-controls); L2 [Per-ref push enforcement](control-reference.md#l2-session-isolation); L4 [Owner-signed, instance-bound, time-bound grants](control-reference.md#l4-permission-sets-and-delegation) | Partly (Lite); Addresses (High) for push credentials and grants | Agent tokens are revoked at teardown and cannot merge or tag, but below High the push credential reaches every ref, and a stolen model key works from anywhere until teardown, within its spend cap. |
| AML.T0012 | Valid Accounts | L1 [Per-environment, least-scope credentials](control-reference.md#l1-infrastructure-isolation); L2 [Own identity per environment](control-reference.md#l2-session-isolation); L5 [Owner-only merge and tag](control-reference.md#l5-repository-controls); L4 [Owner performs all outward actions](control-reference.md#l4-permission-sets-and-delegation) | Partly (Lite) | This covers the accounts the method issues to agents; a compromised owner account is outside its model, apart from key rotation at High. |

### Collection, exfiltration and command-and-control

| ID | Name | Method controls | Relation | Note |
|---|---|---|---|---|
| AML.T0086 | Exfiltration via AI Agent Tool Invocation | L1 [Egress allow-list](control-reference.md#l1-infrastructure-isolation); L3 [Deny push, publish, secret reads](control-reference.md#l3-tool-control), [Action classifier](control-reference.md#l3-tool-control); L4 [Owner performs all outward actions](control-reference.md#l4-permission-sets-and-delegation); L5 [Boundary scans](control-reference.md#l5-repository-controls) | Partly (Lite) | From Standard the allow-list closes destinations off the list, but allowed hosts, such as the model provider's API reached with an attacker's own key, remain channels. |
| AML.T0025, AML.T0072 | Exfiltration via Cyber Means; Cyber Communication Channel | L1 [Egress allow-list](control-reference.md#l1-infrastructure-isolation), [Segmented network](control-reference.md#l1-infrastructure-isolation) | Partly (Standard) | The allow-list closes hosts off the list, but allowed shared hosts, such as the model API, registries and the code host, stay usable as channels, and Lite has no egress control. |
| AML.T0077 | LLM Response Rendering | L1 [Egress allow-list](control-reference.md#l1-infrastructure-isolation); L5 [Boundary scans](control-reference.md#l5-repository-controls) | Related (Standard) | Agent-written markdown is rendered by the host or the owner's tools, outside every agent environment, where egress control does not apply. |
| AML.T0057 | LLM Data Leakage | L3 [Deny push, publish, secret reads](control-reference.md#l3-tool-control); L8 [ledger hygiene](control-reference.md#ledger-hygiene); L5 [Boundary scans](control-reference.md#l5-repository-controls), [Required CI, enforced by the host](control-reference.md#l5-repository-controls) (secret scan) | Partly (Lite) | Data sent to the model provider is outside the method's scope. |
| AML.T0037 | Data from Local System | L1 [Isolated environment](control-reference.md#l1-infrastructure-isolation), [No host or production credentials mounted or forwarded](control-reference.md#l1-infrastructure-isolation), [Throwaway data stores per session](control-reference.md#l1-infrastructure-isolation), [Lab separate from production](control-reference.md#l1-infrastructure-isolation) | Partly (Lite) | "Local" here is the agent environment, and which data agents may read is the adopter's decision. |
| AML.T0085.001 | Data from AI Services: AI Agent Tools | L4 [Role permission sets](control-reference.md#l4-permission-sets-and-delegation); [read-only specialists](method.md#6-the-specialist-bench); L1 [Fetch tools denied outside ingestion environments](control-reference.md#l1-infrastructure-isolation) | Partly (Lite) | Each role reaches only the tools, and so the data, its permission set allows. |
| AML.T0108 | AI Agent | L1 [Egress allow-list](control-reference.md#l1-infrastructure-isolation), [Fetch tools denied outside ingestion environments](control-reference.md#l1-infrastructure-isolation); L5 [Required CI, enforced by the host](control-reference.md#l5-repository-controls); L8 [Claims only from output](control-reference.md#l8-state-and-audit), [Message audit](control-reference.md#l8-state-and-audit) | Partly (Standard) | Records taken from outside the agent, such as CI conclusions and the transport's audit, survive an agent told to hide its actions, but the ledger does not, and allowed hosts can still carry commands. |
| AML.T0096 | AI Service API | L1 [Fetch tools denied outside ingestion environments](control-reference.md#l1-infrastructure-isolation), [Egress allow-list](control-reference.md#l1-infrastructure-isolation) | Partly (Standard) | The model API is an egress path every session keeps, and an allow-list by host does not bind the account used there. |
| AML.T0120 | AI Artifact Repository | L1 [Egress allow-list](control-reference.md#l1-infrastructure-isolation); L2 [Internal git remote as the only shared repository surface](control-reference.md#l2-session-isolation) | Partly (Standard) | Allowed registries and the host can still serve as drop points, and only the High action classifier looks at how they are used. |

### Agent tooling and software supply chain

| ID | Name | Method controls | Relation | Note |
|---|---|---|---|---|
| AML.T0010.005, AML.T0011.002 | AI Supply Chain Compromise: AI Agent Tool; User Execution: Poisoned AI Agent Tool | L3 [Agent tooling only from an owner-approved list](control-reference.md#l3-tool-control); L5 [Protected-path review](control-reference.md#l5-repository-controls); L4 [Role permission sets](control-reference.md#l4-permission-sets-and-delegation); L1 [Egress allow-list](control-reference.md#l1-infrastructure-isolation) | Partly (Lite); Addresses (Standard) for locally installed tooling that is unapproved or silently changed | The method governs which tools arrive and when they change, not how an approval inspects them, and a remotely hosted tool server can change after approval; in a shared worker environment a peer session can still poison the package cache later installs read, a Standard residual. |
| AML.T0110.000 | AI Agent Tool Poisoning: Definition and Instructions | L3 [Agent tooling only from an owner-approved list](control-reference.md#l3-tool-control), [Action classifier](control-reference.md#l3-tool-control); L5 [Protected-path review](control-reference.md#l5-repository-controls) | Partly (Standard) | No control inspects tool definitions or skill instructions for embedded instructions. |
| AML.T0110.001 | AI Agent Tool Poisoning: Implementation | L3 [Agent tooling only from an owner-approved list](control-reference.md#l3-tool-control) (pinned, auto-update off); L1 [Isolated environment](control-reference.md#l1-infrastructure-isolation), [Egress allow-list](control-reference.md#l1-infrastructure-isolation) | Partly (Standard) | Tool servers and hooks often run outside the shell sandbox, so containment rests on the environment. |
| AML.T0110.002 | AI Agent Tool Poisoning: Runtime Response | L3 [Action classifier](control-reference.md#l3-tool-control) | Partly (High) | Tool output is untrusted text that the method records as a residual at every profile. |
| AML.T0109 | AI Supply Chain Rug Pull | L3 [Agent tooling only from an owner-approved list](control-reference.md#l3-tool-control) (pinned, auto-update off); L1 [frozen-lockfile installs](control-reference.md#why-sessions-are-separated-and-how-far) | Addresses (Standard) for locally installed agent tooling; Partly (Standard) for remotely hosted tool servers and product dependencies | A malicious update still lands if a reviewer approves a version bump without reading what changed, and a remote tool server cannot be pinned. |
| AML.T0111 | AI Supply Chain Reputation Inflation | L3 [Agent tooling only from an owner-approved list](control-reference.md#l3-tool-control) | Related (Lite) | The method gives no approval criteria, and reputation is what an approver would otherwise rely on. |
| AML.T0115.002 | Publish Poisoned AI Artifacts: AI Agent Tools | L3 [Agent tooling only from an owner-approved list](control-reference.md#l3-tool-control) | Related (Lite) | This is the attacker's preparation; the method's control point is adoption. |
| AML.T0010.001, AML.T0011.001 | AI Supply Chain Compromise: AI Software; User Execution: Malicious Package | L3 [Agent tooling only from an owner-approved list](control-reference.md#l3-tool-control); L1 [Egress allow-list](control-reference.md#l1-infrastructure-isolation), [Isolated environment](control-reference.md#l1-infrastructure-isolation); L5 [CI kept out of band](control-reference.md#l5-repository-controls), [Reproducible builds](control-reference.md#l5-repository-controls); L7 [Plan and design review](control-reference.md#l7-process-controls) | Addresses (Standard) for locally installed agent tooling that is unapproved or silently changed; Partly (Lite) for product dependencies | Install scripts run with the session's authority inside its environment, and vetting product dependencies is left to the adopter. |
| AML.T0060 | Publish Hallucinated Entities | L7 [Plan and design review](control-reference.md#l7-process-controls), [Deliverable review on the final diff](control-reference.md#l7-process-controls); L1 [Egress allow-list](control-reference.md#l1-infrastructure-isolation) | Partly (Lite) | A new dependency is a reviewed plan decision, but no step checks that the package exists and is the intended one. |
| AML.T0010.003 | AI Supply Chain Compromise: Model | L3 [Agent tooling only from an owner-approved list](control-reference.md#l3-tool-control) (models pinned); L6 [Decision classifier](control-reference.md#l6-message-controls) (weights pinned by hash) | Partly (Standard); Addresses (High) for the self-hosted classifier | The adopter can pin the choice of a hosted model but cannot check its weights. |
| AML.T0010.004 | AI Supply Chain Compromise: Container Registry | L5 [Protected-path review](control-reference.md#l5-repository-controls) (environment definitions); L1 [ephemeral environments](control-reference.md#l1-infrastructure-isolation) | Partly (Standard) | Images named in definitions are not required to be pinned by digest, so an overwritten tag passes review. |
| AML.T0074 | Masquerading | L3 [Agent tooling only from an owner-approved list](control-reference.md#l3-tool-control) (pinned) | Partly (Standard) | Version pins do not cover a reclaimed name for a model, package or tool server. |
| AML.T0119 | Exploit Automated Artifact Processing Pipeline | L5 [CI kept out of band](control-reference.md#l5-repository-controls), [Protected-path review](control-reference.md#l5-repository-controls) | Partly (Lite); Addresses (Standard) for secrets and write tokens in agent-triggered workflows, where code-owner rules enforce the protected-path review | Workflows triggered by agent changes get no secrets or write tokens, but changes to what CI runs reach the owner by expectation unless code-owner rules enforce it, and whether build caches cross into release builds is not stated. |

### Agent autonomy and communication between agents

| ID | Name | Method controls | Relation | Note |
|---|---|---|---|---|
| AML.T0117 | Autonomous Attack-Path Adaptation | L3 [Enforcement material read-only to the session](control-reference.md#l3-tool-control), [Action classifier](control-reference.md#l3-tool-control); L1 [Environment changes through the owner](control-reference.md#l1-infrastructure-isolation), [Per-environment, least-scope credentials](control-reference.md#l1-infrastructure-isolation), [Egress allow-list](control-reference.md#l1-infrastructure-isolation); L4 [refusals are never worked around](control-reference.md#permission-sets-must-express-the-delegation) | Partly (Lite) | An agent's attempt to widen its own reach meets settings and boundaries, but drift inside permitted calls is checked only at High. |
| AML.T0118.000 | Autonomous AI Agent Communication: Communication via Shared Artifacts | L2 [Internal git remote as the only shared repository surface](control-reference.md#l2-session-isolation), [Per-ref push enforcement](control-reference.md#l2-session-isolation); L5 [Protected-path review](control-reference.md#l5-repository-controls); L8 [Ledger on its own ref in the repository](control-reference.md#l8-state-and-audit) | Related (Lite); Partly (High) | The method's agents coordinate through committed files, and below High a compromised session can push to every ref on the internal remote. |
| AML.T0118.001 | Autonomous AI Agent Communication: Direct Agent Communication | L6 [Authority never relayed](control-reference.md#l6-message-controls), [Content as repository references](control-reference.md#l6-message-controls), [Authenticated messaging](control-reference.md#l6-message-controls), [Private bus](control-reference.md#l6-message-controls); L8 [Message audit](control-reference.md#l8-state-and-audit) | Related (Lite) | The method's own channel between agents is authenticated, audited and unable to carry authority. |
| AML.T0121 | AI Agent Environment Reconstruction | L1 [Environment changes through the owner](control-reference.md#l1-infrastructure-isolation), [ephemeral environments](control-reference.md#l1-infrastructure-isolation); L4 [refusals are never worked around](control-reference.md#permission-sets-must-express-the-delegation) | Partly (Lite); Addresses (Standard) for credentials and grants | A session cannot restore its own credentials or grants, but below High it can reinstall user-level tools, and restarts rebuild working state from the ledger by design. |

### Discovery and reconnaissance

| ID | Name | Method controls | Relation | Note |
|---|---|---|---|---|
| AML.T0084 and its sub-techniques, AML.T0133 | Discover AI Agent Configuration; Discover AI Agent Runtime Capabilities | L3 [Enforcement material read-only to the session](control-reference.md#l3-tool-control); L1 [Isolated environment](control-reference.md#l1-infrastructure-isolation) | Related (Lite) | The method relies on settings and boundaries, not secrecy, so knowing the limits gives no extra reach where those keep them. |
| AML.T0002.002 | Acquire Public AI Artifacts: AI Agent Configuration | L5 [Boundary scans](control-reference.md#l5-repository-controls) | Related (Lite) | With a public mirror, committed agent instructions and settings may be published, and the scans look for private information, not for these. |
| AML.T0095.000 | Search Open Websites/Domains: Code Repositories | L5 [Required CI, enforced by the host](control-reference.md#l5-repository-controls) (secret scan), [Boundary scans](control-reference.md#l5-repository-controls) | Partly (Lite) | Secret and boundary scans limit what a public repository gives away. |
| AML.T0132 | Misconfigured or Publicly Exposed AI Services | L6 [Authenticated messaging](control-reference.md#l6-message-controls), [Private bus](control-reference.md#l6-message-controls); L1 [Segmented network](control-reference.md#l1-infrastructure-isolation) | Partly (Standard) | This covers the method's own channels, not listeners started inside agent environments, such as local tool servers. |

### Model-level evasion

| ID | Name | Method controls | Relation | Note |
|---|---|---|---|---|
| AML.T0054 | LLM Jailbreak | L1 [Isolated environment](control-reference.md#l1-infrastructure-isolation); L3 [Enforcement material read-only to the session](control-reference.md#l3-tool-control), [Deny push, publish, secret reads](control-reference.md#l3-tool-control); L4 [Owner performs all outward actions](control-reference.md#l4-permission-sets-and-delegation) | Partly (Lite) | No control targets the jailbreak itself, but a jailbroken agent meets the same settings and boundaries. |
| AML.T0123 | Obfuscated Files or Information | L5 [Required CI, enforced by the host](control-reference.md#l5-repository-controls) (secret scan), [Boundary scans](control-reference.md#l5-repository-controls); L7 [Deliverable review on the final diff](control-reference.md#l7-process-controls); L6 [Decision classifier](control-reference.md#l6-message-controls) | Partly (Lite) | Encoded content in agent-written code can pass agent review and scanners, and a human reads only protected paths and, at High, high-risk slices. |

### Outside the method's scope

The method governs agent teams that build software. It trains, hosts and serves no models, and
agents inside the product are out of its scope ([method.md](method.md), §0). These technique groups
are therefore Outside. Names for the IDs are in ATLAS release 2026.09 at
[atlas.mitre.org](https://atlas.mitre.org).

- **Model artefacts, training and model integrity:** AML.T0020, AML.T0018 and its sub-techniques,
  AML.T0031, AML.T0059, AML.T0076, AML.T0010.000, AML.T0010.002, AML.T0011.000, AML.T0112.001,
  AML.T0115.000, AML.T0115.001, AML.T0043.004.
- **Attacks on a deployed model's decisions:** AML.T0040, AML.T0044, AML.T0047, AML.T0015,
  AML.T0043 and AML.T0043.000 to AML.T0043.003, AML.T0041, AML.T0042, AML.T0005 and its
  sub-techniques, AML.T0046, AML.T0013, AML.T0014, AML.T0063.
- **Model and IP theft through inference:** AML.T0024 and its sub-techniques, AML.T0035,
  AML.T0048.004, AML.T0056.
- **Adversary reconnaissance and resource development:** AML.T0000 and its sub-techniques,
  AML.T0001, AML.T0002.000, AML.T0002.001, AML.T0003, AML.T0004, AML.T0006 and its sub-techniques,
  AML.T0007, AML.T0008 and its sub-techniques, AML.T0016 and its sub-techniques, AML.T0017,
  AML.T0017.000, AML.T0017.002, AML.T0021, AML.T0062, AML.T0064, AML.T0065, AML.T0066, AML.T0079,
  AML.T0087, AML.T0128. The method's control point is where the result is consumed, mapped above.
- **Autonomous offence run by an adversary:** AML.T0116, AML.T0124, AML.T0017.001. Benign agents
  crossing a boundary are mapped under AML.T0117.
- **AI in the product and consumer assistants:** AML.T0029, AML.T0034.000, AML.T0034.001,
  AML.T0048.000 to AML.T0048.003, AML.T0049, AML.T0067 and its sub-technique, AML.T0069 and its
  sub-techniques, AML.T0082, AML.T0085.000, AML.T0052 and its sub-techniques, AML.T0088, AML.T0131,
  AML.T0113. They apply to the product if the product is an AI system.
- **Host-compromise techniques adopted from ATT&CK:** AML.T0036, AML.T0075, AML.T0089, AML.T0097,
  AML.T0102, AML.T0106, AML.T0107, AML.T0114, AML.T0122, AML.T0125, AML.T0126, AML.T0127,
  AML.T0091.001. Inside an agent environment they meet only L1 containment and least-scope
  credentials, as AML.T0037 and AML.T0050 do.

## OWASP Top 10 for Agentic Applications

The OWASP Top 10 for Agentic Applications is the OWASP GenAI Security Project's list of the ten most
critical risks to agentic AI applications. This crosswalk uses the 2026 edition, published on 9
December 2025 and read on 28 October 2026 at
[genai.owasp.org](https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/).

*OWASP Top 10 for Agentic Applications 2026, by the OWASP GenAI Security Project, licensed under
[CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/). This crosswalk uses item IDs and
names only, copies no OWASP text, and is not endorsed by OWASP.*

The list concerns agentic applications. Here the agentic system is the team that builds the
product; an adopter whose product contains agents applies the list to the product separately. Names
follow the list's at-a-glance page. For the detailed threats behind each item (the T-codes), see
OWASP's
[Agentic AI – Threats and Mitigations](https://genai.owasp.org/resource/agentic-ai-threats-and-mitigations/);
the Top 10 maps each item to version 1.1 of it in its Appendix A.

| ID | Name | Method controls | Relation | Note |
|---|---|---|---|---|
| ASI01 | Agent Goal Hijack | L6 [Authority never relayed](control-reference.md#l6-message-controls), [Filtering and provenance on untrusted content](control-reference.md#l6-message-controls), [Decision classifier](control-reference.md#l6-message-controls); L4 [Owner performs all outward actions](control-reference.md#l4-permission-sets-and-delegation); L1 [Fetch tools denied outside ingestion environments](control-reference.md#l1-infrastructure-isolation), [Orchestrator environment runs no product code and no fetches](control-reference.md#l1-infrastructure-isolation), [Egress allow-list](control-reference.md#l1-infrastructure-isolation); L5 [Protected-path review](control-reference.md#l5-repository-controls); L3 [Action classifier](control-reference.md#l3-tool-control) | Partly (Lite) | A hijacked session cannot widen its authority or act outward, but untrusted text in dependencies and tool output reaches every session at every profile. |
| ASI02 | Tool Misuse & Exploitation | L3 [Deny push, publish, secret reads](control-reference.md#l3-tool-control), [Pre-execution hooks](control-reference.md#l3-tool-control), [Wrapped commands](control-reference.md#l3-tool-control), [Allow-list only](control-reference.md#l3-tool-control), [Action classifier](control-reference.md#l3-tool-control); L4 [Role permission sets](control-reference.md#l4-permission-sets-and-delegation); L1 [Isolated environment](control-reference.md#l1-infrastructure-isolation), [Throwaway data stores per session](control-reference.md#l1-infrastructure-isolation), [Egress allow-list](control-reference.md#l1-infrastructure-isolation) | Partly (Lite) | Rules remove whole tools from a role, but only the High action classifier judges how a permitted tool is used, and it catches some misuse, not all. |
| ASI03 | Identity & Privilege Abuse | L6 [Authority never relayed](control-reference.md#l6-message-controls); L3 [Enforcement material read-only to the session](control-reference.md#l3-tool-control); L1 [No host or production credentials mounted or forwarded](control-reference.md#l1-infrastructure-isolation), [Per-environment, least-scope credentials](control-reference.md#l1-infrastructure-isolation); L2 [No standing approvals](control-reference.md#l2-session-isolation), [Own identity per environment](control-reference.md#l2-session-isolation), [Per-ref push enforcement](control-reference.md#l2-session-isolation); L4 [Role permission sets](control-reference.md#l4-permission-sets-and-delegation), [Grants as the owner's answer to the tool's own permission prompt](control-reference.md#l4-permission-sets-and-delegation), [Owner-signed, instance-bound, time-bound grants](control-reference.md#l4-permission-sets-and-delegation) | Partly (Lite); Addresses (High) | No message can confer authority and no session without environment authority can widen its own limits, but at Lite all sessions share one identity, and below High workers that share an environment share its identity and the push credential reaches every ref. |
| ASI04 | Agentic Supply Chain Vulnerabilities | L3 [Agent tooling only from an owner-approved list](control-reference.md#l3-tool-control); L5 [Protected-path review](control-reference.md#l5-repository-controls); L1 [Egress allow-list](control-reference.md#l1-infrastructure-isolation), [No host or production credentials mounted or forwarded](control-reference.md#l1-infrastructure-isolation) | Partly (Lite); Addresses (Standard) for locally installed agent tooling that is unapproved or silently changed | Agent tooling is approved, pinned and changed only through review, but no inspection criteria, signatures or software bill of materials (SBOM) are required, and remote tool servers cannot be pinned. |
| ASI05 | Unexpected Code Execution (RCE) | L1 [Isolated environment](control-reference.md#l1-infrastructure-isolation), [No host or production credentials mounted or forwarded](control-reference.md#l1-infrastructure-isolation), [Environment changes through the owner](control-reference.md#l1-infrastructure-isolation), [Orchestrator environment runs no product code and no fetches](control-reference.md#l1-infrastructure-isolation), [Lab separate from production](control-reference.md#l1-infrastructure-isolation); L2 [Per-ref push enforcement](control-reference.md#l2-session-isolation); L4 [Owner performs all outward actions](control-reference.md#l4-permission-sets-and-delegation); L5 [CI kept out of band](control-reference.md#l5-repository-controls) | Partly (Lite); Addresses (High), by containment | Running code is the agents' job, so the method contains execution rather than preventing it, but at Lite there is no egress control and below High the push credential reaches every ref. |
| ASI06 | Memory & Context Poisoning | L8 [Ledger on its own ref in the repository](control-reference.md#l8-state-and-audit); L6 [context-sharing rules](control-reference.md#context-sharing); L5 [Protected-path review](control-reference.md#l5-repository-controls); L4 [grant refusal rules](control-reference.md#grants-across-profiles); [capability gates](method.md#7-the-capability-library) | Partly (Lite) | Limits and authority are never taken from memory or summaries, but memory a tool keeps outside the repository is not covered. |
| ASI07 | Insecure Inter-Agent Communication | L6 [Authority never relayed](control-reference.md#l6-message-controls), [Local messaging with senders set by the tool or the launch configuration](control-reference.md#l6-message-controls), [Authenticated messaging](control-reference.md#l6-message-controls), [Content as repository references](control-reference.md#l6-message-controls), [Private bus](control-reference.md#l6-message-controls); L4 [Signed kickoff manifest](control-reference.md#l4-permission-sets-and-delegation); L8 [Message audit](control-reference.md#l8-state-and-audit) | Partly (Lite); Addresses (High) for spoofing and eavesdropping | A message never carries authority, but replay protection for ordinary messages exists only in an optional mode at High. |
| ASI08 | Cascading Failures | L5 [Required CI, enforced by the host](control-reference.md#l5-repository-controls); L4 [Owner performs all outward actions](control-reference.md#l4-permission-sets-and-delegation); L7 [Capability pilot → guard → scale](control-reference.md#l7-process-controls), [Checkpoint review](control-reference.md#l7-process-controls); L1 [Isolated environment](control-reference.md#why-sessions-are-separated-and-how-far) split by authority; L8 [Minimal metrics](control-reference.md#l8-state-and-audit) | Partly (Lite) | CI, review and the owner's merge stop faults before main, but nothing pauses work automatically when behaviour turns anomalous. |
| ASI09 | Human-Agent Trust Exploitation | L5 [Required CI, enforced by the host](control-reference.md#l5-repository-controls), [Protected-path review](control-reference.md#l5-repository-controls); L8 [Claims only from output](control-reference.md#l8-state-and-audit); L7 [Human reading of high-risk slices](control-reference.md#l7-process-controls), [Refute passes and blind judges](control-reference.md#l7-process-controls); L4 [Two-party rule for releases](control-reference.md#l4-permission-sets-and-delegation) | Partly (Lite) | Merges rest on the host's CI result rather than an agent's report, but below High the owner merges product code on a decision the orchestrator prepares, not on reading the diff. |
| ASI10 | Rogue Agents | L4 [Owner performs all outward actions](control-reference.md#l4-permission-sets-and-delegation), [Two-party rule for releases](control-reference.md#l4-permission-sets-and-delegation); L3 [Enforcement material read-only to the session](control-reference.md#l3-tool-control), [Action classifier](control-reference.md#l3-tool-control); L5 [CI kept out of band](control-reference.md#l5-repository-controls); L7 [Property and mutation tests](control-reference.md#l7-process-controls); L8 [Tamper-evident audit trail](control-reference.md#l8-state-and-audit); [suspected compromise](practice.md#suspected-compromise) | Partly (Lite) | A rogue agent cannot act outward, widen its limits, or change CI or fake its result, but there is no behavioural baseline or automatic stop. |

## NIST SSDF

The Secure Software Development Framework, NIST SP 800-218, sets out practices for producing
software with fewer vulnerabilities across the development life cycle. This crosswalk uses version
1.1, final, February 2022, read on 28 October 2026 at
[csrc.nist.gov](https://csrc.nist.gov/pubs/sp/800/218/final).

*SP 800-218 is a work of the US government and not subject to copyright in the United States.
Practice IDs and names are from NIST SP 800-218,
[doi:10.6028/NIST.SP.800-218](https://doi.org/10.6028/NIST.SP.800-218), courtesy of the National
Institute of Standards and Technology.*

The SSDF concerns the software produced and the whole development process; the method concerns how
agents produce it. Most rows are therefore Partly: the method serves the agent part of a practice,
and the product-security part stays with the adopter. Do not count the relations into a coverage
figure, and do not use this table to support a self-attestation that rests on SP 800-218, such as
the CISA Secure Software Development Attestation Form. Rows are at practice level (PO.1); a note
names a task (PO.1.1) only where it changes the relation. SSDF 1.1 has no PW.3.

| ID | Name | Method controls | Relation | Note |
|---|---|---|---|---|
| PO.1 | Define Security Requirements for Software Development | [Profiles per layer](method.md#12-assurance-profiles); [control-to-profile map](control-reference.md#control-to-profile-map); [documentation chain](practice.md#1-documentation-chain) | Partly (Lite) | The chosen profile per layer is a requirement set for how agents develop, not a set of security requirements for the software. |
| PO.2 | Implement Roles and Responsibilities | [Roles](method.md#2-topology); L4 [Role permission sets](control-reference.md#l4-permission-sets-and-delegation), [Delegation manifest](control-reference.md#l4-permission-sets-and-delegation) | Partly (Lite) | Agent and owner roles and their authority are defined, and enforced from Standard; human security roles, training and management commitment are left to the adopter. |
| PO.3 | Implement Supporting Toolchains | L3 [Agent tooling only from an owner-approved list](control-reference.md#l3-tool-control), [Enforcement material read-only to the session](control-reference.md#l3-tool-control), [Enforcing checks fail closed](control-reference.md#l3-tool-control); L5 [Required CI, enforced by the host](control-reference.md#l5-repository-controls), [CI kept out of band](control-reference.md#l5-repository-controls) | Partly (Lite) | The agent toolchain and CI are controlled and produce evidence, but the method does not say which security tools a toolchain must include. |
| PO.4 | Define and Use Criteria for Software Security Checks | L7 [Pre-PR gate](control-reference.md#l7-process-controls), [rule that gates must be able to fail](control-reference.md#gates-must-be-able-to-fail); L5 [Required CI, enforced by the host](control-reference.md#l5-repository-controls); L8 [Ledger on its own ref in the repository](control-reference.md#l8-state-and-audit), [Claims only from output](control-reference.md#l8-state-and-audit) | Partly (Lite) | The method fixes where checks run and how their results are kept, not the criteria for acceptable security risk. |
| PO.5 | Implement and Maintain Secure Environments for Software Development | L1 [Isolated environment](control-reference.md#why-sessions-are-separated-and-how-far) split by authority and input trust, [No host or production credentials mounted or forwarded](control-reference.md#l1-infrastructure-isolation), [Egress allow-list](control-reference.md#l1-infrastructure-isolation), [Lab separate from production](control-reference.md#l1-infrastructure-isolation), [Per-environment, least-scope credentials](control-reference.md#l1-infrastructure-isolation), [Segmented network](control-reference.md#l1-infrastructure-isolation); L2 [Own identity per environment](control-reference.md#l2-session-isolation), [Per-ref push enforcement](control-reference.md#l2-session-isolation); L4 [the owner's own checkout](control-reference.md#where-the-owner-acts) | Partly (Lite); Addresses (High) for separating and protecting agent environments | This is the closest fit, for agent environments only, but monitoring is limited to the egress deny log until High, and the owner's workstation and human developers' endpoints are outside. |
| PS.1 | Protect All Forms of Code from Unauthorized Access and Tampering | L5 [Owner-only merge and tag](control-reference.md#l5-repository-controls), [No commits on main; PRs](control-reference.md#l5-repository-controls), [Protected-path review](control-reference.md#l5-repository-controls), [Signed commits and tags](control-reference.md#l5-repository-controls); L2 [Internal git remote as the only shared repository surface](control-reference.md#l2-session-isolation), [Per-ref push enforcement](control-reference.md#l2-session-isolation); L3 [Deny push, publish, secret reads](control-reference.md#l3-tool-control) | Addresses (Lite) for main and tags; Partly (Lite), Addresses (High) for other refs; Outside for confidentiality | Host rules keep main and tags with the owner where the host plan allows, but below High a session can push to any ref on the internal remote, commit signing is High only, and every file a session reads goes to the model provider. |
| PS.2 | Provide a Mechanism for Verifying Software Release Integrity | [Release checklist](practice.md#release-vehicles) (digests); L5 [Reproducible builds](control-reference.md#l5-repository-controls); L7 [Rehearsed, reproduce-gated release kit](control-reference.md#l7-process-controls); L4 [Two-party rule for releases](control-reference.md#l4-permission-sets-and-delegation) | Partly (Lite) | Digests are recorded at every profile and come from an independent build at High, but publishing integrity information to acquirers is not required. |
| PS.3 | Archive and Protect Each Software Release | [Release checklist](practice.md#release-vehicles) and run sheet; L7 [Rehearsed, reproduce-gated release kit](control-reference.md#l7-process-controls); L8 [Ledger on its own ref in the repository](control-reference.md#l8-state-and-audit) | Partly (Lite) | The run sheet and ledger keep each release's steps, gates and digests, but keeping the artefacts and their component provenance is not required. |
| PW.1 | Design Software to Meet Security Requirements and Mitigate Security Risks | [Documentation chain](practice.md#1-documentation-chain); [specialist lenses](method.md#6-the-specialist-bench); L8 [Decision register](control-reference.md#l8-state-and-audit); L7 [Acceptance-criteria trace](control-reference.md#l7-process-controls) | Partly (Lite) | Design decisions are tracked, but a threat model of the product is available as a lens, not required. |
| PW.2 | Review the Software Design to Verify Compliance with Security Requirements and Risk Information | L7 [Plan and design review](control-reference.md#l7-process-controls), [Multi-lens review](control-reference.md#l7-process-controls), [External reviewer](control-reference.md#l7-process-controls) | Related (Lite); Partly (Standard) | Designs are reviewed by agent reviewers before code is written, each without the author's context, but the review is a security review only from Standard. |
| PW.4 | Reuse Existing, Well-Secured Software When Feasible Instead of Duplicating Functionality | [Sustain mode](practice.md#8-sustain-mode); L1 [new dependency as a plan decision](control-reference.md#why-sessions-are-separated-and-how-far); L3 [Agent tooling only from an owner-approved list](control-reference.md#l3-tool-control) | Partly (Lite) | Third-party components are kept current and their advisories triaged once a product is released, but criteria for choosing well-secured components are not stated. |
| PW.5 | Create Source Code by Adhering to Secure Coding Practices | L5 [Protected-path review](control-reference.md#l5-repository-controls) (agent instruction files) | Related (Lite) | The method names no secure coding standard, and one the adopter puts in agent instruction files reaches the owner as the diff when changed, by expectation at Lite and by code-owner rule from Standard. |
| PW.6 | Configure the Compilation, Interpreter, and Build Processes to Improve Executable Security | L5 [Protected-path review](control-reference.md#l5-repository-controls) | Related (Lite) | The method chooses no build hardening, and a change to build configuration reaches the owner as the diff only if the adopter lists it as a protected path. |
| PW.7 | Review and/or Analyze Human-Readable Code to Identify Vulnerabilities and Verify Compliance with Security Requirements | L7 [Pre-PR gate](control-reference.md#l7-process-controls), [Deliverable review on the final diff](control-reference.md#l7-process-controls), [Human reading of high-risk slices](control-reference.md#l7-process-controls); L5 [Protected-path review](control-reference.md#l5-repository-controls) | Related (Lite); Partly (Standard) | Agents review all code and a security review joins from Standard, but product code outside the protected paths, and outside the high-risk slices read at High, can reach main without a human reading it. |
| PW.8 | Test Executable Code to Identify Vulnerabilities and Verify Compliance with Security Requirements | L5 [Required CI, enforced by the host](control-reference.md#l5-repository-controls); L7 [Property and mutation tests](control-reference.md#l7-process-controls), [User-path end-to-end release gate](control-reference.md#l7-process-controls); L8 [Claims only from output](control-reference.md#l8-state-and-audit) | Partly (Lite) | Tests run in host-enforced CI and their adequacy is checked from Standard, but security testing such as fuzzing or penetration testing is not required. |
| PW.9 | Configure Software to Have Secure Settings by Default | None | Outside | Product defaults are the adopter's; the method's fail-closed rules govern agent enforcement only. |
| RV.1 | Identify and Confirm Vulnerabilities on an Ongoing Basis | [Sustain mode](practice.md#8-sustain-mode) (advisory triage); L1 [Isolated environment](control-reference.md#why-sessions-are-separated-and-how-far) split by input trust | Partly (Lite) | Advisories for third-party components are triaged, but reports about the product's own code and a disclosure policy are left to the adopter. |
| RV.2 | Assess, Prioritize, and Remediate Vulnerabilities | [Sustain mode](practice.md#8-sustain-mode) (verdicts, emergency vulnerability path, rollback); [exceptions](method.md#12-assurance-profiles); [suspected compromise](practice.md#suspected-compromise) | Partly (Lite) | Each advisory gets one recorded verdict and every deferral a re-check date, but risk analysis and advisories for first-party vulnerabilities are left to the adopter. |
| RV.3 | Analyze Vulnerabilities to Identify Their Root Causes | [Learning loop](method.md#11-the-learning-loop); [incident record](practice.md#9-the-incident-record) | Partly (Lite) | Each incident gets a root-cause class and a repeated failure becomes a mechanism, but the classes describe the process, not vulnerability types. |

A draft revision, [SP 800-218 Rev. 1](https://csrc.nist.gov/pubs/sp/800/218/r1/ipd) (SSDF 1.2,
initial public draft, December 2025), adds PO.6, Define and Implement a Continuous Process
Improvement Plan, which is close to the method's [learning loop](method.md#11-the-learning-loop),
and PS.4, Updates Are Robust and Reliable, which the method touches only through its release gates
and rollback. This crosswalk maps version 1.1 only.

[SP 800-218A](https://doi.org/10.6028/NIST.SP.800-218A), the SSDF profile for generative AI and
dual-use foundation models, is not mapped. It covers producing and integrating AI models, not using
them as development tools, and it treats AI-written code like any other code, so the rows above
apply to agent output unchanged. An adopter whose product trains or integrates models applies it in
its own process.

## Gaps the crosswalk shows

These are the main gaps this mapping found across the three standards, merged and stated as found.
The list is not exhaustive: every row marked Partly or Related names what it leaves open in its
note. The first list is where the method's own scope has no control, or one that only touches the
item; the second is what the method leaves to the adopter by design.

### Gaps inside the method's scope

- **Tool-kept memory outside the repository.** Memory stores an agent tool keeps for itself are
  neither enforcement material nor protected paths (AML.T0080.000, ASI06).
- **Credentials of agent tools.** The method's credential scope names the model key, push, lab,
  broker and PR-opening credentials; credentials held in tool-server configuration for other
  services are not named (AML.T0083, AML.T0098).
- **Identity of a new dependency.** Adding a dependency is a reviewed plan decision, but no step
  checks that the package exists, is the intended project and has provenance (AML.T0060,
  AML.T0011.001).
- **What approving agent tooling involves.** The approved list and pinning say who decides and
  when tooling changes, not what an approval inspects; no signatures or SBOM are required, and
  remotely hosted tool servers cannot be pinned (AML.T0010.005, AML.T0109, AML.T0110.000,
  AML.T0111, ASI04).
- **Image digests.** Images named in environment definitions are not required to be pinned by
  digest (AML.T0010.004, AML.T0074).
- **The model provider's API as an exfiltration channel.** Every session keeps the model API as an
  allowed egress host, and an allow-list by host does not bind the account used there (AML.T0086,
  AML.T0096).
- **Other allowed hosts as channels.** Registries and the code host stay allowed, and whether DNS
  passes through the allow-list is not stated (AML.T0025, AML.T0072, AML.T0108, AML.T0120, ASI02).
- **Listeners inside agent environments.** Local tool servers and development servers started in
  an agent environment have no authentication or origin rule, and from Standard they may run on the
  machine where the owner browses (AML.T0011.003, AML.T0132).
- **Markdown rendered outside agent environments.** Agent-written markdown is rendered by the host
  or the owner's tools, where egress control does not apply, and nothing checks it for external
  references (AML.T0077).
- **Other input classes.** Non-text inputs, event-triggered agents and retrieval indexes have no
  input rule of their own (AML.T0129, AML.T0051.002, AML.T0070).
- **Records the method does not keep.** The tool's own session transcripts, and agent processes a
  session starts inside its environment, are outside the method's records (AML.T0092, AML.T0103).
- **Message replay and downgrade.** Replay protection for ordinary messages exists only in an
  optional mode at High, and protocol downgrade is not addressed (ASI07).
- **Automatic response to anomalies.** No rate limit, circuit breaker or automatic pause reacts to
  anomalous behaviour; the stop is the owner's (AML.T0034.002, ASI08, ASI10).
- **Security criteria and tools.** The method fixes where checks run, not what counts as acceptable
  security risk or which security tools a toolchain must include (PO.3, PO.4).
- **Lite.** At Lite there is no security lens or security review, no environment split and no
  egress control, so the relations for PO.5, PW.2, PW.7, ASI03 and ASI05 are weaker there.
- **Evidence.** Most relations rest on self-attested proof; an assessor corroborates it against
  host, CI and signature records first ([control-reference.md](control-reference.md), How to read
  this reference).

### Left to the adopter by design

- **Agents inside the product.** They are out of the method's scope, and every standard above still
  applies to them (ASI01 to ASI10; the ATLAS groups listed as Outside).
- **Product security content.** Security requirements for the software, product threat modelling,
  secure coding, build hardening, secure defaults, SBOM and release provenance for acquirers,
  security testing tools, vulnerability disclosure and intake, training and management commitment
  are left to the adopter's process (PO.1, PO.2, PS.2, PS.3, PW.1, PW.5, PW.6, PW.8, PW.9, RV.1,
  RV.2).
- **Scope limits.** The owner's workstation, human developers' endpoints, and the data sent to the
  model provider are outside the method (PO.5, PS.1; [method.md](method.md), §0).

Several rows also meet risks the method already accepts and records per profile, such as untrusted
text in tool output or no egress control at Lite. Those are listed once, in
[method.md](method.md#12-assurance-profiles), §12, under "Residual risk at every profile" and the
Lite and Standard residual paragraphs.

**Open question for auditors: agent reviewers.** Whether an agent reviewer counts as a reviewer for
SSDF PW.2 and PW.7 is the adopter's or the auditor's call. The method's reviewers are agents, and a
human reads only the protected paths and, at High, the high-risk slices.

## Sources

- MITRE. [MITRE ATLAS™](https://atlas.mitre.org), content release 2026.09, 15 September 2026; data
  file `dist/v6/ATLAS-2026.09.yaml` in [atlas-data](https://github.com/mitre-atlas/atlas-data).
  Apache License 2.0. Read 28 October 2026.
- OWASP GenAI Security Project.
  [*OWASP Top 10 for Agentic Applications 2026*](https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/),
  9 December 2025. CC BY-SA 4.0. Read 28 October 2026.
- National Institute of Standards and Technology.
  [*NIST SP 800-218, Secure Software Development Framework (SSDF) Version 1.1*](https://doi.org/10.6028/NIST.SP.800-218),
  February 2022. Read 28 October 2026.
