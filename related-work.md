# Related work

*How this method relates to published work, and where it differs. Current as of 20 October 2026.
Corrections and additions are welcome as issues.*

This document exists to place the method, not to stake a claim. It makes no novelty claim and
no priority claim.

**Correction notice (v0.4).** The v0.2 version of this document claimed four positions in this
method were unaddressed in published work. That claim is withdrawn. The survey behind it covered
2026 preprints on coding agents and missed multi-agent organizational models, capability
security, harness engineering and the orchestration frameworks, all of which address the same
properties directly. Rather than restate the claim more carefully, it is gone: a reference method
does not need to be first, and a gap claim is the one thing in a document like this that can be
wrong at the author's expense. The method is offered as a way to do the work, and the literature
below is offered so a reader can see what it is built out of.

| Work | Governs | Unit of control | Evidence basis |
|---|---|---|---|
| **MAS organizational models** (Gaia 2000; MOISE/MOISE+ 2002) | The organization as an explicit, specified object | Roles, groups, links, role cardinality | Two decades of academic use |
| **Capability security** (confused deputy 1988; macaroons 2014; RFC 8693) | What a credential permits and who may narrow it | The credential | Deployed infrastructure |
| **Harness engineering** (2026) | What the tool layer permits before a call executes | The tool call | Empirical studies |
| **GAIE / OCM** (Kang 2026) | Whether a human reviews a given change | The individual code generation task | Analytical; no production data |
| **Deterministic control plane** (Madatha 2026) | Permissions, integrity and phase gates in a coding harness | Tools, paths, commands, phases | Conformance tests on injected violations |
| **This method** | How one owner runs agent teams on a product | The control layer, chosen per layer | One product, eight months, twelve releases |

## Multi-agent organizational models

This is the body of work the v0.2 claim should have engaged with. Treating the organization as
the governance object, specified separately from the agents, is not new and is not a gap.

**Gaia** (Wooldridge, Jennings & Kinny, *Autonomous Agents and Multi-Agent Systems* 3, 2000)
treats a multi-agent system as a computational organization of interacting roles, and models
organizational structure together with the organizational rules governing global behaviour.

**MOISE and MOISE+** (Hübner, Sichman & Boissier; MOISE 2000, MOISE+ 2002) separate a
structural specification (roles, groups, links), a functional specification, and a deontic
specification linking roles to goals through permissions and obligations. MOISE+ added role
inheritance, recursive groups, role compatibility and **role cardinality** -- constraints such
as "this group has one to five writers and exactly one editor."

That last item needs stating plainly: **a cardinality limit on how many agents occupy a role
within a group was formalised in an organizational specification language in 2002.** So the
*formalism* for expressing such a limit is old, and this method did not invent the idea that an
organizational specification should carry one.

One distinction survives, and it cuts both ways. MOISE+ role cardinality is a composition
constraint on a group: how many agents may hold a role. Span of control as used in §2.1 is a
claim about a coordinator's capacity: how many reports one coordinator can hold before intent
degrades. MOISE+ gives the language for the first and makes no empirical claim about the second.
So the method's number is not a reinvention of role cardinality. It is also not a result: it is
one practice's operating limit, undocumented as to method, on a single case.

The classical management literature behind "span of control" also says less than the method
implied. Graicunas (1933) argued the field had no theoretical basis for span limits and counted
relationships rather than prescribing a number; Urwick (*Harvard Business Review*, 1956) is
commonly credited with five, at most six; twentieth-century empirical work found median spans
closer to ten. Modern treatment holds the optimum to be contingent on task interdependence,
technology, standardisation and monitoring cost. A universal integer is the part of that
literature that did not survive, so "three to four" should be read as this practice's operating
limit under its own task mix, not as a transferable figure.

## Agent orchestration frameworks

The organizational frame also exists as shipped software, which is the nearest prior art of all
and was absent from v0.2.

**CrewAI** implements a hierarchical process in which a manager agent coordinates the workflow,
delegates tasks and validates outcomes, with the manager either supplied explicitly or created
from a configured `manager_llm`. Delegation is per-agent, gated by an `allow_delegation` flag,
and performed as ordinary tool calls -- a delegate-work tool and an ask-question tool -- that the
manager invokes inside its normal executor loop.

**MetaGPT** assigns software-company roles (product manager, architect, project manager,
engineer, QA) and runs them through standardised operating procedures on an assembly-line model,
passing information between roles as intermediate documents -- requirements, specifications, test
reports -- rather than as free conversation. That is the same instinct as this method's artefacts
and documentation chain, and it predates it.

