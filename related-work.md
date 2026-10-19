# Related work

*How this method relates to published work on governing AI coding agents, and where it
differs. Current as of October 2026. Corrections and additions are welcome as issues.*

The method was developed from delivery rather than from the literature, and the convergences
below were found afterwards. Where another work got somewhere first, it is said so here.

| Work | Governs | Unit of control | Evidence basis |
|---|---|---|---|
| **GAIE / OCM** (Kang, 2026) | Whether a human reviews a given change, and when | The individual code generation task | Analytical; no production data |
| **Deterministic Control Plane** (Madatha, 2026) | What a coding harness permits an agent to do | Tools, paths, shell commands, git operations | Conformance tests on injected violations |
| **CASE** (Telukunta, Lilis & Baron, 2026) | Enterprise agentic AI operations | The agent, the collective, the human-agent team, the fleet | Framework and maturity instrument |
| **This method** | How an owner runs agent teams on a product | The control layer, chosen per layer | One product, eight months, twelve releases |

## Graduated human oversight: GAIE

Richard Kang's *Governed AI-Assisted Engineering* ([arXiv:2606.22484](https://arxiv.org/abs/2606.22484),
cs.HC, June 2026) proposes an Oversight Classification Model that routes each code generation
task to one of three tiers -- human-in-the-loop, human-over-the-loop, or automated with
monitoring -- from four inputs: regulatory impact, customer proximity, reversibility and data
sensitivity. Each tier names the evidence it must produce, held in append-only storage with a
hash chain, with signed deployment authorisation in the two supervised tiers.

GAIE reached two of this method's positions first, and should be credited with them.

- **Oversight must be scaled to risk rather than applied uniformly.** Its §I-D states that
  "review everything" creates velocity collapse, with the productivity benefit "entirely
  consumed by review overhead". That is the same argument as Principle 3 and Principle 7 here:
  efficiency matters because a bypassed control protects nothing.
- **Classification uncertainty escalates upward, never downward** (its P3), which is the same
  fail-closed posture this method takes at L3 and in delegation.

Its P4 -- "the agent that generates code cannot approve its own deployment" -- is an
independent arrival at the separation this method enforces through the two-party rule at
High L4.

**How this method differs.** The unit of control is different, and the difference is the
substance rather than a detail. GAIE tiers *the task*: given this change, how much human
review does it need. This method profiles *the control layer*: given this work, how strongly
is the infrastructure isolated, the session separated, the tool surface restricted, the
repository protected. The two axes are orthogonal and compatible -- a task tiered HITL by
GAIE could be executed in a session at Lite L1 -- and an organisation could reasonably apply
both.

GAIE also does not address what this method's L1, L2 and L6 cover. Its §I-C explicitly
excludes cybersecurity review and enterprise security architecture, and the development
environment appears only as pre-transmission data filtering and one mention of sandboxed
execution. Multi-agent operation is one architectural sentence and a generator/validator
separation; there is no delegation of authority, no concurrency, and no ownership model.
Oversight cost is argued qualitatively and modelled from assumed per-tier velocities; no
reviewer-load metric is defined.

Finally, GAIE states that its evaluation is analytical and that no production deployment data
is available, with expert validation pending. This method has the opposite profile: a single
product, which bounds what it can claim, but real delivery rather than analysis.

## Deterministic enforcement in the harness: Madatha

