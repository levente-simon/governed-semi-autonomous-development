# Governed semi-autonomous development: Glossary

*The terms used across the method's documents, in alphabetical order. The rules are in [method.md](method.md), the procedures in [practice.md](practice.md), the controls in [control-reference.md](control-reference.md), and one tool mapping in [claude-code-reference.md](claude-code-reference.md).*

---

| Term | Definition |
|---|---|
| **Action classifier** | At High, a calibrated decision model that checks side-effecting tool calls the rules allowed, for misuse of a permitted tool ([control-reference.md](control-reference.md), L3) |
| **Agent PR identity** | A host identity that can open pull requests and nothing more; it opens each vehicle's PR from the branch the owner pushed ([control-reference.md](control-reference.md), L5) |
| **Baseline** | Authority set at kickoff in each environment: pinned orchestrator, what it may assign, the session's limits. Owner-signed (High), owner-applied (Standard), or at Lite the owner's own settings in the one environment |
| **Blind judge** | A specialist that evaluates one candidate without seeing alternatives |
| **Board / log** | The two parts of the ledger: current state (rewritten) and history (append-only). The log is rotated per vehicle or checkpoint into **log segments** |
| **Brief** | The written scope, environment constraints, routing rules and slice routine an agent works from: a common brief and one per role ([method.md](method.md), §3) |
| **Build mode / release mode** | The initiative's working mode, and the mode one release vehicle enters until it is published or rolled back ([method.md](method.md), §4) |
| **Capability** | A pre-built, versioned production line with a contract ([method.md](method.md), §7). In the "Kept by" reading of the control reference, an agent's capability means what it can do, as against its **mandate** |
| **Checkpoint** | A point where integrated work is reviewed as a whole; every merge vehicle closes one |
| **Complexity-and-risk class** | The plan's tag on a slice. It sets the tier the slice runs at ([method.md](method.md), §10) and whether the slice is **high-risk**, which [control-reference.md](control-reference.md) (L7) defines. Any reviewer can raise the class; only the owner lowers it |
| **Coverage** | The share of items a decision or action classifier decides on its own at the target error rate, measured on the calibration set. The rest goes to review |
| **Decision classifier** | A self-hosted, locally calibrated model that sorts messages at the gateway and external content at ingestion into pass, quarantine or block. An extra filter after the deterministic checks; never the boundary, never a source of authority |
| **Decision engine / calibration set** | The self-hosted model behind the action and decision classifiers / the versioned, labelled set its thresholds are fitted on, one per decision point ([control-reference.md](control-reference.md), L6) |
| **Decision message** | One decision for the owner: a TL;DR, lettered options and a recommendation with reasons ([method.md](method.md), §10) |
| **Decision register** | The record of every owner decision, numbered, with options, choice and rationale; for an exception, also its acceptor and review date |
| **Dedicated development machine** | The Lite option of one machine set aside for the agents; its requirements are in [control-reference.md](control-reference.md) (L1, Why sessions are separated) |
| **Delegation manifest** | The record of who granted what to whom, with scope and expiry |
| **Documentation chain** | The sequence of functional specification, technical specification with gap analysis, implementation plan, slice execution and close-out, each with a review gate and a decision step. Its depth follows the pattern ([practice.md](practice.md), §1) |
| **Documentation fold** | Folding working documents into durable architecture, user and agent-instruction documents at each checkpoint and vehicle ([practice.md](practice.md), §6) |
| **Enforcement material** | What enforces a session's limits and must be unwritable by it: permission settings, hooks, the trust anchor, the grant manifest, revocation records ([control-reference.md](control-reference.md), L3) |
| **Environment authority** | The mandate for any action that changes what another session, or a later session in the same environment, gets by default, or that acts on a store or resource outside the environment. Authority in the L4 sense, assigned from Standard only at kickoff as a planned delegation, to a session in its own environment for that environment's lifetime; never by grant, never to the orchestrator, and at Lite to no session. Every other environment change goes to the owner. A session that can take such an action (its capability) does not hold environment authority unless assigned it (its mandate). What counts is in [control-reference.md](control-reference.md) (L1, What counts as environment authority) |
| **Equal authority** | The same role permission set. Tiers do not change it |
| **Exception** | A recorded decision to run below a required control, with a named acceptor and a review date ([method.md](method.md), §12) |
| **Execution pattern** | How the work is organised (P1 single session to P6 matrix, [method.md](method.md), §2.1); independent of the assurance profile |
| **External ingestion** | Assigned work whose input comes from outside the repository and its locked dependency set, such as fetching, research or triage of external material ([control-reference.md](control-reference.md), L1) |
| **Gateway / ingestion point** | The named points where the decision classifier runs: the federation gateway for peer messages, and fetch tools and capability inputs for external content |
| **Grant** | An owner-issued widening of authority beyond the baseline: at Standard a permission-prompt answer, at High a signed, instance-bound entry ([method.md](method.md), §5, L4) |
| **Grantable action class** | An action class on the closed list fixed at kickoff, for which the owner may issue a grant ([control-reference.md](control-reference.md), L4) |
| **Hand-over note** | The worker's committed account after work: changes, assumptions and decisions taken, tests with excerpted output, open items ([method.md](method.md), §3) |
| **High-risk slice** | See **complexity-and-risk class** |
| **Incident** | An event where a control failed, a rule was broken, or a defect escaped a stage that should have caught it ([method.md](method.md), §11; record in [practice.md](practice.md), §9) |
| **Independent verifier** | For a sole owner at High, the second party of the two-party release rule: an agent session in a separate trust domain that rebuilds its own evidence and signs a required verdict ([control-reference.md](control-reference.md), L4) |
| **Ingestion environment** | The environment of a session assigned external ingestion, apart from the other sessions from Standard ([control-reference.md](control-reference.md), L1) |
| **Initiative workspace** | One repository folder per initiative, moving through states such as backlog, active and done, holding the specifications, decision register, plan, checkpoint records and status |
| **Integration branch** | The orchestrator-owned branch that slices merge into; cut fresh from main for each merge vehicle ([control-reference.md](control-reference.md), L5) |
| **Kept by** | What keeps the gap between an agent's capability and its mandate closed: an **expectation** (a rule in a brief, instruction or checklist that the agent is expected to follow), a **setting** (tool or host configuration that refuses the action) or a **boundary** (an environment, system or party the session cannot reach or change) ([control-reference.md](control-reference.md), How to read this reference) |
| **Kickoff manifest** | At High, across environments, the owner-signed baseline for each environment, verified against the trust anchor |
| **Kickoff record** | The committed results of the kickoff tests and of every later re-check, with the profile per layer |
| **Lane / slice** | A parallel stream of work / a unit one worker completes and hands over |
| **Ledger** | The orchestrator ledger: the initiative's board and log on their own ref, its recovery point and its history ([method.md](method.md), §3) |
| **Leftover scan / sweep** | A search for debug code, stale flags, temporary bypasses, private references and working-document references before work closes. The scan runs in every pre-PR gate; the sweep is the full search at every merge vehicle (Standard) |
| **Lens / performer** | A specialist that reviews from one view / that produces one kind of output |
| **Mandate** | What an agent may do, given its role, its baseline and any grant. Compared with its capability, what it can do ([control-reference.md](control-reference.md), How to read this reference) |
| **Merge vehicle** | One PR to main carrying a defined set of slices. It closes a checkpoint and does not enter release mode |
| **Message audit** | The record of messages written by the transport, holding metadata and hashes, not bodies ([control-reference.md](control-reference.md), L6, L8) |
| **Oracle** | An independent implementation built from the decision table only |
| **Owner-attention budget** | The rules that keep owner interruptions to real decisions and batched chores ([method.md](method.md), §10) |
| **Ownership partition** | The plan's assignment of files, services and resources to agents, so that no two agents edit the same thing |
| **Planned compaction** | Context reduction at a safe point, after a brief for the compacted session is written ([practice.md](practice.md), §7) |
| **Planned delegation** | An authority the owner gives at kickoff, translated into the tool's configuration and exercised once ([control-reference.md](control-reference.md), L4) |
| **Plan segment** | A part of a plan with its own execution pattern, where patterns compose ([method.md](method.md), §2.1). Not a log segment |
| **Pre-PR gate** | The ritual that runs the checks the profile requires before a vehicle's PR opens |
| **Profile** | Lite, Standard or High: the controls required per layer |
| **Proof: independent / self-attested** | Evidence produced and stored outside every agent environment it describes / evidence written by an agent session ([control-reference.md](control-reference.md), How to read this reference) |
| **Protected path** | A path whose changes reach the owner as the diff, such as permission sets, hooks, CI workflows and what they invoke, and agent instruction files ([control-reference.md](control-reference.md), L5) |
| **Quarantine** | Holding a classifier-flagged item for batch review by an output-only reviewer, stored outside every agent's read path |
| **Re-check** | The start self-check run again after a restart or resume, after a protected-path merge, and at each vehicle ([practice.md](practice.md), §2) |
| **Recovery / pause protocol** | Rebuilding a session's picture from the ledger after a restart / stopping safely for usage limits, absence or the night ([practice.md](practice.md), §7) |
| **Red-first** | A fix that starts with a test failing on the defect |
| **Refute pass** | An attempt by another agent to show that a finding is wrong, before the finding becomes work |
| **Re-injection** | Putting the restated limits from the ledger back into a session's context at set intervals ([practice.md](practice.md), §7) |
| **Relayed** | Passed on without the owner's signature. An orchestrator carrying a signed grant is not relaying |
| **Release kit / run sheet** | The rehearsed, gated release scripts / the steps and evidence for one release vehicle |
| **Release vehicle** | One tag or publish run carrying a defined set of merged changes. It enters release mode |
| **Resource registry** | The shared record assigning ports, stacks, environments, lab slots and branches |
| **Ritual** | A codified, repeatable workflow that executes a control in full for its profile, the same way every time |
| **Roles** | Owner, orchestrator, worker, specialist, capability and peer ([method.md](method.md), §2) |
| **Shared hot files** | Files every lane touches: lockfiles, workspace manifests, version files, changelogs and generated indexes. The orchestrator owns them |
| **Shared worker environment** | From Standard, one environment shared by workers of equal authority, a worktree each ([control-reference.md](control-reference.md), L1, L2) |
| **Slice ledger** | A worker's record of slice state and grant use, on its own branch, aggregated by the orchestrator |
| **Slice plan** | The worker's committed plan for one slice, approved before work ([practice.md](practice.md), §3) |
| **Start self-check** | The mechanised check a new session runs on an already-tested environment and launch configuration: identity, restated limits re-injected from the ledger, one denied action, one enforcement-material write, one credentialed action ([practice.md](practice.md), §2) |
| **Stop** | In tier escalation, a worker's report that it cannot complete the slice as planned. Pauses, usage limits, environment failures and returned plan corrections are not stops ([method.md](method.md), §10) |
| **Sustain mode** | The steady-state maintenance loop for a released product; outlives initiatives |
| **Tier** | A session's configuration: model and effort level. Set per slice from its complexity-and-risk class, as a launch parameter of the owning worker's session. A launch command that differs only in its tier counts as tested ([practice.md](practice.md), §2). A tier never changes permissions. The classifiers' decider models are not tiers |
| **Trust anchor** | The owner's public key, provisioned into each environment image at creation |
| **Trust domain** | The set of sessions that share an environment, its credentials and its enforcement material. Compromise of one is compromise of all |
| **Two-party rule** | At High, a release needs the owner and a second human, or for a sole owner an independent verifier ([control-reference.md](control-reference.md), L4) |
| **Vehicle** | A merge vehicle or a release vehicle |