Roles, a manager, delegation and structured hand-off between agents are therefore shipped
features rather than propositions, and any claim to the organizational frame has to start there.

**These frameworks are also where this method's narrow claim survives.** CrewAI's delegation is
a capability the manager holds, exercised as a tool call. What reaches the worker is a task
description, not a credential: no audience, no scope, no expiry, and nothing the receiving agent
can verify. That is ambient authority in Hardy's sense, shipped as a product feature. The
community guidance that only the manager should be permitted to delegate, because otherwise
delegation recurses without bound, is the same observation from the other side: the model has no
narrowing property, so the practical mitigation is to allow exactly one level of it. Published
documentation for the framework also disagrees on whether the delegation flag defaults on or
off, which suggests it is treated as a configuration convenience rather than as a security
control.

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
be narrower. SPKI/SDSI addressed delegation authority directly. **RFC 8693** (OAuth 2.0 Token
Exchange, 2020) provides downscoped tokens enforced by the authorization server, with an `act`
claim recording a delegation chain.

The agent case is also already being worked: IETF drafts exist on attenuating agent tokens and
on OAuth chain delegation for agent-to-tool calls. Delegation that only narrows, and authority
that cannot be relayed, are therefore well-specified properties with deployed implementations.
What this method does with them is require them of agent sessions, where the enforcement points
are currently missing.

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
- **Macaroons and RFC 8693 give the credential properties, and the agent frameworks do not
  implement them.** A grant in a running agent harness today is typically a prompt answered
  once, and in CrewAI delegation is a tool the manager calls rather than a credential the
  receiver checks. That gap is the narrow, real claim available here, and the IETF agent-token
  drafts indicate others are closing it.
- **The orchestration frameworks structure work without governing authority.** CrewAI and
  MetaGPT establish roles, managers and document hand-off; neither carries a human owner holding
  irreversible outward actions, an assurance profile scaled to risk, environment isolation, or
  any measure of what the arrangement costs the human in the loop.

None of that makes this method novel. It means the known properties have to be assembled for a
setting their originators were not addressing, and that assembly is what the method is.

## Harness engineering

The method's framing of a choice between model behaviour and organizational design omits a
third layer where much of 2026's work actually sits: the harness. Enforcement at the tool-call
layer is neither model nor organization. *Harness Engineering for Agentic AI Coding Tools: An
Exploratory Study* (arXiv:2602.14690) is one entry in a line that also includes work on harness
definitions, harness evolution and system-level scaling.

This matters for one specific claim. §4's rule decay is diagnosed as context compaction, which
is a harness property, and is therefore reachable by harness changes -- longer effective
context, non-compactable pinned context, or re-injecting the rule set per turn. Whether those
are sufficient is an open question. They should not be excluded by classification.

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

Risk-tiered escalation is therefore Kang's. What this method adds is composition across
multiple levels of delegation, which is an increment on GAIE rather than a separate position.

**One qualifier when citing it.** GAIE's velocity-preservation figures are an analytical model
built on assumed per-tier inputs, not measurements, and the paper says its evaluation is
analytical with no production deployment data. Anyone contrasting measured against unmeasured
oversight cost should note that both sides of that contrast are currently unmeasured.

## Deterministic enforcement: Madatha

