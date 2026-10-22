# Related work

*How this method relates to published work, and where it differs. Current as of 22 October 2026.
Terms are defined in [glossary.md](glossary.md). Corrections and additions are welcome as issues.*

This document exists to place the method, not to stake a claim. It makes no novelty claim and
no priority claim: a reference method does not need to be first. The method is offered as a way
to do the work, and the literature below shows what it is built out of.

| Work | Governs | Unit of control | Evidence basis |
|---|---|---|---|
| **MAS organizational models** (Gaia 2000 and 2003; MOISE 2000; MOISE+ 2002) | The organization as an explicit, specified object | Roles, groups, links, role cardinality | Two decades of academic use |
| **Capability security** (confused deputy 1988; macaroons 2014; RFC 8693) | What a credential permits and who may narrow it | The credential | Deployed infrastructure |
| **Harness engineering** (2026) | How the mechanisms around a model are configured | Context files, rules, hooks, tool servers | Repository mining |
| **GAIE / OCM** (Kang 2026) | Whether a human reviews a given change | The individual code generation task | Analytical; no production data |
| **Deterministic control plane** (Madatha 2026) | Permissions, integrity and phase gates in a coding harness | Tools, paths, commands, phases | Conformance tests on injected violations |
| **This method** | How one owner runs agent teams on a product | The control layer, chosen per layer | One product, open-sourced in February 2026 and released continuously since |

## Multi-agent organizational models

Treating the organization as the governance object, specified separately from the agents, is not
new and is not a gap.

**Gaia** (Wooldridge, Jennings & Kinny, *Autonomous Agents and Multi-Agent Systems* 3, 2000)
treats a multi-agent system as a computational organization of interacting roles. The revised
Gaia (Zambonelli, Jennings & Wooldridge, *ACM TOSEM* 12(3), 2003) adds organizational structure
and the organizational rules governing global behaviour as explicit abstractions.

**MOISE** (Hannoun, Boissier, Sichman & Sayettat, 2000) models an organization through roles,
organizational links and groups. **MOISE+** (Hübner, Sichman & Boissier, 2002) separates a
structural specification (roles, groups, links), a functional specification, and a deontic
specification linking roles to goals through permissions and obligations. It added role
inheritance, recursive groups, role compatibility and **role cardinality** -- constraints such
as "this group has one to five writers and exactly one editor", an example from the Moise
software distribution.

That last item needs stating plainly: **a cardinality limit on how many agents occupy a role
within a group was formalised in an organizational specification language in 2002.** So the
*formalism* for expressing such a limit is old, and this method did not invent the idea that an
organizational specification should carry one.

One distinction survives, and it cuts both ways. MOISE+ role cardinality is a composition
constraint on a group: how many agents may hold a role. Span of control as used in method.md
§2.1 is a claim about a coordinator's capacity: how many reports one coordinator can hold before
intent degrades. MOISE+ gives the language for the first and makes no empirical claim about the
second. So the method's number is not a reinvention of role cardinality. It is also not a
result: it is one practice's operating limit, undocumented as to method, on a single case.

The classical management literature behind "span of control" did prescribe small numbers.
Graicunas (1933) set out to give the existing belief in limited spans a theoretical basis: he
counted the relationships a manager must handle as subordinates are added, and concluded that
the maximum should be five, and probably four. Urwick (*Harvard Business Review*, 1956) is
commonly credited with five, at most six, for subordinates whose work interlocks. Those numbers
come from argument about human managers, not from measurement of agent coordinators. So "three to
four" should be read as this practice's operating limit under its own task mix, not as a
transferable figure.

## Agent orchestration frameworks

The organizational frame also exists as shipped software, which is the nearest prior art of all.

**CrewAI** implements a hierarchical process in which a manager agent coordinates the workflow,
delegates tasks and validates outcomes, with the manager either supplied explicitly or created
from a configured `manager_llm`. Delegation is per-agent, gated by an `allow_delegation` flag,
and performed as ordinary tool calls -- a delegate-work tool and an ask-question tool -- that the
manager invokes inside its normal executor loop.