Padmaraj Madatha's *A Deterministic Control Plane for LLM Coding Agents*
([arXiv:2606.26924](https://arxiv.org/abs/2606.26924), cs.SE, June 2026) governs the
configuration layer of IDE coding harnesses. "Deterministic" is load-bearing in it: every
mechanism is ordinary testable code -- hashing, regex, a state machine, set arithmetic -- and
explicitly not delegated to further LLM orchestration. It defines five permission tiers
(readonly, scribe, operations, specialist, orchestrator), each a strict tool allowlist
enforced fail-closed at install and transform time, with write-path validation, an
attack-derived shell blocklist deployed as pre-tool-use hooks, and rules forbidding git
configuration and hook manipulation.

This is the closest published work to L3 and L4 here, and it arrived first at several
positions this method holds:

- **Enforcement belongs in code, not in agent compliance.** Its §1.2 refuses to delegate
  governance to an LLM. Principle 2 and Principle 6 here say the same thing from practice.
- **Fail-closed permission sets per role**, which is L4's role permission sets.
- **Enforcement material protected from the session** -- its rules forbidding writes to
  `.git/config`, `.git/hooks/` and `core.hooksPath` are the specific case of L3's requirement
  that enforcement material be read-only to the session.
- **Mechanisms are tested, including that they reject what they should**: its conformance suite
  injects integrity tampering, over-privileged declarations and blocklist strings. §11 here
  requires the same of any mechanism added by the learning loop.

**How this method differs.** Madatha governs one harness on one machine. It states that it is
not building a sandbox and not proposing agent coordination, and it addresses no environment
isolation -- no containers, VMs, worktrees, network egress or per-session credentials. It has
delegation *receipts* recorded between agents and a fixed set of human gates with a
three-iteration escalation cap, but no role and ownership model, no nested delegation, and no
approval routing. Phase-gate overhead is explicitly unmeasured.

Where that work stops at the configuration and hook layer, this method treats that layer as
L3 and L4 of eight, and specifies the layers around it: what the session runs inside, what it
may reach, how authority is granted and revoked, what the repository enforces, how messages
between sessions are authenticated, and what the audit trail must survive.

**On its prevalence study.** The paper reports that fewer than 1% of agent configuration
files declare permission boundaries against 33% of GitHub Actions workflow files. The paper
itself labels this indicative, produced by a fragile parser, with n=31 true positives of
6,145 files, and it is not the study's headline finding. The headline finding is that agent
configurations propagate as undeclared shared components: 10.1% of tracked paths are exact
duplicates across repositories, and 75.5% of clone pairs cross organisational boundaries.
Anyone citing either number should carry those qualifiers.

## Enterprise control architecture: CASE

The CASE framework (Telukunta, Lilis & Baron, [arXiv:2608.10153](https://arxiv.org/abs/2608.10153),
cs.AI, August 2026) is a four-layer control architecture for enterprise agentic AI --
control theory for the individual agent, complex adaptive systems for collectives,
supervisory cybernetics for human-agent teams, engineering operations for fleets at scale --
with a five-level maturity model and an assessment instrument.

Its third layer is the nearest published treatment of human-agent team structure. It governs
enterprise agent operations generally rather than software delivery; coding agents are not
addressed, and delegation is not a framework concept. It is useful here as the enterprise
frame into which a delivery method like this one would sit, not as an alternative to it.

## Environment isolation

The techniques in L1 and L2 are not novel and are not claimed as such. Vendor and
practitioner guidance -- NVIDIA's AI red team, JetBrains, and others -- has converged on the
same answers: a worktree is not a security boundary, egress should be default-deny with an
allowlist, DNS is an exfiltration path, metadata endpoints and private ranges should be
blocked, credentials belong outside the sandbox, and isolation strength runs microVM over
user-space kernel over container.

What is missing from that body of work is not the techniques but the shape: it exists as
scattered advice rather than as a baseline with profiles, raise-triggers, recorded residual
risks and a stated proof per control. L1 and L2 here are an attempt at that shape, built on
findings that are largely other people's.

## What appears to be unoccupied

Four positions in this method are not, as far as this survey found, held by published work.

1. **Agent delivery as an organisational design problem.** The literature treats it as a
   human-factors problem (how one person supervises one agent) or a controls problem (how a
   harness is restricted). This method treats it as organisation: span of control with a
   number on it, levels that cost latency and dilute intent, escalation that filters at each
   level, and named topologies through to a matrix.
2. **Delegation that nests and only narrows, with authority that is never relayed.** The
   failure mode in §2 -- "the owner said you may", sent by an orchestrator, is not a grant --
   appears in none of the works surveyed. It only becomes visible above one level of
   hierarchy.
3. **Owner attention as a measured quantity.** GAIE argues review overhead consumes the gain
   but defines no reviewer-load metric; Madatha states phase-gate overhead is unmeasured.
   §14 here defines owner interrupts against real decisions, owner-wait time as the sum of
   blocked worker time, and idle-worker time.
4. **Per-layer profile selection.** Published maturity models assign one level to an
   organisation or one tier to a task. Assurance profiles here are chosen independently per
   layer, from triggers, with coherence rules and recorded residual risk where a layer sits
   below its profile.

## Works cited

- Kang, R. (2026). *Governed AI-Assisted Engineering: Graduated Human Oversight for Agentic
  Code Generation in Regulated Domains*. arXiv:2606.22484 [cs.HC].
- Madatha, P. (2026). *A Deterministic Control Plane for LLM Coding Agents*.
  arXiv:2606.26924 [cs.SE].
- Telukunta, S., Lilis, G. N., & Baron, L. (2026). *The CASE Framework: A Multi-Disciplinary
  Control Architecture for Governing Enterprise Agentic AI*. arXiv:2608.10153 [cs.AI].
- Dhanorkar, S., Passi, S., & Vorvoreanu, M. (2026). *Human oversight of agentic systems in
  practice*. arXiv:2606.05391.
- OWASP GenAI Security Project. *State of Agentic AI Security and Governance* v2.01, and
  *Top 10 for Agentic Applications* (2026). https://genai.owasp.org/initiatives/agentic-security/

All three arXiv works above are preprints with no stated venue at the time of writing.