Madatha's *A Deterministic Control Plane for LLM Coding Agents*
([arXiv:2606.26924](https://arxiv.org/abs/2606.26924), June 2026) governs the configuration
layer of IDE coding harnesses. Every mechanism is ordinary testable code -- hashing, regex, a
state machine, set arithmetic -- explicitly not delegated to further LLM orchestration. Five
permission tiers, each a strict tool allowlist enforced fail-closed at install and transform
time, with write-path validation, an attack-derived shell blocklist on pre-tool-use hooks, and
rules forbidding git configuration and hook manipulation.

It arrived first at several positions this method holds: enforcement in code rather than in
agent compliance, fail-closed permission sets per role, enforcement material protected from the
session it governs, and mechanisms tested by injecting what they must reject.

**A correction to how v0.2 characterised it.** That version filed this work under enforcement
and kept the organizational ground for this method. That classification does not hold. The
paper includes a phase state machine with named delegation receipts, human-in-the-loop gates
after specific phases, and a capped escalation to a human after three failed auto-fix
iterations. A gated phase machine with delegation records and human escalation points is an
authority structure, not only an enforcement primitive.

What remains different is scope rather than category: it governs one harness on one machine,
states that it is not building a sandbox, and addresses no environment isolation, nested
delegation or approval routing. Its phase-gate overhead is explicitly unmeasured.

**On its prevalence study.** The sub-1% permission-declaration figure is labelled indicative in
the paper, produced by a fragile parser, with n=31 true positives of 6,145 files, and is not
the study's headline finding. The headline finding is that agent configurations propagate as
undeclared shared components: 10.1% of tracked paths are exact duplicates across repositories,
75.5% of clone pairs crossing organizational boundaries. Two further qualifiers for anyone
citing it: the sample is 10,008 *public* repositories, which is not the enterprise population,
and no base rate for configuration-file cloning is offered, so a cross-organization clone may
be a widely adopted good default rather than an ungoverned copy.

## Enterprise control architecture: CASE

The CASE framework (Telukunta, Lilis & Baron, [arXiv:2608.10153](https://arxiv.org/abs/2608.10153),
August 2026) is a four-layer control architecture for enterprise agentic AI, with a five-level
maturity model and an assessment instrument. Its third layer, on human-agent teams, is the
nearest published treatment of team structure. It governs enterprise agent operations generally
rather than software delivery; coding agents are not addressed.

## Environment isolation

The techniques in L1 and L2 are not novel and are not claimed as such. Vendor and practitioner
guidance has converged on the same answers: a worktree is not a security boundary, egress
should be default-deny with an allowlist, DNS is an exfiltration path, metadata endpoints and
private ranges should be blocked, credentials belong outside the sandbox, and isolation strength
runs microVM over user-space kernel over container.

What that body of work lacks is not the techniques but the shape: it exists as scattered advice
rather than as a baseline with profiles, raise-triggers, recorded residual risks and a stated
proof per control.

## What this method is

**A synthesis assembled for one setting.** Eight layers, three profiles chosen independently per
layer with explicit raise-triggers, coherence rules, recorded accepted residual risks and a
stated proof per control. The constituent controls come from the bodies of work above. What is
offered is their assembly into something a single owner can adopt and an auditor can hold.

**A method that instruments the owner.** §14 treats the owner's queue as a thing to measure:
owner interrupts against real decisions, owner-wait time as the sum of blocked work waiting on a
person, and idle-worker time. The measures are defined here and have not been published from this
practice, so they are a proposal for what to collect rather than a reported result. A reader
weighing the oversight-cost literature should note that Kang's velocity figures are modelled and
Madatha's phase-gate overhead is explicitly unmeasured, so there is currently little measured
data on any side.

**An open question this method does not answer.** Human delegation is load-bearing on
properties agents lack: an agent holds no reputation across grants, learns nothing from being
revoked, and cannot bear a consequence. Responsibility therefore collapses onto the human in
every case, which is why §1's first principle puts authority there. The disanalogies may matter
more than the transfers -- no human organization has a class of participant that loses its
policy memory at unpredictable intervals. This method is built on the organizational
resemblance and does not yet account for where it fails.

## Works cited

- Wooldridge, M., Jennings, N. R., & Kinny, D. (2000). *The Gaia Methodology for Agent-Oriented
  Analysis and Design*. Autonomous Agents and Multi-Agent Systems, 3(3), 285-312.
- Hübner, J. F., Sichman, J. S., & Boissier, O. (2002). *MOISE+: Towards a Structural,
  Functional and Deontic Model for MAS Organization*. AAMAS 2002.
- Hardy, N. (1988). *The Confused Deputy (or why capabilities might have been invented)*. ACM
  SIGOPS Operating Systems Review, 22(4).
- Birgisson, A., et al. (2014). *Macaroons: Cookies with Contextual Caveats for Decentralized
  Authorization in the Cloud*. NDSS 2014.
- Jones, M., et al. (2020). *OAuth 2.0 Token Exchange*. RFC 8693.
- Graicunas, V. A. (1933). *Relationship in Organization*. Bulletin of the International
  Management Institute. Urwick, L. (1956). *The Manager's Span of Control*. Harvard Business
  Review.
- *Harness Engineering for Agentic AI Coding Tools: An Exploratory Study*. arXiv:2602.14690.
- Kang, R. (2026). *Governed AI-Assisted Engineering*. arXiv:2606.22484 [cs.HC].
- Madatha, P. (2026). *A Deterministic Control Plane for LLM Coding Agents*. arXiv:2606.26924 [cs.SE].
- Telukunta, S., Lilis, G. N., & Baron, L. (2026). *The CASE Framework*. arXiv:2608.10153 [cs.AI].
- *Organization of Multi-Agent Systems: An Overview*. arXiv:1506.09032, as a survey entry point
  to the organizational-model literature.

The 2026 arXiv works above are preprints with no stated venue at the time of writing.