**MetaGPT** assigns software-company roles (product manager, architect, project manager,
engineer, QA) and runs them through standardised operating procedures on an assembly-line model,
passing information between roles as intermediate documents -- requirements, designs, interface
specifications -- rather than as free conversation. That is the same instinct as this method's artefacts
and documentation chain, and it predates it.

Roles, a manager, delegation and structured hand-off between agents are therefore shipped
features rather than propositions, and any claim to the organizational frame has to start there.

**These frameworks are also where the difference in setting is clearest.** CrewAI's delegation
is a capability the manager holds, exercised as a tool call. What reaches the worker is a task
description, not a credential: no audience, no scope, no expiry, and nothing the receiving agent
can verify. That is ambient authority in Hardy's sense, shipped as a product feature. CrewAI's
own documentation warns that agents can delegate back and forth indefinitely, and suggests
turning delegation off for specialists. That is the same observation from the other side: the
model has no narrowing property, so the practical mitigation is to limit delegation to the
manager. Delegation was on by default in earlier versions and is now off by default, to give
users explicit control. It is a configuration default, not a credential the receiver can check.

## Capability security

The delegation properties this method requires are long-specified.

**The confused deputy** (Hardy, *ACM SIGOPS Operating Systems Review* 22(4), 1988) is the
canonical statement of the failure in §2 of the method. A Tymshare compiler held write
permission to a system directory and accepted a caller-supplied output path, so a user could
direct its debugging output onto the billing file. Hardy's lesson is that permission attached
to the program rather than to the request. "*The owner approved this*, sent by an orchestrator"
is that failure with new actors: authority held by the relaying party rather than carried by
the request.

**Narrowing-only delegation** is the property called *attenuation*. Macaroons (Birgisson et
al., NDSS 2014) implement it by construction: a holder adds caveats without contacting the
issuer, and a signature chain prevents caveats being removed, so a derived credential can only
be narrower. SPKI/SDSI addressed delegation authority directly (RFC 2693). **RFC 8693** (OAuth
2.0 Token Exchange, 2020) lets an authorization server issue a token for a delegated call, with
an `act` claim that can record the chain of actors. It does not require the new token to be
narrower; that is left to the server's policy. Prior actors in nested `act` claims are
informational and are not used for access control, so the chain is recorded, not enforced.

The agent case is also already being worked: individual IETF drafts propose attenuated tokens or
delegation chains for agents (Niyikiza 2026; Hassan 2026). Delegation that only narrows is
therefore a well-specified property with deployed implementations. Authority that cannot be
relayed needs more than attenuation. Macaroons and exchanged tokens are bearer credentials:
whoever holds one can present it. Binding a credential to its holder takes sender-constrained
tokens, such as mutual-TLS-bound tokens (RFC 8705) or DPoP (RFC 9449). What this method does with
these properties is require them of agent sessions.

## Why the prior art does not simply apply

A fair question from anyone reading the sections above is why this method exists at all rather
than an adoption of MOISE+ with macaroons underneath. The honest answer is about setting rather
than about priority.

- **Hardy (1988) is about an operating system.** A compiler, a directory, a billing file, one
  machine. It names the property -- authority attaching to the running program rather than to
  the request -- and nothing in it concerns a coordinating process that summarises a human's
  decision to a subordinate process in natural language, where the summary is lossy by nature
  rather than by bug.
- **MOISE+ and Gaia specify organizations of designed, cooperative agents.** Roles are filled
  by agents built to fill them. There is no human owner holding irreversible outward actions, no
  participant that loses its policy memory mid-task, and no commercial production risk attached
  to a role being played badly. The organizational formalism transfers; the threat model behind
  this method does not come from there.
- **Macaroons and RFC 8693 give attenuation, and the agent frameworks do not implement it.** A
  grant in a running agent harness today is typically a prompt answered once, and in CrewAI
  delegation is a tool the manager calls rather than a credential the receiver checks. That gap
  is where the agent setting differs, and the IETF agent-token drafts indicate others are closing
  it.
