# Related work

*How this method relates to published work, and where it differs. Current as of 26 October 2026.
Terms are defined in [glossary.md](glossary.md). Corrections and additions are welcome as issues.*

## In short

Most parts of this method exist in published work: organizations of roles, delegation that only
narrows, enforcement in code around the model, oversight scaled to risk, and isolated agent
environments. What that work does not address is the setting: agents that pass a person's decision
on in natural language, that lose their rules mid-task, and that work under one owner, or a small
owner team, who holds every outward action. The method assembles the parts for that setting, a team
of AI agents building production software, and that assembly is what it is. Its measures of the
owner's queue are defined in [method.md](method.md) (§14) and are a proposal, not a reported result. This document exists to
place the method, not to stake a claim: it makes no novelty claim and no priority claim, because a
reference method does not need to be first, and the method is offered as a way to do the work.

**Who reads what.** To adopt the method, you can stop here and go to [method.md](method.md) and
[practice.md](practice.md). The rest of this document is the evidence, for reviewers and sceptics.
It runs in one arc: a [comparison](#comparison) in two tables;
[how agents are organized](#organizing-agents);
[how authority is held and enforced](#authority-and-enforcement);
[isolation, evidence and standards](#isolation-evidence-and-standards);
[one practitioner's reading](#a-practitioners-reading); and a
[conclusion](#conclusion-an-assembly-for-one-setting) on why the prior art does not simply apply
and what the method is. [Works cited](#works-cited) lists every published source, with a link
wherever one is stable.

**Key.** L1 to L8 are the method's eight control layers ([method.md](method.md) §5, detailed in
[control-reference.md](control-reference.md)). Lite, Standard and High are its assurance profiles
(§12), and principle numbers refer to §1.

## Comparison

The two tables place this method against the work discussed below. The first compares
positioning, level and form. The second compares authority over outward actions, enforcement,
isolation, verification and continuity; it leaves out the multi-agent system (MAS) organizational
models, capability security and harness engineering. Positioning cells paraphrase each work's own
description of itself, or for a body of work its literature; the other cells describe mechanisms.
Every cell describes what the work states, and "not stated" means the sources read do not say. The
sources for the frameworks, the papers and CrewAI were read on 26 October 2026; the rows for the
MAS organizational models, capability security and harness engineering summarise the sections
below.

Works enter by one rule: a public primary source, about AI coding agents, widely used or cited,
that governs or structures how coding agents work. Works this document already discusses keep
their rows where they fall outside the rule: the MAS organizational models, capability security,
CrewAI and CASE are not specific to coding agents, and GAIE, Madatha and the harness-engineering
study are 2026 papers whose use and citation were not assessed. pstack is included as an official
Cursor plugin; its use was not measured. Paperclip is not specific to coding agents either, since
it orchestrates agents to run a business, but it runs coding agents such as Claude Code and Codex
and is widely used: its repository had about 107,000 GitHub stars on 26 October 2026.

| Work | Positioning | Level | Form |
|---|---|---|---|
| **MAS organizational models** (Gaia 2000 and 2003; MOISE 2000; MOISE+ 2002) | Agent systems as organizations of roles, specified explicitly | The organization: roles, groups, links, role cardinality | Methodologies and specification languages |
| **[CrewAI](https://docs.crewai.com/)** | Open-source framework for orchestrating autonomous AI agents | A crew of agents inside an event-driven flow | Open-source framework run in the developer's process |
| **[MetaGPT](https://arxiv.org/abs/2308.00352)** (Hong et al. 2024) | Multi-agent framework encoding human workflows as standard procedures | A team of software roles on one task | Open-source Python framework, MIT |
| **[Paperclip](https://github.com/paperclipai/paperclip)** (v2026.1023.0) | Open-source orchestration for teams of AI agents, run as a company | A company of agents on an org chart, under a human board | Self-hosted Node.js server and web UI, MIT; adapters for many agent CLIs |
| **[GSD Core](https://github.com/open-gsd/gsd-core)** (v1.18.0) | Meta-prompting, context engineering and spec-driven development system | One project as milestones and phases, from one orchestrating session | Installable prompts and hooks for many agent tools |
| **[GitHub Spec Kit](https://github.com/github/spec-kit)** (v1.2.3) | Structured processes, templates and documented outcomes for coding agents | One feature, under a project constitution | Python CLI installing per-agent commands, extensions and workflows |
| **[BMAD Method](https://github.com/bmad-code-org/BMAD-METHOD/tree/v6.12.4)** (v6.12.4) | Idea to working software without giving up the thinking | One change per session; larger work as tracked stories | Installable skills and agent personas |
| **[pstack](https://github.com/cursor/plugins/tree/main/pstack)** (0.15.32) | The author's daily skills for writing less, higher-quality code | One session, up to a coordinator over subagent fleets | Cursor plugin; generated copy for Claude Code |
| **[Superpowers](https://github.com/obra/superpowers)** (7.0.1) | A software development methodology for coding agents, as composable skills | One feature, from design to finished branch, in one session | Skills plugin with a session-start hook, for many harnesses |
| **Capability security** (confused deputy 1988; macaroons 2014; [RFC 8693](https://www.rfc-editor.org/rfc/rfc8693)) | Authority carried by credentials and narrowed on delegation | A credential and its delegation chain | Papers, RFCs and Internet-Drafts, with deployed implementations |
| **[Harness engineering](https://arxiv.org/abs/2602.14690)** (2026) | How coding tools are configured in practice | The harness around one model | An exploratory study |
| **[Deterministic control plane](https://arxiv.org/abs/2606.26924)** (Madatha 2026) | A deterministic control plane above the coding harness | An organisation's agent definitions and harness configuration | Node.js tool: CLI and tool server, seven IDE targets |
| **[GAIE / OCM](https://arxiv.org/abs/2606.22484)** (Kang 2026; OCM: Oversight Classification Model) | Graduated human oversight for code generation in regulated domains | A single code generation task | Framework paper with a reference architecture |
| **[CASE](https://arxiv.org/abs/2608.10153)** (Telukunta et al. 2026) | Control architecture for governing enterprise agentic AI | Organisation: agent, collective, human-agent team, fleet | Paper: reference architecture, maturity model, assessment instrument |
| **This method** | Tool-agnostic reference method for governing agent teams that build production software | An agent team on one product, under one owner or a small owner team | A method: documents, with a Claude Code reference mapping |

| Work | Authority over outward actions | Enforcement | Isolation | Verification | Continuity |
|---|---|---|---|---|---|
| **CrewAI** | Set in developer code; Flows offer human-feedback gates | Framework parameters and task guardrails in developer code | Docker execution deprecated; external sandbox services recommended | Manager evaluates; guardrails; optional human review of final answer | Context summarising, stored memory, persisted flow state |
| **MetaGPT** | Humans initiate, observe and stop runs | Procedures as prompt sequences; structured outputs | Not stated | Engineer runs unit tests, up to three retries; no human gate described | Shared message pool; agent memory |
| **Paperclip** | Board approves strategy, budget overrides and, if set, hires; per gateway action Allowed, Ask first or Off, with actions on an ordinary new connection Allowed; an approval covers one call or saves a revocable rule; shell git and gh left to host branch protection | Server-side authorization bounded by a responsible user; run-scoped agent tokens; tool gateway; runtime review stages; hard budget caps pause the agent; spend approval and merge refusal by agent instructions | Host by default; worktree per task and sandbox environments are experimental opt-ins; Claude Code skips permission prompts by default | Optional review and approval stages per issue, self-review excluded; agent review loops escalate to a human after three rounds by default | Tasks, comments and the agent's home folder kept in the server; adapters resume saved sessions; append-only activity feed |
| **GSD Core** | Not stated; interactive mode confirms each step, "yolo" auto-approves | Prompts; blocking hooks on some runtimes; advisory injection hooks | Worktree per parallel executor; no sandbox, by stated design | Plan checker, verifier subagent, human acceptance testing | `.planning/` state files; fresh-context executors; context-fill warnings |
| **GitHub Spec Kit** | Not stated; human reviews each step and approves workflow gates | Prompts and templates; workflow engine and scripts outside the model | None in core; shell steps run with the user's privileges | Consistency and convergence commands in the session; human per step | Constitution and per-feature spec files; resumable workflow runs |
| **BMAD Method** | User approves flagged plans; unattended worker commits locally, does not push | Skills and step files; scripts for state; no core hooks described | Not stated; unattended worker needs a clean working tree | Three reviewer layers by default; repair loop halts after five rounds; human retrospective | Spec file status as state; tracking files; fresh chat per workflow |
| **pstack** | Reversible work and external actions such as team chat proceed; always pauses for irreversible writes such as deploys and force-pushes | Prompts and playbooks; bookkeeping scripts; no hooks | Cloud environment or worktree per worker; one writer per branch | Verifier on another model family for costly units; verdicts keyed to the head commit | Standing orders re-injected on every spawn and resume; ledger files |
| **Superpowers** | Stops before merge, shared push or publish; user decides integration | Prompts; a session-start hook that injects rules and does not block | Git worktree with consent; per-plan scratch directory | Test-first; fresh reviewer per task in subagent mode; capped fix loop; final review | Per-plan ledger; rules re-injected after compaction; committed design docs; plan files |
| **Deterministic control plane** | Human gates at spec, design, pre-release and merge | Errors at install time; blocklist hooks in two harnesses | None; states it is not building a sandbox | Human gates, delegation receipts, scan-diff security gate | Persisted phase state; hash-chained local audit log |
| **GAIE / OCM** | Human deploy approval in Tiers 1 and 2; Tier 3 auto-deploys on gates | Rule engine and CI approval gate, at architecture level | Sandboxed execution named, not detailed | Evidence per tier; generator cannot approve its own deployment | Append-only, hash-chained provenance; context loss not stated |
| **CASE** | Approval gates for high-consequence action classes; role-bound override rights | Gateway budget caps, per-call credential checks, policy-as-code gates | Per-agent workload identity, budget cells; no sandbox specified | Rubric judge, human escalation packets, evals in CI | Provenance-tagged shared memory; signed decision chains |
| **This method** | Owner holds every merge, release and outward action, at every profile | Per control: expectation, setting or boundary, chosen by profile | By profile: a dedicated machine or one isolated environment, worktree per session; containers or VMs split by authority, environment authority and input trust; VM per session | Fresh-context review; host-enforced CI out of band; human reading of protected paths, and of high-risk slices at High | Ledger on its own git ref; sessions rebuild from it |

## Organizing agents

Roles, managers, delegation and structured hand-off between agents are old ideas and shipped
features, and session workflow frameworks already structure how one agent plans, checks and records
its work. These sections show where those ideas come from and how current frameworks use them; the
differences from this method lie mostly in who holds authority and what an agent can change.

### Multi-agent organizational models

Treating the organization as the governance object, specified separately from the agents, is not
new and is not a gap.

**[Gaia](https://doi.org/10.1023/A:1010071910869)** (Wooldridge, Jennings & Kinny, *Autonomous
Agents and Multi-Agent Systems* 3, 2000) treats a multi-agent system as a computational
organization of interacting roles. The [revised Gaia](https://doi.org/10.1145/958961.958963)
(Zambonelli, Jennings & Wooldridge, *ACM TOSEM* 12(3), 2003) adds organizational structure and the
organizational rules governing global behaviour as explicit abstractions.

**[MOISE](https://doi.org/10.1007/3-540-44399-1_17)** (Hannoun, Boissier, Sichman & Sayettat, 2000)
models an organization through roles, organizational links and groups.
**[MOISE+](https://doi.org/10.1007/3-540-36127-8_12)** (Hübner, Sichman & Boissier, 2002) separates
a structural specification (roles, groups, links), a functional specification, and a deontic
specification linking roles to goals through permissions and obligations. It added role inheritance,
recursive groups, role compatibility and **role cardinality** -- constraints such as "this group has
one to five writers and exactly one editor", an example from the [Moise software
distribution](https://github.com/moise-lang/moise/blob/main/examples/specifications/wp-os.xml).

That last item needs stating plainly: **a cardinality limit on how many agents occupy a role
within a group was formalised in an organizational specification language in 2002.** So the
*formalism* for expressing such a limit is old, and this method did not invent the idea that an
organizational specification should carry one.

One distinction survives, and it cuts both ways. MOISE+ role cardinality is a composition
constraint on a group: how many agents may hold a role. Span of control as used in
[method.md](method.md) §2.1 is a claim about a coordinator's capacity: how many reports one
coordinator can hold before intent degrades. MOISE+ gives the language for the first and makes no
empirical claim about the second. So the method's number is not a reinvention of role
cardinality. It is also not a result: it is one practice's operating limit, undocumented as to
method, on a single case.

The classical management literature behind "span of control" did prescribe small numbers.
Graicunas (1933) set out to give the existing belief in limited spans a theoretical basis: he
counted the relationships a manager must handle as subordinates are added, and concluded that
the maximum should be five, and probably four. Urwick (*Harvard Business Review*, 1956) is
commonly credited with five, at most six, for subordinates whose work interlocks. Those numbers
come from argument about human managers, not from measurement of agent coordinators. So "three to
four" should be read as this practice's operating limit under its own task mix, not as a
transferable figure.

### Agent orchestration frameworks

The organizational frame also exists as shipped software, which is the nearest prior art of all.

**[CrewAI](https://docs.crewai.com/)** implements a hierarchical process in which a manager agent
coordinates the workflow, delegates tasks and validates outcomes, with the manager either supplied
explicitly or created from a configured `manager_llm`. Delegation is per-agent, gated by an
`allow_delegation` flag, and performed as ordinary tool calls -- a delegate-work tool and an
ask-question tool -- that the manager invokes inside its normal executor loop.

**[MetaGPT](https://arxiv.org/abs/2308.00352)** assigns software-company roles (product manager,
architect, project manager, engineer, QA) and runs them through standardised operating procedures
on an assembly-line model, passing information between roles as intermediate documents --
requirements, designs, interface specifications -- rather than as free conversation. That is the
same instinct as this method's artefacts and documentation chain, and it predates it.

Roles, a manager, delegation and structured hand-off between agents are therefore shipped
features rather than propositions, and any claim to the organizational frame has to start there.

**These frameworks are also where the difference in setting is clearest.** CrewAI's delegation is a
capability the manager holds, exercised as a tool call. What reaches the worker is a task
description, not a credential: no audience, no scope, no expiry, and nothing the receiving agent can
verify. That is ambient authority in Hardy's sense (see [Capability
security](#capability-security)), shipped as a product feature. CrewAI's own documentation warns
that agents can delegate back and forth indefinitely, and suggests turning delegation off for
specialists. That is the same observation from the other side: the model has no narrowing property,
so the practical mitigation is to limit delegation to the manager. Delegation was on by default in
earlier versions and is now off by default, to give users explicit control. It is a configuration
default, not a credential the receiver can check.

**[Paperclip](https://github.com/paperclipai/paperclip)** calls itself "Open-source orchestration
for teams of AI agents" and "the operating system for your AI company". It is a self-hosted server
and web UI that runs agents on an org chart under a human board; its adapters cover many coding
tools, Claude Code and Codex among them. It governs authority in several places. The board
approves strategy, budget overrides and, when a company setting is on, hires; it can pause or
terminate any agent and reassign its tasks. A responsible user bounds every agent write and is
recorded beside the acting agent, and local agent tokens name their run and expire. A hard budget
cap pauses an agent. An issue can pass through review and approval stages before it is done, and
the agent that did the work is excluded from reviewing it. Agent configuration saves are revisions
that can be rolled back, and skill edits are versioned and can be restored. It differs from this
method in what runs by default and in who can change the rules:

- **Actions run unless held.** Each action through its tool gateway is Allowed, Ask first or Off,
  and active actions on an ordinary new connection "start as Allowed", destructive ones included.
  Hires skip the approval queue when the company setting is off. The board chooses what to hold;
  this method holds every merge, release and outward action with the owner at every profile
  ([method.md](method.md) §1, principle 1).
- **An approval can stand.** An Ask-first prompt can be answered "Always allow", which saves a
  trust rule: the same agent may call the same action with any arguments on that connection,
  within the project when one is set, until the rule is revoked. This method has no standing
  approvals ([control-reference.md](control-reference.md), L2).
- **Git is left to the host.** The docs state that shell git and gh "are not constrained by
  per-tool Ask-first controls", and point to branch protection and required reviews on GitHub.
  In the GitHub recipe, the coding agent is configured through its own `AGENTS.md` to refuse
  merging on its own, and "a human or a senior agent merges". In this method, merge is owner-only
  on the repository host (L5).
- **Instruction changes take effect before review.** An agent "can edit its own folder" of
  instructions, which has "no revision history". By default every agent can create and edit
  skills, and an updated skill reaches every agent that uses it unless that agent is pinned, with
  no approval step described. A company skill policy or the opt-in low-trust preset narrows this.
  In this method, a change to agent instruction files reaches the owner as the diff before it
  reaches main (L5: an expectation at Lite, a setting from Standard where code-owner rules enforce
  it), and skills are agent tooling from an owner-approved list, pinned from Standard with changes
  through the same review (L3).
- **Isolation is opt-in.** Runs execute on the Paperclip host by default, in one shared working
  tree. A worktree per task and sandbox environments are experimental opt-ins, and the Claude
  Code adapter skips permission prompts by default.

Of note: "Allow once" answers one call, as a grant does at this method's Standard profile; at High,
a signed grant covers any action in its scope until it expires or is revoked (L4). The activity
feed is append-only.

### Session workflow frameworks

A second family of shipped work structures how coding agents work inside a session or a project:
what they plan, when they stop, how they check their output and what they record for the next
session.

#### GSD Core

[GSD Core](https://github.com/open-gsd/gsd-core) describes itself as a meta-prompting, context
engineering and spec-driven development system for many coding agents. It addresses context rot:
heavy work runs in fresh-context subagents, and the main session acts as a thin orchestrator that
never touches source files. A project runs as milestones of phases, and state lives in a
`.planning/` directory that agents read instead of relying on memory. Most rules are prompts. On
some runtimes, hooks block an executor dispatched without worktree isolation, writes outside the
worktree and reads of secret files; its prompt-injection hooks advise unless blocking is turned
on. The project states that there is no sandbox, and that consent, integrity and reversibility are
the barrier.

Two mechanisms are of note. Each blocking hook replaced a prose rule after an observed failure,
and its header records the incident that the prose rule failed to prevent;
[method.md](method.md) §1, principle 6 and §11 describe the same move from rule to mechanism. A
context-monitor hook warns at 35% of context remaining and asks for a stop and a state save at
25%; the project calls it advisory. The
[original GSD repository](https://github.com/gsd-build/get-shit-done) is archived and points to
GSD Core.

#### GitHub Spec Kit

[Spec Kit](https://github.com/github/spec-kit) is a toolkit that gives coding agents structured
processes, templates and documented outcomes, and it calls its goals experimental. A project has
one constitution, and each feature gets its own spec, plan and tasks. The user runs each command
and reviews its result before the next. Most enforcement is prompt and template: the plan template
checks the constitution through gates, and a failed gate must be justified in writing. Two parts
run outside the model. A workflow engine runs approval gates that abort on rejection, shell steps
and agent steps, and keeps run state. Extension events are translated into each agent's own hook
configuration and run scripts directly. The project states: "There is no capability sandbox".

Of note: checklist marks belong to the reviewer, and the implement command treats them as a
read-only gate. A convergence command compares the implementation with the spec and may append
tasks but not edit code.

#### BMAD Method

[BMAD](https://github.com/bmad-code-org/BMAD-METHOD/tree/v6.12.4) aims to "turn an idea or change
request into working software without giving up the thinking" (its README). This section describes
the stable v6.12 line. A typical session is one goal of about 500 lines of code. Before code, the
build skill reports intent gaps, irreversible actions and footprint. Anything flagged gets a
written plan that the user approves, and the approved intent block can then be changed by the
human alone. Larger work becomes tracked stories, built one per invocation by an unattended worker
that commits locally and does not push; a human or an outside orchestrator owns dispatch.
Enforcement is skills, personas and step files, with scripts for state; no hooks are described for
core BMAD. Review runs three reviewer layers by default, on models as capable as the session's, and
the repair loop halts for a human after five rounds.

Of note: a review finding rooted in the approved intent reverts the code and returns the question
to the human, while a finding rooted elsewhere amends the spec with a logged change. A v7 preview
on the default branch differs: a Product Owner agent slices work into a ticket tree, and the
unattended worker never marks a ticket done.

#### pstack

[pstack](https://github.com/cursor/plugins/tree/main/pstack) is a Cursor plugin, also installable
in Claude Code. Its README presents it as the author's everyday skills for shipping code at
Cursor, meant to help you write less, higher-quality code. A router matches each task to one of 23
playbooks. Enforcement is prompt-level, with bookkeeping scripts and no hooks. It prefers lessons
encoded as a lint, a runtime check or a script over more text, the move [method.md](method.md) §1,
principle 6 also makes.

Its `orchestrate` playbook reached several structures this method also uses, and should be
credited with them. A coordinator runs tracks of workers, one writer per worktree or branch. A
ledger and a decision log are kept as files. A standing-orders file is pasted verbatim into every
spawn and every resume, because "Directives decay across resumes" (orchestrate playbook). For
costly or high-blast-radius units, a verifier on a different model family from the worker records
a verdict keyed to the head commit, and a new head voids it. These correspond to this method's
orchestrator and ledger, its limits re-injected from the ledger (method.md §1, principle 7;
[practice.md](practice.md) §7), and its fresh-context verification. Similar structures in two
practices are a signal that they are reachable, not evidence that they work.

One difference concerns authority. pstack's default lets reversible work and external actions,
such as team chat, ticket updates and starting evals, proceed without asking. It always pauses for
irreversible writes, among them deploys, force-pushes to shared branches, data deletion and
customer messages. The user can grant fuller autonomy in the prompt. Under that grant, the agent
applies and reports a default for calls only the user can make, and "Gates the operator named and
the Always-pause list in Autonomy still need the operator" (poteto-mode skill). On request, an
autopilot playbook carries each pull request through merge after a verifier pass. This method
holds every outward-facing action with the owner, merges and external communication included
(method.md §1, principle 1).

#### Superpowers

[Superpowers](https://github.com/obra/superpowers) describes itself as a software development
methodology for coding agents, built from composable skills. It takes a feature from a design the
human approves, through a plan of small tasks, to a finished branch, in one session that
dispatches subagents. Enforcement is prompt-level: skills are written as mandatory, with tables
that rebut common rationalisations. One session-start hook injects the skill rules at startup,
after a clear and after compaction; it does not block. Work runs in a git worktree created with
consent. In subagent-driven execution, each task gets a fresh reviewer that treats the
implementer's report as unverified, and a fix loop capped at five rounds; inline execution keeps
one fresh review of the whole branch. During execution the agent does not check in: it decides
conflicts and ambiguities itself, records each as a ruling and reports every ruling at the end. It
stops for irreversible or security-sensitive actions and for side effects outside the worktree,
such as a merge, a shared push or a publish. The integration decision stays with the user.

Of note: skills are written test-first. A pressure scenario runs on a subagent without the skill,
and the skill is written until the agent complies. Its philosophy lists "Evidence over claims",
and a completion claim needs fresh verification evidence, as in this method's fourth principle.
After compaction the agent trusts the per-plan ledger rather than memory, as in the seventh
([method.md](method.md) §1).

#### Further spec-driven work

[OpenSpec](https://github.com/Fission-AI/OpenSpec) and [Kiro specs](https://kiro.dev/docs/specs/)
are further spec-driven work. OpenSpec keeps each change as a folder of proposal, spec deltas,
design and tasks, which its CLI validates. Kiro gives each feature spec requirements, design and
tasks files, with a bugfix file in place of requirements for a bugfix spec, and its hooks can block
a tool call until preconditions hold.

#### How these relate to this method

These frameworks structure how one agent or one coordinator plans, checks and records work, and
several also set stop points, worktrees and verifiers. This method specifies the controls around a
team: who holds outward actions, how environments are separated by authority, and which
verification runs outside every agent environment. It leaves the in-session workflow open. A
worker in this method could run one of them inside its slice. The framework's plugins, skills,
subagent definitions and hooks are then agent tooling: from an owner-approved list, and from
Standard pinned with auto-update off and changed through the protected-path review
([control-reference.md](control-reference.md), L3). Its instruction files that steer every session
reach the owner as the diff from Lite, and its hooks from Standard; hooks and hook scripts are also
enforcement material, read-only to the worker's session (control-reference.md, L3 and L5). A
framework's text that lets an agent act outward does not change the session's denied permissions
(control-reference.md, L3).

## Authority and enforcement

Who may do what, and what stops an agent doing more, is also well worked: these sections cover
delegation that only narrows, enforcement around the model, oversight scaled to risk, and control
of agent operations across an enterprise. Each property exists in published work; what a given work
leaves out is the agent setting (capability security), environment isolation (Madatha), delegation
across levels (GAIE) or software delivery (CASE).

### Capability security

The delegation properties this method requires are long-specified.

**[The confused deputy](https://doi.org/10.1145/54289.871709)** (Hardy, *ACM SIGOPS Operating
Systems Review* 22(4), 1988) is the canonical statement of the failure in [method.md](method.md) §2.
A Tymshare compiler held write permission to a system directory and accepted a caller-supplied
output path, so a user could direct its debugging output onto the billing file. Hardy's lesson is
that permission attached to the program rather than to the request. "*The owner approved this*, sent
by an orchestrator" is that failure with new actors: authority held by the relaying party rather
than carried by the request.

**Narrowing-only delegation** is the property called *attenuation*.
[Macaroons](https://www.ndss-symposium.org/ndss2014/macaroons-cookies-contextual-caveats-decentralized-authorization-cloud)
(Birgisson et al., NDSS 2014) implement it by construction: a holder adds caveats without contacting
the issuer, and a signature chain prevents caveats being removed, so a derived credential can only
be narrower. SPKI/SDSI (Simple Public Key Infrastructure / Simple Distributed Security
Infrastructure) addressed delegation authority directly ([RFC
2693](https://www.rfc-editor.org/rfc/rfc2693)). **[RFC
8693](https://www.rfc-editor.org/rfc/rfc8693)** (OAuth 2.0 Token Exchange, 2020) lets an
authorization server issue a token for a delegated call, with an `act` claim that can record the
chain of actors. It does not require the new token to be narrower; that is left to the server's
policy. Prior actors in nested `act` claims are informational and are not used for access control,
so the chain is recorded, not enforced.

The agent case is also already being worked: individual IETF drafts propose attenuated tokens or
delegation chains for agents ([Niyikiza
2026](https://datatracker.ietf.org/doc/draft-niyikiza-oauth-attenuating-agent-tokens/); [Hassan
2026](https://datatracker.ietf.org/doc/html/draft-hamr-oauth-agent-delegation-02)). Delegation that
only narrows is therefore a well-specified property with deployed implementations. Authority that
cannot be relayed needs more than attenuation. Macaroons and exchanged tokens are bearer
credentials: whoever holds one can present it. Binding a credential to its holder takes
sender-constrained tokens, such as mutual-TLS-bound tokens ([RFC
8705](https://www.rfc-editor.org/rfc/rfc8705)) or DPoP ([RFC
9449](https://www.rfc-editor.org/rfc/rfc9449)). What this method does with these properties is
require them of agent sessions.

### Harness engineering

The method's principle that trust comes from layered controls rather than agent behaviour
([method.md](method.md) §1) names two places. Much of 2026's work sits in a third it does not name:
the harness, the set of mechanisms configured around a model. Galster et al. (*Harness Engineering
for Agentic AI Coding Tools: An Exploratory Study*,
[arXiv:2602.14690](https://arxiv.org/abs/2602.14690)) study how coding tools are configured in
practice: context files, skills, subagents, commands, rules, settings, hooks and tool servers.
Enforcement before a call executes is better represented by [Madatha's
work](#deterministic-enforcement-madatha), in the next section.

This matters for one specific claim. The method's account of rules lost through compaction
(method.md §1, principles 6 and 7; §3) describes a harness property, which is therefore reachable
by harness changes -- longer effective context, non-compactable pinned context, or re-injecting
the rule set per turn. Whether those are sufficient is an open question. They should not be
excluded by classification.

**Self-improving harnesses.** A practitioner suggested that NVIDIA's
[NOOA](https://github.com/NVIDIA-NeMo/labs-OO-Agents) agent framework could fit the method better
than Claude Code, and called the self-improving harnesses
[Prime Agent](https://github.com/PrimeIntellect-ai/prime-agent) and
[Harness²](https://github.com/google-research/harness2) incompatible with it. The method needs the
features in [What the tool must provide](control-reference.md#what-the-tool-must-provide), not one
tool; NOOA checks model-written code before running it and leaves permissions and containment to
the deployment. Prime Agent edits prompt notes, skills and subagents over an immutable base prompt,
with rollback; Harness² also edits guardrails and plugins, by default through a separate editor
model. Where such a change lands, existing rules apply: agent instruction files in the repository
reach the owner as the diff (L5); skills are agent tooling from an owner-approved list, pinned from
Standard (L3); enforcement material is read-only to the session (L3). Memories a tool keeps for
itself are an open question. [RegenHarness](https://arxiv.org/abs/2609.27612)'s updater cannot
weaken its own acceptance gate; its paper does not state who authorizes a release.

### Deterministic enforcement: Madatha

Madatha's *A Deterministic Control Plane for LLM Coding Agents*
([arXiv:2606.26924](https://arxiv.org/abs/2606.26924), June 2026) governs the configuration
layer of IDE coding harnesses. Every mechanism is ordinary testable code -- hashing, regex, a
state machine, set arithmetic -- explicitly not delegated to further LLM orchestration. Five
permission tiers are enforced as errors at install and transform time, so they fail closed;
every tier except the unrestricted orchestrator tier is a strict tool allowlist. One tier adds
write-path validation, an attack-derived shell blocklist runs on pre-tool-use hooks for two
harnesses, and git configuration and hook manipulation are forbidden by prompt rules backed by
blocklist patterns.

Madatha reached several positions this method holds, and should be credited with them: enforcement
in code rather than in agent compliance, fail-closed permission sets per role, and mechanisms
tested by injecting what they must reject. Its integrity checks re-hash agent definitions at
install time and detect accidental or automated tampering through a keyed lockfile; the paper
notes that a process running as the same user could forge the lockfile stamp.

**More than enforcement.** The paper includes a phase state machine with named delegation
receipts, human-in-the-loop gates after specific phases, and a capped escalation to a human after
three failed auto-fix iterations by default. A gated phase machine with delegation records and
human escalation points is an authority structure, not only an enforcement primitive.

What remains different is scope rather than category. It governs agent definitions and harness
configuration across an organisation's repositories, compiled to seven IDE targets. Its runtime
enforcement is pre-tool-use hooks in two of them, Claude Code and Cursor; the other targets get
compile- and install-time checks only. It states that it is not building a sandbox, addresses no
environment isolation, and describes no path for widening an agent's tier. It names bounded
delegation depth as a mitigation but describes no mechanism for it. Its phase-gate overhead is
explicitly unmeasured.

**On its prevalence study.** The sub-1% permission-declaration figure is labelled indicative in
the paper, produced by a fragile parser, with n=31 true positives of 6,145 files, and is not
the study's headline finding. The headline finding is that agent configurations propagate as
undeclared shared components: 10.1% of tracked paths are exact duplicates across independent
repositories, after adjusting for forks. A second figure, 75.5% of clone pairs crossing
organizational boundaries, is computed on raw pairs without fork filtering. Two further
qualifiers for anyone citing it: the sample is 10,008 *public* repositories, which is not the
enterprise population, and no base rate for configuration-file cloning is offered, so a
cross-organization clone may be a widely adopted good default rather than an ungoverned copy.

### Graduated human oversight: GAIE

Kang's *Governed AI-Assisted Engineering* ([arXiv:2606.22484](https://arxiv.org/abs/2606.22484),
June 2026) routes each code generation task to human-in-the-loop, human-over-the-loop or
automated-with-monitoring. The paper frames the routing around four risk dimensions: regulatory
impact, customer proximity, reversibility and data sensitivity. Its decision function also takes a
confidence score against a threshold, and checks for a dependency-graph path to a strategic
function. Each tier names its required evidence, held append-only with a hash chain. GAIE also
states that uncertain classification escalates to a higher oversight tier, never a lower one.

GAIE reached positions this method holds, and should be credited with them: that oversight must
be scaled to risk rather than applied uniformly, and that the agent generating code cannot approve
its own deployment. In this method the second corresponds to the first principle: the owner holds
every merge and release, at every profile ([method.md](method.md) §1, principle 1).

GAIE's tiering is the closest prior formulation of risk-tiered escalation for code generation;
risk-proportionate human oversight as such predates it. This method composes it across multiple
levels of delegation, an increment on GAIE rather than a separate position.

**One qualifier when citing it.** GAIE's velocity-preservation figures are an analytical model
built on assumed per-tier inputs, not measurements, and the paper says its evaluation is
analytical with no production deployment data. Anyone contrasting measured against unmeasured
oversight cost should note that both sides of that contrast are currently unmeasured.

### Enterprise control architecture: CASE

The CASE framework (Telukunta, Lilis & Baron, [arXiv:2608.10153](https://arxiv.org/abs/2608.10153),
August 2026) is a four-layer control architecture for enterprise agentic AI, with a five-level
maturity model and an assessment instrument. Its third layer applies supervisory cybernetics to
human-agent teams: tiered autonomy, federated oversight loops and role-bound override rights. It is
the nearest recent treatment of how humans supervise agent teams, though not of team structure,
which the [organizational models](#multi-agent-organizational-models) above treat more directly. It
governs enterprise agent operations generally rather than software delivery; coding agents are not
addressed. It reports three studies on public data: 62 incidents, 22 agent-operations tools and 35
enterprise deployments. The pipeline that coded the data for those studies is released under the MIT
licence.

## Isolation, evidence and standards

The remaining work is settled practice the method adopts, measured evidence its choices follow,
or frameworks a reader may map it onto.

### Environment isolation

The techniques in L1 and L2 are not novel and are not claimed as such. Vendor and practitioner
guidance has converged on the same answers: a worktree is not a security boundary, egress
should be default-deny with an allowlist, DNS is an exfiltration path, metadata endpoints and
private ranges should be blocked, credentials belong outside the sandbox, and isolation strength
runs microVM over user-space kernel over container.

This method arranges those techniques as a baseline with profiles, raise-triggers, recorded
residual risks and a stated proof per control.

### When multiple agents help

Practitioner and research work since 2024 converges on one point: more agents is not better by
default, and whether they help depends on how the work decomposes.

Anthropic's guidance on
[building agents](https://www.anthropic.com/engineering/building-effective-agents) (2024)
recommends "finding the simplest solution possible, and only increasing complexity when needed",
and names the patterns this method's execution patterns draw on: routing, orchestrator-workers and
evaluator-optimizer. Its account of a
[multi-agent research system](https://www.anthropic.com/engineering/multi-agent-research-system)
(2025) reports roughly fifteen times the tokens of a chat interaction for a multi-agent setup,
gains on breadth-first research, and the observation that most coding tasks are less
parallelisable than research. It also stresses detailed delegation briefs and effort scaled to
complexity.

Kim et al. measured, in the first version of their study (2025,
[arXiv:2512.08296](https://arxiv.org/abs/2512.08296)) and its
[blog](https://research.google/blog/towards-a-science-of-scaling-agent-systems-when-and-why-agent-systems-work/)
(2026), 180 configurations across four benchmarks and three model families; the current version
(2026) reports 260 configurations across six benchmarks. In the first version, multi-agent
coordination improved a parallelisable task by up to 81% and degraded a sequential one by 39-70%.
Errors were amplified 17.2 times with independent agents and 4.4 times with a centralised
orchestrator, measured on traces. Task decomposability and tool count predicted the best
architecture for most unseen configurations. This method's rule of choosing a pattern by coupling
rather than size, its default to one agent for sequential work, and its routing of workers through
one orchestrator follow the same direction. Their study did not test multi-level hierarchies, so it
neither supports nor contradicts patterns P5 and P6 (function teams and matrix,
[method.md](method.md) §2.1).

Cognition's two posts trace a practitioner's position over ten months. The
[first](https://cognition.com/blog/dont-build-multi-agents) (2025) argued against multi-agent
systems: agents need shared context, and "actions carry implicit decisions", so parallel agents
with different assumptions produce clashing work. The
[second](https://cognition.com/blog/multi-agents-working) (2026) reports what now works in their
product: writes kept single-threaded, reviewers given a clean context, escalation to a stronger
model that receives the primary's context, and manager-child delegation. Parallel writers into
shared work still do not. This method's project team runs workers in parallel on separate branches
with orchestrator-owned integration, which is the manager-child shape; its assumptions field and
clean-context reviewers address the same failure modes. Its tier escalation differs: the stronger
tier starts fresh from the last reviewed commit rather than receiving the failing context. Both
posts rest on internal, mostly anecdotal evidence.

Cemri et al. (2025, [arXiv:2503.13657](https://arxiv.org/abs/2503.13657)) built a taxonomy of
fourteen multi-agent failure modes in three categories (system design, inter-agent misalignment,
task verification) from 150 traces, and applied it to more than 1,600 annotated traces. The method's
learning loop ([method.md](method.md) §11) can tag incidents with these categories, so that one
practice's incidents can be compared with published failure data.

### Calibrated decision models

The decision classifier ([control-reference.md](control-reference.md), L6) and the action
classifier (L3) assume a model that returns a confidence over typed questions, with thresholds
fitted from a target error rate. Two 2026 preprints describe that class of model, and the first
shows why the thresholds have to be fitted locally.

Guo et al. ([arXiv:2609.29429](https://arxiv.org/abs/2609.29429)) evaluate a model trained with
reinforcement learning for calibrated decisions (RLCD) as a zero-shot detector across ten
alignment-failure types, prompt injection, deception, reward hacking and power seeking among them.
They report a median AUROC (area under the ROC curve) of 0.886 from one generic question. Their
benchmarks include agent trajectories in which a tool result carries an injected instruction
(InjecAgent, AUROC 0.988 with the generic question), so detecting injection that arrives through
tool output is partly their result. They do not evaluate inter-agent messages; applying the model
there is this method's extrapolation. The model's probabilities are calibrated when pooled across
benchmarks but not within one, and a threshold fitted on a few local labels recovers most of the F1
gap on benchmarks with rule-based labels, which is why the method fits thresholds locally. They also
find that context matters more than question wording, mostly through fields that encode the label.
That finding is not about who writes the context, so the rule that the action classifier takes its
context only from trusted sources is the method's own design choice, not their result.

Gao and Wang ([arXiv:2609.38850](https://arxiv.org/abs/2609.38850)) publish an open RLCD training
recipe ("calibrate, then reinforce") and report selective prediction: the share of items decided
alone at a bounded error rate. That is the form in which this method sets classifier thresholds
and records coverage. Their evaluation uses one 1.7B model on two reasoning tasks, math answer
verification and knowledge-heavy multiple choice, not security. On a separate dataset of
annotator disagreement, they show that the RLCD objective gains nothing over cross-entropy
training when the uncertainty comes from the labels themselves. The method's choice to decide
scope deterministically does not rest on that result: scope has a deterministic answer in the
slice plan, so it is not left to a model.

Neither paper is evidence that the controls built on these models work in this setting. The
method requires local calibration per decision point and recorded error rates for that reason.

### Standards and frameworks

Security leaders usually map a new method onto the frameworks they already report against. This
section is placement for that, not a compliance claim. The method's layers touch familiar control
domains: environment segregation (L1, L2), access control and segregation of duties (L4), change
management (L5, L7), logging and audit (L8), and supply-chain integrity (L3, and L5 at High).
NIST's [Secure Software Development Framework](https://csrc.nist.gov/pubs/sp/800/218/final) (SSDF)
and [SLSA](https://slsa.dev/) cover the build and supply-chain side. OWASP's
[guidance on LLM and agentic applications](https://genai.owasp.org/llm-top-10/) covers threats to
agent sessions. NIST's [AI Risk Management Framework](https://doi.org/10.6028/NIST.AI.100-1) and
[ISO/IEC 42001](https://www.iso.org/standard/81230.html) cover AI governance at the level of the
organisation. The method maps no clauses of these. For MITRE ATLAS, the OWASP Top 10 for Agentic
Applications and the SSDF, [standards-map.md](standards-map.md) shows which method controls relate
to each item; like this section, it is placement, not a compliance claim.

## A practitioner's reading

[Cruz (2026)](https://newsroom.sgit.ai/articles/governed-semi-autonomous-development-read-here.html)
read this method against a newsroom run by agents. It is one practitioner's self-assessment of his
own setting against the method, not an audit, and its evidence is that one setting.

It reports independent convergence on several principles: authority stays with a person for
outward actions; the owner receives decisions, not files; a rule that breaks twice becomes a
mechanism; rule text is not a boundary; messages carry information, never authority; and a
manifest is anchored on the owner's key. Arriving at the same ideas from a different starting
point is a signal that they are reachable, not evidence that they work.

The "Kept by" column of the [control reference](control-reference.md) adapts its framing of each
rule as what the agent can do, what it may do, the gap between the two, and the barrier that keeps
the gap closed. The article calls the first of these a grant. This method calls it the agent's
capability instead, because a grant here is something else: an owner-issued widening of authority.
"Capability" in this sense, what the agent can do, is distinct from the capability library and from
capability security's token of authority.

The article makes four other proposals. The method responds to two. Memory loss as a design input
corresponds to principle 7: limits are re-read from durable artefacts and re-checked, and the
restated limits are re-injected at intervals, modelled on the article's own status reports.
Encrypted transport corresponds to the end-to-end encrypted message option at High; the article
also proposes encrypted state, which the method does not adopt. Its proposal to govern agents
inside the product is recorded as out of scope ([method.md](method.md) §0). Its offer of measured
owner metrics, as a second evidence base, is not taken up.

The reading also found its own CI to be in-band: the agent that pushes can change the checks
that gate it, in the same push. That is the case the out-of-band CI rule addresses
([control-reference.md](control-reference.md), L5).

## Conclusion: an assembly for one setting

A fair question from anyone reading the sections above is why this method exists at all rather than
an adoption of [MOISE+](https://doi.org/10.1007/3-540-36127-8_12) with
[macaroons](https://www.ndss-symposium.org/ndss2014/macaroons-cookies-contextual-caveats-decentralized-authorization-cloud)
underneath. The honest answer is about setting rather than about priority.

- **[Hardy (1988)](https://doi.org/10.1145/54289.871709) is about an operating system.** A compiler,
  a directory, a billing file, one machine. It names the property -- authority attaching to the
  running program rather than to the request -- and nothing in it concerns a coordinating process
  that summarises a human's decision to a subordinate process in natural language, where the summary
  is lossy by nature rather than by bug.
- **MOISE+ and [Gaia](https://doi.org/10.1023/A:1010071910869) specify organizations of designed,
  cooperative agents.** Roles are filled by agents built to fill them. There is no human owner
  holding irreversible outward actions, no participant that loses its policy memory mid-task, and no
  commercial production risk attached to a role being played badly. The organizational formalism
  transfers; the threat model behind this method does not come from there.
- **Macaroons and [RFC 8693](https://www.rfc-editor.org/rfc/rfc8693) give attenuation, and the agent
  frameworks do not implement it.** A grant in a running agent harness today is typically a prompt
  answered once, and in CrewAI delegation is a tool the manager calls rather than a credential the
  receiver checks. That gap is where the agent setting differs, and the IETF agent-token drafts
  indicate others are closing it.
- **The orchestration frameworks structure work without governing authority.**
  [CrewAI](https://docs.crewai.com/) and [MetaGPT](https://arxiv.org/abs/2308.00352) establish
  roles, managers and document hand-off. CrewAI offers human review of a task's output, guardrails,
  and in Flows a human-feedback decorator for approval gates and review points. These are steps the
  developer's code places in a flow. Neither framework carries a human owner who holds irreversible
  outward actions as a separate authority, an assurance profile scaled to risk, or a measure of the
  owner's interrupts and waiting time during a run; MetaGPT measures the human revisions its output
  needs afterwards.
- **[Paperclip](https://github.com/paperclipai/paperclip) governs authority as configuration.** A
  human board approves strategy, budget overrides and, if set, hires; a hard budget cap pauses an
  agent; local agent tokens name their run and expire; a responsible user bounds every agent write
  and is recorded beside the acting agent; agent configuration can be rolled back and skills keep
  versions; and any gateway action can be set to ask first. Which actions are held, and whether
  agents may change their own instructions and skills, are the deployer's settings. This method
  fixes them: the owner holds every outward action at every profile, and changes to agent
  instruction files reach the owner as the diff from Lite.

None of that makes this method novel. It means the known properties have to be assembled for a
setting their originators were not addressing, and that assembly is what the method is: a
synthesis of eight layers, three profiles chosen independently per layer with explicit
raise-triggers, coherence rules, recorded accepted residual risks and a stated proof per control.
The constituent controls come from the bodies of work above. What is offered is their assembly
into something a single owner can adopt, designed so an auditor can hold it.

**A method that instruments the owner.** [method.md](method.md) §14 treats the owner's queue as a
thing to measure: owner interrupts against real decisions, owner-wait time as the sum of blocked
work waiting on a person, and fitting work left waiting while a fitting worker is idle. The
measures are defined here and have not been published from this practice, so they are a proposal
for what to collect rather than a reported result. A reader weighing the oversight-cost
literature should note that [Kang's](https://arxiv.org/abs/2606.22484) velocity figures are
modelled and [Madatha's](https://arxiv.org/abs/2606.26924) phase-gate overhead is explicitly
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

Grouped as the document is, and listed in the order of the sections that discuss them. Abbas et
al. is further reading on organizational models; Laya is cited from
[claude-code-reference.md](claude-code-reference.md).

**Organizing agents**

- Wooldridge, M., Jennings, N. R., & Kinny, D. (2000).
  [*The Gaia Methodology for Agent-Oriented Analysis and Design*](https://doi.org/10.1023/A:1010071910869).
  Autonomous Agents and Multi-Agent Systems, 3(3), 285-312.
- Zambonelli, F., Jennings, N. R., & Wooldridge, M. (2003).
  [*Developing Multiagent Systems: The Gaia Methodology*](https://doi.org/10.1145/958961.958963).
  ACM Transactions on Software Engineering and Methodology, 12(3), 317-370.
- Hannoun, M., Boissier, O., Sichman, J. S., & Sayettat, C. (2000).
  [*MOISE: An Organizational Model for Multi-agent Systems*](https://doi.org/10.1007/3-540-44399-1_17).
  IBERAMIA-SBIA 2000, LNAI 1952, 156-165.
- Hübner, J. F., Sichman, J. S., & Boissier, O. (2002).
  [*A Model for the Structural, Functional, and Deontic Specification of Organizations in
  Multiagent Systems*](https://doi.org/10.1007/3-540-36127-8_12).
  SBIA 2002, LNCS 2507, 118-128. Extended abstract:
  [*MOISE+: Towards a Structural, Functional and Deontic Model for MAS Organization*](https://doi.org/10.1145/544741.544858).
  AAMAS 2002, 501-502.
- Moise software distribution, `writePaper` example specification,
  [`examples/specifications/wp-os.xml`](https://github.com/moise-lang/moise/blob/main/examples/specifications/wp-os.xml),
  accessed 26 October 2026.
- Abbas, H. A., Shaheen, S. I., & Amin, M. H. (2015).
  [*Organization of Multi-Agent Systems: An Overview*](https://doi.org/10.11648/j.ijiis.20150403.11).
  International Journal of Intelligent Information Systems, 4(3), 46-57. Preprint
  [arXiv:1506.09032](https://arxiv.org/abs/1506.09032) [cs.MA]. A survey entry point to the
  organizational-model literature.
- Graicunas, V. A. (1933). *Relationship in Organization*. Bulletin of the International
  Management Institute. Reprinted in Gulick, L., & Urwick, L. (Eds.) (1937). *Papers on the
  Science of Administration*. Institute of Public Administration, Columbia University.
- Urwick, L. (1956). *The Manager's Span of Control*. Harvard Business Review.
- CrewAI. [*CrewAI documentation*](https://docs.crewai.com/): Processes, Hierarchical Process,
  Collaboration, Agents, Tasks, Flows, accessed 26 October 2026.
- Hong, S., et al. (2024).
  [*MetaGPT: Meta Programming for A Multi-Agent Collaborative Framework*](https://arxiv.org/abs/2308.00352).
  ICLR 2024. arXiv:2308.00352.
- paperclipai. [*Paperclip*](https://github.com/paperclipai/paperclip). Software, v2026.1023.0, MIT.
  With the [Paperclip documentation](https://docs.paperclip.ing): what Paperclip is, approvals,
  execution policy, the connector access model, action permissions, review requests, the GitHub
  connector and recipe, agents, skills, roles and permissions, activity log, experimental
  environments and isolated workspaces, trust presets, and the Claude Code adapter, accessed
  26 October 2026.
- Open GSD. [*GSD Core*](https://github.com/open-gsd/gsd-core). Software, v1.18.0 (released
  19 October 2026), branch `next`, accessed 26 October 2026.
- TÂCHES. [*Get Shit Done*](https://github.com/gsd-build/get-shit-done). Software, archived
  repository that points to GSD Core, accessed 26 October 2026.
- GitHub. [*Spec Kit*](https://github.com/github/spec-kit). Software, branch `main` (release
  v1.2.3), accessed 26 October 2026.
- bmad-code-org. [*BMAD Method*](https://github.com/bmad-code-org/BMAD-METHOD/tree/v6.12.4).
  Software, v6.12.4 (stable line; a v7 preview is on the default branch), accessed
  26 October 2026.
- Tan, L. [*pstack*](https://github.com/cursor/plugins/tree/main/pstack). Cursor plugin, v0.15.32,
  MIT. In *cursor/plugins*, folder `pstack/`, accessed 26 October 2026.
- Vincent, J., and Prime Radiant. [*Superpowers*](https://github.com/obra/superpowers). Software,
  v7.0.1, MIT, accessed 26 October 2026. Announced in
  [*Superpowers: How I'm using coding agents in October 2025*](https://blog.fsck.com/2025/10/09/superpowers/),
  blog.fsck.com, 9 October 2025.
- Fission AI. [*OpenSpec*](https://github.com/Fission-AI/OpenSpec). Software, branch `main` at
  commit 6d04a1b (latest release v1.15.0), accessed 26 October 2026.
- Kiro. [*Kiro documentation*](https://kiro.dev/docs/specs/): Specs, Feature Specs, Correctness,
  Hooks, accessed 26 October 2026.

**Authority and enforcement**

- Hardy, N. (1988).
  [*The Confused Deputy (or why capabilities might have been invented)*](https://doi.org/10.1145/54289.871709).
  ACM SIGOPS Operating Systems Review, 22(4), 36-38.
- Birgisson, A., et al. (2014).
  [*Macaroons: Cookies with Contextual Caveats for Decentralized Authorization in the Cloud*](https://www.ndss-symposium.org/ndss2014/macaroons-cookies-contextual-caveats-decentralized-authorization-cloud).
  NDSS 2014.
- Ellison, C., et al. (1999). [*SPKI Certificate Theory*](https://www.rfc-editor.org/rfc/rfc2693).
  RFC 2693.
- Jones, M., et al. (2020). [*OAuth 2.0 Token Exchange*](https://www.rfc-editor.org/rfc/rfc8693).
  RFC 8693.
- Campbell, B., et al. (2020).
  [*OAuth 2.0 Mutual-TLS Client Authentication and Certificate-Bound Access Tokens*](https://www.rfc-editor.org/rfc/rfc8705).
  RFC 8705.
- Fett, D., et al. (2023).
  [*OAuth 2.0 Demonstrating Proof of Possession (DPoP)*](https://www.rfc-editor.org/rfc/rfc9449).
  RFC 9449.
- Niyikiza, N. A. (2026).
  [*Attenuating Authorization Tokens for Agentic Delegation Chains*](https://datatracker.ietf.org/doc/draft-niyikiza-oauth-attenuating-agent-tokens/).
  Internet-Draft draft-niyikiza-oauth-attenuating-agent-tokens-01, individual submission.
- Hassan, A. (2026).
  [*An Attenuated Delegation Profile for Automated Agents*](https://datatracker.ietf.org/doc/html/draft-hamr-oauth-agent-delegation-02).
  Internet-Draft draft-hamr-oauth-agent-delegation-02, individual submission, 19 September 2026.
  Revision -03 (22 October 2026) keeps the narrowing-only delegation chain.
- Galster, M., Mohsenimofidi, S., Lulla, J. L., Abubakar, M. A., Treude, C., & Baltes, S.
  (2026).
  [*Harness Engineering for Agentic AI Coding Tools: An Exploratory Study*](https://arxiv.org/abs/2602.14690).
  arXiv:2602.14690 [cs.SE]. Based on a paper in the Proceedings of AIware 2026.
- Furgale, P., et al. (2026).
  [*NVIDIA-labs OO Agents: Native Python Object-Oriented Agents*](https://arxiv.org/abs/2607.20709).
  arXiv:2607.20709 [cs.AI]. Software:
  [NVIDIA-NeMo/labs-OO-Agents](https://github.com/NVIDIA-NeMo/labs-OO-Agents), accessed
  26 October 2026.
- Karten, S., et al. (2026).
  [*Prime Agent: A Self-Improving RLM Harness*](https://arxiv.org/abs/2608.23552). arXiv:2608.23552
  [cs.AI], technical report. Software:
  [PrimeIntellect-ai/prime-agent](https://github.com/PrimeIntellect-ai/prime-agent), accessed
  26 October 2026.
- Xu, R., et al. (2026).
  [*Harness^2: Recursive Agent Harnessing for an Open World*](https://arxiv.org/abs/2610.14224).
  arXiv:2610.14224 [cs.AI]. Software:
  [google-research/harness2](https://github.com/google-research/harness2), accessed 26 October 2026.
- Wang, K., et al. (2026).
  [*RegenHarness: A Robot Agent Harness with Evidence-Gated Recursive Self-Improvement*](https://arxiv.org/abs/2609.27612).
  arXiv:2609.27612 [cs.RO].
- Madatha, P. (2026).
  [*A Deterministic Control Plane for LLM Coding Agents*](https://arxiv.org/abs/2606.26924).
  arXiv:2606.26924 [cs.SE].
- Kang, R. (2026).
  [*Governed AI-Assisted Engineering: Graduated Human Oversight for Agentic Code Generation in
  Regulated Domains*](https://arxiv.org/abs/2606.22484).
  arXiv:2606.22484 [cs.HC].
- Telukunta, S., Lilis, G. N., & Baron, L. (2026).
  [*The CASE Framework: A Multi-Disciplinary Control Architecture for Governing Enterprise Agentic
  AI*](https://arxiv.org/abs/2608.10153).
  arXiv:2608.10153 [cs.AI].

**Isolation, evidence and standards**

- Anthropic (2024).
  [*Building effective agents*](https://www.anthropic.com/engineering/building-effective-agents).
  Anthropic Engineering, 19 December 2024.
- Anthropic (2025).
  [*How we built our multi-agent research system*](https://www.anthropic.com/engineering/multi-agent-research-system).
  Anthropic Engineering, 13 June 2025.
- Kim, Y., Gu, K., Park, C., et al. (2025).
  [*Towards a Science of Scaling Agent Systems*](https://arxiv.org/abs/2512.08296).
  arXiv:2512.08296 [cs.AI] (v1 December 2025; v3 April 2026); summarised on the
  [Google Research blog](https://research.google/blog/towards-a-science-of-scaling-agent-systems-when-and-why-agent-systems-work/),
  28 January 2026.
- Yan, W. (2025). [*Don't Build Multi-Agents*](https://cognition.com/blog/dont-build-multi-agents).
  Cognition, 12 June 2025.
- Yan, W. (2026).
  [*Multi-Agents: What's Actually Working*](https://cognition.com/blog/multi-agents-working).
  Cognition, 22 April 2026.
- Cemri, M., Pan, M. Z., Yang, S., et al. (2025).
  [*Why Do Multi-Agent LLM Systems Fail?*](https://arxiv.org/abs/2503.13657) arXiv:2503.13657
  [cs.AI].
- Guo, R., Liu, Y., Deng, G., Li, Y., Zhao, L., Wu, Y., Chen, S., Zhang, Y., & Zhang, L. Y.
  (2026).
  [*Just Ask Jev: Reinforcement Learning for Calibrated Decisions as a Zero-Shot Detector of AI
  Alignment Failures*](https://arxiv.org/abs/2609.29429).
  arXiv:2609.29429 [cs.AI].
- Gao, Z., & Wang, P. (2026).
  [*OpenJev-RLCD: A Working RLCD Implementation*](https://arxiv.org/abs/2609.38850).
  arXiv:2609.38850 [cs.AI].
- NandhaKishorM. [*Laya*](https://github.com/NandhaKishorM/laya). Software, Apache-2.0. An example
  decision engine for the Claude Code reference mapping.
- NIST (2022).
  [*Secure Software Development Framework (SSDF) Version 1.1: Recommendations for Mitigating the
  Risk of Software Vulnerabilities*](https://csrc.nist.gov/pubs/sp/800/218/final).
  SP 800-218.
- OpenSSF. [*Supply-chain Levels for Software Artifacts (SLSA)*](https://slsa.dev/). slsa.dev.
- OWASP GenAI Security Project.
  [*Top 10 for LLM Applications*](https://genai.owasp.org/llm-top-10/) and agentic AI security
  guidance. genai.owasp.org.
- NIST (2023).
  [*Artificial Intelligence Risk Management Framework (AI RMF 1.0)*](https://doi.org/10.6028/NIST.AI.100-1).
  NIST AI 100-1.
- ISO/IEC 42001:2023.
  [*Information technology -- Artificial intelligence -- Management system*](https://www.iso.org/standard/81230.html).

**A practitioner's reading**

- Cruz, D. (2026).
  [*Governed semi-autonomous development, read in a newsroom run by agents: where Levente Simon's
  method and our work arrived at the same ideas, how we measure up to it, and what we would add*](https://newsroom.sgit.ai/articles/governed-semi-autonomous-development-read-here.html).
  newsroom.sgit.ai, 21 October 2026. Read at v0.7 of this method; accessed 22 October 2026.

The 2026 arXiv works above are preprints. None states a venue at the time of writing, except
the harness-engineering study, which is based on a paper in the Proceedings of AIware 2026.