- **The orchestration frameworks structure work without governing authority.** CrewAI and
  MetaGPT establish roles, managers and document hand-off, and CrewAI offers human review of a
  task's output and guardrails. Neither carries a human owner who holds irreversible outward
  actions as a separate authority, an assurance profile scaled to risk, or any measure of what
  the arrangement costs the human in the loop.

None of that makes this method novel. It means the known properties have to be assembled for a
setting their originators were not addressing, and that assembly is what the method is.

## Harness engineering

The method's principle that trust comes from layered controls rather than agent behaviour
(method.md §1) does not name a third place where much of 2026's work sits: the harness, the set
of mechanisms configured around a model. Galster et al. (*Harness Engineering for Agentic AI
Coding Tools: An Exploratory Study*, arXiv:2602.14690) study how coding tools are configured in
practice: context files, skills, subagents, commands, rules, settings, hooks and tool servers.
Enforcement before a call executes is better represented by Madatha's work, below.

This matters for one specific claim. The method's account of rules lost through compaction
(method.md §1, principles 6 and 7; §3) describes a harness property, which is therefore reachable by
harness changes -- longer effective context, non-compactable pinned context, or re-injecting the
rule set per turn. Whether those are sufficient is an open question. They should not be excluded
by classification.

## Graduated human oversight: GAIE

Kang's *Governed AI-Assisted Engineering* ([arXiv:2606.22484](https://arxiv.org/abs/2606.22484),
June 2026) routes each code generation task to human-in-the-loop, human-over-the-loop or
automated-with-monitoring, from four inputs: regulatory impact, customer proximity,
reversibility and data sensitivity. Each tier names its required evidence, held append-only
with a hash chain.

GAIE reached positions this method holds, and should be credited with them: that oversight must
be scaled to risk rather than applied uniformly, that uncertainty escalates upward and never
downward, and that the agent generating code cannot approve its own deployment, which is this
method's two-party rule.

GAIE's tiering is the closest prior formulation of risk-tiered escalation for code generation;
risk-proportionate human oversight as such predates it. This method composes it across multiple
levels of delegation, an increment on GAIE rather than a separate position.

**One qualifier when citing it.** GAIE's velocity-preservation figures are an analytical model
built on assumed per-tier inputs, not measurements, and the paper says its evaluation is
analytical with no production deployment data. Anyone contrasting measured against unmeasured
oversight cost should note that both sides of that contrast are currently unmeasured.

## Deterministic enforcement: Madatha

Madatha's *A Deterministic Control Plane for LLM Coding Agents*
([arXiv:2606.26924](https://arxiv.org/abs/2606.26924), June 2026) governs the configuration
layer of IDE coding harnesses. Every mechanism is ordinary testable code -- hashing, regex, a
state machine, set arithmetic -- explicitly not delegated to further LLM orchestration. Five
permission tiers are enforced as errors at install and transform time, so they fail closed;
every tier except the unrestricted orchestrator tier is a strict tool allowlist. One tier adds
write-path validation, an attack-derived shell blocklist runs on pre-tool-use hooks for two
harnesses, and git configuration and hook manipulation are forbidden by prompt rules backed by
blocklist patterns.

It arrived first at several positions this method holds: enforcement in code rather than in
agent compliance, fail-closed permission sets per role, enforcement material protected from the
session it governs, and mechanisms tested by injecting what they must reject.

**More than enforcement.** The paper includes a phase state machine with named delegation
receipts, human-in-the-loop gates after specific phases, and a capped escalation to a human after
three failed auto-fix iterations by default. A gated phase machine with delegation records and
human escalation points is an authority structure, not only an enforcement primitive.

What remains different is scope rather than category: it governs one harness on one machine,
states that it is not building a sandbox, and addresses no environment isolation, nested
delegation or approval routing. Its phase-gate overhead is explicitly unmeasured.

**On its prevalence study.** The sub-1% permission-declaration figure is labelled indicative in
the paper, produced by a fragile parser, with n=31 true positives of 6,145 files, and is not
the study's headline finding. The headline finding is that agent configurations propagate as
undeclared shared components: 10.1% of tracked paths are exact duplicates across independent
repositories, after adjusting for forks. A second figure, 75.5% of clone pairs crossing
organizational boundaries, is computed on raw pairs without fork filtering. Two further
qualifiers for anyone citing it: the sample is 10,008 *public* repositories, which is not the
enterprise population, and no base rate for configuration-file cloning is offered, so a
cross-organization clone may be a widely adopted good default rather than an ungoverned copy.

## Enterprise control architecture: CASE

The CASE framework (Telukunta, Lilis & Baron, [arXiv:2608.10153](https://arxiv.org/abs/2608.10153),
August 2026) is a four-layer control architecture for enterprise agentic AI, with a five-level
maturity model and an assessment instrument. Its third layer applies supervisory cybernetics to
human-agent teams: tiered autonomy, federated oversight loops and role-bound override rights. It
is the nearest recent treatment of how humans supervise agent teams, though not of team
structure, which the organizational models above treat more directly. It governs enterprise
agent operations generally rather than software delivery; coding agents are not addressed.

## Environment isolation

The techniques in L1 and L2 are not novel and are not claimed as such. Vendor and practitioner
guidance has converged on the same answers: a worktree is not a security boundary, egress
should be default-deny with an allowlist, DNS is an exfiltration path, metadata endpoints and
private ranges should be blocked, credentials belong outside the sandbox, and isolation strength
runs microVM over user-space kernel over container.

This method arranges those techniques as a baseline with profiles, raise-triggers, recorded
residual risks and a stated proof per control.

## When multiple agents help

Practitioner and research work since 2024 converges on one point: more agents is not better by
default, and whether they help depends on how the work decomposes.

Anthropic's guidance on building agents (2024) recommends "finding the simplest solution
possible, and only increasing complexity when needed", and names the patterns this method's
execution patterns draw on: routing, orchestrator-workers and evaluator-optimizer. Its account
of a multi-agent research system (2025) reports roughly fifteen times the tokens of a chat
interaction for a multi-agent setup, gains on breadth-first research, and the observation that
most coding tasks are less parallelisable than research. It also stresses detailed delegation
briefs and effort scaled to complexity.

Kim et al. measured, in the first version of their study (2025) and its blog (2026), 180
configurations across four benchmarks and three model families; the current version (2026)
reports 260 configurations across six benchmarks. In the first version, multi-agent coordination
improved a parallelisable task by up to 81% and degraded a sequential one by 39-70%. Errors were
amplified 17.2 times with independent agents and 4.4 times with a centralised orchestrator,
measured on traces. Task decomposability and tool count predicted the best architecture for
most unseen configurations. This method's rule of choosing a pattern by coupling rather than
size, its default to one agent for sequential work, and its routing of workers through one
orchestrator follow the same direction. Their study did not test multi-level hierarchies, so it
neither supports nor contradicts patterns P5 and P6.

Cognition's two posts trace a practitioner's position over ten months. The first (2025) argued
against multi-agent systems: agents need shared context, and "actions carry implicit decisions",
so parallel agents with different assumptions produce clashing work. The second (2026) reports what
now works in their product: writes kept single-threaded, reviewers given a clean context, escalation
to a stronger model that receives the primary's context, and manager-child delegation. Parallel
writers into shared work still do not. This method's project team runs workers in parallel on
separate branches with orchestrator-owned integration, which is the manager-child shape; its
assumptions field and clean-context reviewers address the same failure modes. Its tier
escalation differs: the stronger tier starts fresh from the last reviewed commit rather than
receiving the failing context. Both posts rest on internal, mostly anecdotal evidence.

Cemri et al. (2025) built a taxonomy of fourteen multi-agent failure modes in three categories
(system design, inter-agent misalignment, task verification) from 150 traces, and applied it to
more than 1,600 annotated traces. The method's learning loop can tag incidents with these
categories, so that one practice's incidents can be compared with published failure data.

## Calibrated decision models

The decision classifier (control reference, L6) and the action classifier (L3) assume a model
that returns a confidence over typed questions, with thresholds fitted from a target error rate.
Two 2026 preprints describe that class of model, and the first shows why the thresholds have
to be fitted locally.

Guo et al. evaluate a model trained with reinforcement learning for calibrated decisions (RLCD) as
a zero-shot detector across ten alignment-failure types, prompt injection, deception, reward
hacking and power seeking among them. They report a median AUROC of 0.886 from one generic
question. Their benchmarks include agent trajectories in which a tool result carries an injected
instruction (InjecAgent, AUROC 0.988 with the generic question), so detecting injection that
arrives through tool output is partly their result. They do not evaluate inter-agent messages;
applying the model there is this method's extrapolation. The model's probabilities are
calibrated when pooled across benchmarks but not within one, and a threshold fitted on a few
local labels recovers most of the F1 gap on benchmarks with rule-based labels, which is why the
method fits thresholds locally. They also find
that context matters more than question wording, mostly through fields that encode the label.
That finding is not about who writes the context, so the rule that the action classifier takes
its context only from trusted sources is the method's own design choice, not their result.

Gao and Wang publish an open RLCD training recipe ("calibrate, then reinforce") and report
selective prediction: the share of items decided alone at a bounded error rate. That is the form
in which this method sets classifier thresholds and records coverage. Their evaluation uses one
1.7B model on two reasoning tasks, math answer verification and knowledge-heavy multiple choice,
not security. On a separate dataset of annotator disagreement, they show that the RLCD objective
gains nothing over cross-entropy training when the uncertainty comes from the labels themselves.
The method's choice to decide scope deterministically does not rest on that result: scope has a
deterministic answer in the slice plan, so it is not left to a model.

Neither paper is evidence that the controls built on these models work in this setting. The
method requires local calibration per decision point and recorded error rates for that reason.

## Standards and frameworks

Security leaders usually map a new method onto the frameworks they already report against. This
section is placement for that, not a compliance claim. The method's layers touch familiar control
domains: environment segregation (L1, L2), access control and segregation of duties (L4), change
management (L5, L7), logging and audit (L8), and supply-chain integrity (L3, and L5 at High).
NIST's Secure Software Development Framework (SSDF) and SLSA cover the build and supply-chain
side. OWASP's guidance on LLM and agentic applications covers threats to agent sessions. NIST's
AI Risk Management Framework and ISO/IEC 42001 cover AI governance at the level of the
organisation. The method maps no clauses and offers no crosswalk.

## A practitioner's reading

Cruz (2026) read v0.7 of this method against a newsroom run by agents. It is one practitioner's
self-assessment of his own setting against the method, not an audit, and its evidence is that one
setting.

It reports independent convergence on several principles: authority stays with a person for
outward actions; the owner receives decisions, not files; a rule that breaks twice becomes a
mechanism; rule text is not a boundary; messages carry information, never authority; and a
manifest is anchored on the owner's key. Arriving at the same ideas from a different starting
point is a signal that they are reachable, not evidence that they work.

The "Kept by" column of the control reference adapts its framing of each rule as what the agent
can do, what it may do, the gap between the two, and the barrier that keeps the gap closed. The
article calls the first of these a grant. This method calls it the agent's capability, in the
sense of what it can do, distinct from the capability library and from capability security's
token of authority, because a grant here is an owner-issued widening of authority.

The article makes four other proposals. The method responds to two. Memory loss as a design input
became principle 7: limits are re-read from durable artefacts and re-checked, and the restated
limits are re-injected at intervals, modelled on the article's own status reports. Encrypted
transport became an end-to-end encrypted message option at High; the article also proposes
encrypted state, which the method does not adopt. Its proposal to govern agents inside the
product is recorded as out of scope (method.md §0). Its offer of measured owner metrics, as a second
evidence base, is not yet taken up.

The reading also found its own CI to be in-band: the agent that pushes can change the checks
that gate it, in the same push. That is the case the out-of-band CI rule addresses (control
reference, L5).

## What this method is

**A synthesis assembled for one setting.** Eight layers, three profiles chosen independently per
layer with explicit raise-triggers, coherence rules, recorded accepted residual risks and a
stated proof per control. The constituent controls come from the bodies of work above. What is
offered is their assembly into something a single owner can adopt, designed so an auditor can
hold it.

**A method that instruments the owner.** method.md §14 treats the owner's queue as a thing to
measure: owner interrupts against real decisions, owner-wait time as the sum of blocked work
waiting on a person, and fitting work left waiting while a fitting worker is idle. The measures
are defined here and have not been published from this practice, so they are a proposal for what
to collect rather than a reported result. A reader weighing the oversight-cost literature should
note that Kang's velocity figures are modelled and Madatha's phase-gate overhead is explicitly
unmeasured, so there is currently little measured data on any side.

**An open question this method does not answer.** Human delegation is load-bearing on
properties agents lack: an agent holds no reputation across grants, learns nothing from being
revoked, and cannot bear a consequence. Responsibility therefore collapses onto the human in
every case, which is why §1's first principle puts authority there. The disanalogies may matter
more than the transfers -- no human organization has a class of participant that loses its
policy memory at unpredictable intervals. Principle 7 designs for that memory loss.
This method is built on the organizational resemblance and does not yet account for the other
places where it fails.

## Works cited

- Wooldridge, M., Jennings, N. R., & Kinny, D. (2000). *The Gaia Methodology for Agent-Oriented
  Analysis and Design*. Autonomous Agents and Multi-Agent Systems, 3(3), 285-312.
- Zambonelli, F., Jennings, N. R., & Wooldridge, M. (2003). *Developing Multiagent Systems: The
  Gaia Methodology*. ACM Transactions on Software Engineering and Methodology, 12(3), 317-370.
- Hannoun, M., Boissier, O., Sichman, J. S., & Sayettat, C. (2000). *MOISE: An Organizational
  Model for Multi-agent Systems*. IBERAMIA-SBIA 2000, LNAI 1952.
- Hübner, J. F., Sichman, J. S., & Boissier, O. (2002). *A Model for the Structural, Functional,
  and Deontic Specification of Organizations in Multiagent Systems*. SBIA 2002, LNCS 2507,
  118-128. Extended abstract: *MOISE+: Towards a Structural, Functional and Deontic Model for MAS
  Organization*. AAMAS 2002.
- Moise software distribution, `writePaper` example specification. github.com/moise-lang/moise.
- Hardy, N. (1988). *The Confused Deputy (or why capabilities might have been invented)*. ACM
  SIGOPS Operating Systems Review, 22(4).
- Birgisson, A., et al. (2014). *Macaroons: Cookies with Contextual Caveats for Decentralized
  Authorization in the Cloud*. NDSS 2014.
- Ellison, C., et al. (1999). *SPKI Certificate Theory*. RFC 2693.
- Jones, M., et al. (2020). *OAuth 2.0 Token Exchange*. RFC 8693.
- Campbell, B., et al. (2020). *OAuth 2.0 Mutual-TLS Client Authentication and Certificate-Bound
  Access Tokens*. RFC 8705.
- Fett, D., et al. (2023). *OAuth 2.0 Demonstrating Proof of Possession (DPoP)*. RFC 9449.
- Niyikiza, N. A. (2026). *Attenuating Authorization Tokens for Agentic Delegation Chains*.
  Internet-Draft draft-niyikiza-oauth-attenuating-agent-tokens-01, individual submission.
- Hassan, A. (2026). *An Attenuated Delegation Profile for Automated Agents*. Internet-Draft
  draft-hamr-oauth-agent-delegation-02, individual submission.
- Graicunas, V. A. (1933). *Relationship in Organization*. Bulletin of the International
  Management Institute. Urwick, L. (1956). *The Manager's Span of Control*. Harvard Business
  Review.
- CrewAI documentation: Processes, Hierarchical Process, Collaboration, Agents, Tasks. docs.crewai.com,
  accessed 21 October 2026.
- Hong, S., et al. (2024). *MetaGPT: Meta Programming for A Multi-Agent Collaborative
  Framework*. ICLR 2024. arXiv:2308.00352.
- Galster, M., Mohsenimofidi, S., Lulla, J. L., Abubakar, M. A., Treude, C., & Baltes, S.
  (2026). *Harness Engineering for Agentic AI Coding Tools: An Exploratory Study*.
  arXiv:2602.14690 [cs.SE]. Based on a paper in the Proceedings of AIware 2026.
- Kang, R. (2026). *Governed AI-Assisted Engineering: Graduated Human Oversight for Agentic Code
  Generation in Regulated Domains*. arXiv:2606.22484 [cs.HC].
- Madatha, P. (2026). *A Deterministic Control Plane for LLM Coding Agents*. arXiv:2606.26924 [cs.SE].
- Telukunta, S., Lilis, G. N., & Baron, L. (2026). *The CASE Framework: A Multi-Disciplinary
  Control Architecture for Governing Enterprise Agentic AI*. arXiv:2608.10153 [cs.AI].
- Abbas, H. A., Shaheen, S. I., & Amin, M. H. (2015). *Organization of Multi-Agent Systems: An
  Overview*. Journal of Intelligent Information Systems, 4(3), 46-57. arXiv:1506.09032 [cs.MA], as a survey entry point to the organizational-model
  literature.
- Anthropic (2024). *Building effective agents*. Anthropic Engineering, 19 December 2024.
- Anthropic (2025). *How we built our multi-agent research system*. Anthropic Engineering,
  13 June 2025.
- Kim, Y., Gu, K., Park, C., et al. (2025). *Towards a Science of Scaling Agent Systems*.
  arXiv:2512.08296 [cs.AI] (v1 December 2025; v3 April 2026); summarised on the Google Research
  blog, 28 January 2026.
- Yan, W. (2025). *Don't Build Multi-Agents*. Cognition, 12 June 2025.
- Yan, W. (2026). *Multi-Agents: What's Actually Working*. Cognition, 22 April 2026.
- Cemri, M., Pan, M. Z., Yang, S., et al. (2025). *Why Do Multi-Agent LLM Systems Fail?*
  arXiv:2503.13657 [cs.AI].
- Guo, R., Liu, Y., Deng, G., Li, Y., Zhao, L., Wu, Y., Chen, S., Zhang, Y., & Zhang, L. Y.
  (2026). *Just Ask Jev: Reinforcement Learning for Calibrated Decisions as a Zero-Shot Detector
  of AI Alignment Failures*. arXiv:2609.29429 [cs.AI].
- Gao, Z., & Wang, P. (2026). *OpenJev-RLCD: A Working RLCD Implementation*. arXiv:2609.38850
  [cs.AI].
- NandhaKishorM. *Laya*. Software, Apache-2.0. https://github.com/NandhaKishorM/laya
- Cruz, D. (2026). *Governed semi-autonomous development, read in a newsroom run by agents: where
  Levente Simon's method and our work arrived at the same ideas, how we measure up to it, and what
  we would add*. newsroom.sgit.ai, 21 October 2026.
  https://newsroom.sgit.ai/articles/governed-semi-autonomous-development-read-here.html,
  accessed 22 October 2026.
- NIST (2022). *Secure Software Development Framework (SSDF) Version 1.1: Recommendations for
  Mitigating the Risk of Software Vulnerabilities*. SP 800-218.
- OpenSSF. *Supply-chain Levels for Software Artifacts (SLSA)*. slsa.dev.
- OWASP GenAI Security Project. *Top 10 for LLM Applications* and agentic AI security guidance.
  genai.owasp.org.
- NIST (2023). *Artificial Intelligence Risk Management Framework (AI RMF 1.0)*. NIST AI 100-1.
- ISO/IEC 42001:2023. *Information technology -- Artificial intelligence -- Management system*.

The 2026 arXiv works above are preprints. None states a venue at the time of writing, except
the harness-engineering study, which is based on a paper in the Proceedings of AIware 2026.
