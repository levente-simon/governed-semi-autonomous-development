# Changelog

Versions are dated and additive unless a change is marked **breaking**, which here means a
control was removed, renamed, or moved to a different assurance profile -- anything that
changes what an adopter has to do.

## v0.10.2 -- 2026-10-28

Added

- `standards-map.md`, a non-normative crosswalk from three external standards to the method's
  controls, for security leaders and auditors who arrive with their own framework: MITRE ATLAS
  (content release 2026.09), the OWASP Top 10 for Agentic Applications 2026, and NIST SP 800-218
  (SSDF 1.1), with a note on the SSDF 1.2 draft and a scope note on SP 800-218A. Each row names the
  related controls, linked to the control reference, a relation (Addresses, Partly, Related or
  Outside) with its profile, and a note in the method's own words. Addresses is used only where a
  setting or boundary is designed to stop or contain the item and no accepted residual undoes it
  at that profile. Every ATLAS technique in the release has a row or an out-of-scope entry. A key
  names the layers, describes the profiles and defines the terms the notes use. It is placement,
  not a compliance claim, a certification or an attestation, and not input for a self-attestation
  that rests on SP 800-218.
- Its entry rule: a standard from a recognised standards body (ISO/IEC, NIST) or a widely used
  community framework (MITRE, OWASP). Vendor frameworks are not included; organisation-level AI
  governance standards (NIST AI RMF, ISO/IEC 42001) are placed in `related-work.md` and not mapped;
  ISO/IEC 27090:2026 is a candidate not yet mapped.
- A "Gaps the crosswalk shows" section: the main gaps found across the three standards, merged,
  split into gaps inside the method's scope and items left to the adopter by design, and the open
  question whether an agent reviewer counts as a reviewer for SSDF PW.2 and PW.7.

Changed

- `related-work.md`, Standards and frameworks: the sentence saying the method offers no crosswalk
  now says the method maps no clauses of the governance standards there and points to
  `standards-map.md`, as placement, not a compliance claim.
- `README.md` lists the new document, points readers who arrive with a security framework to it,
  and its limits list points to the crosswalk.
- `method.md`, §0: the document table lists `standards-map.md`.

No change to any principle, role, control, profile or metric.

## v0.10.1 -- 2026-10-27

Added

- `related-work.md` opens with a short summary: most parts of the method exist in prior work, that
  work does not address the setting, and the method assembles the parts for an agent team building
  production software under one owner or a small owner team. It says who reads what (adopters can
  go straight to `method.md` and `practice.md`; the rest is the evidence, for reviewers), links the
  sections of the arc, and gives a key to L1-L8, the profiles and the principle numbers.
- Paperclip, an open-source orchestration server for agent teams, in both comparison tables, in
  the agent orchestration frameworks section and with its own bullet in the conclusion. Its
  governance is credited: board approvals, budget caps that pause an agent, run-scoped agent
  tokens, a responsible user bounding every agent write, configuration revisions with rollback and
  versioned skills. Where it differs is stated: actions run unless held, "Always allow" saves a
  standing rule, shell git and gh are left to host branch protection, instruction and skill changes
  take effect before review, and isolation is opt-in. It is not specific to coding agents; it is
  included because it runs coding agents such as Claude Code and Codex and is widely used.
- A short note on self-improving harnesses under harness engineering. It reports a practitioner's
  feedback (NOOA as a possible better fit than Claude Code; Prime Agent and Harness² as
  incompatible), credits what each provides, and states how the existing rules apply: agent
  instruction files in the repository reach the owner as the diff (L5), skills are agent tooling
  from an owner-approved list, and enforcement material stays read-only to the session (L3).
  Memories a tool keeps for itself are recorded as an open question. RegenHarness is described as
  an updater that cannot weaken its own acceptance gate; its paper does not state who authorizes a
  release.
- Works cited entries for Paperclip, NOOA, Prime Agent, Harness² and RegenHarness.

Changed

- `related-work.md` follows one arc: comparison; organizing agents; authority and enforcement;
  isolation, evidence and standards; a practitioner's reading; and one conclusion that merges "Why
  the prior art does not simply apply" and "What this method is". Each group opens with a lead-in
  on what it shows, the table rows and Works cited follow the same groups, and the comparison
  intro gives the reading guide before the inclusion rule. No claim is removed.
- Every Works cited entry with a stable address is a link (Graicunas 1933 and Urwick 1956 have
  none), the body links works at their first mention, arXiv IDs and RFC numbers are
  links, and internal references link to their sections. MAS, OCM, SPKI/SDSI and AUROC are
  expanded at first use.
- Citation details: Abbas et al. appeared in the *International Journal of Intelligent Information
  Systems*, not the *Journal of Intelligent Information Systems*; the MOISE entry gives pages
  156-165 and the Hardy entry pages 36-38; the Hassan draft is cited and linked at -02, the
  revision read, and notes that -03 keeps the narrowing-only chain; the GSD Core entry gives the
  release date of v1.18.0; the Moise example names its file; the Graicunas entry names the 1937
  reprint.

No change to any principle, role, control, profile or metric.

## v0.10 -- 2026-10-26

Changed

- **Breaking (Lite): what the tool must provide.** Before kickoff, adopters check the tool against
  "What the tool must provide" (control reference) and record each missing row as an accepted
  residual. Where the chosen profile requires an affected control that is kept no other way, the
  gap is also an exception, unless the control's Kept by cell says "otherwise expectation";
  exceptions for missing tool features are reviewed yearly. Nothing the session can write or
  switch on widens enforcement material, the tool's own configuration included; other user-level
  configuration stays writable at Standard. A check that guards a tool call runs before the call
  and sees the whole call. Nothing routes a Standard grant's permission prompt to anyone but the
  owner. The L6 control "Local messaging with tool-attributed senders" is renamed "Local
  messaging with senders set by the tool or the launch configuration" and applies where more
  than one session runs. `claude-code-reference.md`: role sets come through `--settings` or managed
  settings, settings files the session can reach are unwritable to it, and `autoMode` entries go
  in user scope only where the session cannot write user settings.
- Wording that no longer assumes one tool: a tier is the model and, where the tool has one, its
  reasoning setting; specialists run as subagents or separate sessions; the scope check is a named
  step of the pre-execution check; the start self-check runs from the tool's start hook, or else
  the launch command gives it to the session as its first task.

## v0.9 -- 2026-10-26

Changed

- **Breaking (every profile): environment authority.** Environment authority is the mandate for
  any action that changes what another session, or a later session in the same environment, gets
  by default, or that acts on a store or resource outside the environment. Adding a version to a
  content-addressed store from a committed pin, downloads the environment's definition allows,
  frozen-mode installs from the lockfile, installing a dependency the approved slice plan adds, the
  session's own containers and throwaway services, and its own pushes and messages are not
  environment authority. Environment authority is: system packages, anything on the shared `PATH`,
  user-level or global configuration, credentials, pruning a shared cache or a container runtime,
  and any read or write of a lab or shared store. Environment changes go through the owner. The
  orchestrator never holds environment authority, and a grant never confers it. A container
  runtime counts as root in the environment it reaches. At Lite, no session is assigned
  environment authority: the brief states the rule, a session lists any environment change it
  makes in its hand-over note, and the owner makes environment changes from the owner's own shell
  or account, never through an agent session. "Destructive work" is folded into environment
  authority and removed from the glossary.
- **Breaking (Standard): environment authority is assigned and isolated.** Environment authority is
  assigned only as a planned delegation at kickoff, per lane or per slice, and recorded in the plan
  and the kickoff record. It belongs to an environment for that environment's lifetime: a session
  not assigned it never runs in an environment where it was held or used, unless that environment
  has first been recreated from its definition, and the lab credentials of an assigned environment
  never pass to another session. A session assigned it gets its own environment. No session holds
  both external ingestion and environment authority. Every other session is denied it by its
  environment, a boundary: no root or sudo, system paths read-only, a container runtime that is
  rootless or confined to a VM serving one session alone, image and definition changes applied by
  the owner. Lab credentials sit only in the environments of sessions assigned environment
  authority. The owner changes an environment through its definition, from the owner's checkout,
  and the environment is rebuilt; a broken shared environment is recreated, not repaired. A need
  for environment authority found during a run is met by a definition change and a kickoff for that
  environment; other needs are met by a definition change and a rebuild. New kickoff tests: a
  privilege escalation, a system-path write and a lab action are refused, and any container runtime
  is confirmed rootless or confined to a VM serving one session alone.
- **Breaking (High): environments are recreated, not changed.** Each session's VM is recreated from
  an owner-approved definition; a VM in which environment authority was used is recreated before its
  next slice, and lasting changes go through the definition.
- **Breaking (every profile): smaller obligations.** The plan lists each lane's environment needs.
  Environment changes are owner chores, batched into one owner visit, and the pause protocol lists
  them. Changes made under environment authority are recorded in the slice ledger and the hand-over
  note. Slice-plan review checks each use of environment authority against the owner's assignment.
  After a suspected compromise, lab and shared resources are reviewed from records held outside the
  suspect environments; no revert command or backup written by a suspect session is used.
- "What can catch misdirection inside the baseline" (control reference, L4) states that no
  merge-time control sees an environment action taken during a run, and what bounds it per profile:
  at Lite only the brief, from Standard the isolation split, the environment boundary and
  least-scope credentials. A session assigned environment authority holds root in its own
  environment, so its in-environment controls are self-attested there; the L3 rows for enforcement
  material, hooks and the action classifier say so, and the kickoff's enforcement-material write
  there is tried without root and recorded as self-attested.
- "Where the owner acts" (control reference, L4): for environment changes, "the owner never acts
  inside an agent environment" holds from Standard; at Lite the owner works from the owner's own
  shell or account.
- The owner's row (method §2) adds environments; the orchestrator's "Never" column adds "holds
  environment authority"; the worker's adds any action that needs environment authority without
  being assigned it. In P1 and P2, the agent session prepares kickoff and environment changes.
  Grants no longer list an infrastructure write among their examples. An environment rebuilt from a
  definition change that touches no enforcement material, OS user or group, mount, credential or
  network setting counts as tested; from Standard, its sessions rerun the environment-authority
  refusals and the container-runtime check.
- `related-work.md`: this method's isolation cell names the split by authority, environment
  authority and input trust.

Added

- Residuals. At every profile: misdirection can steer a session that holds environment authority
  within its assignment. Lite: environment changes rest on the brief, any session can change the
  shared environment, and a container runtime counts as root where it reaches. Standard: the shared
  home directory, the container runtime and a persistent package cache, through which one session
  can poison what later installs receive, join the shared-worker residual; a new residual records
  that only system-level changes meet a boundary, and that an assigned session's checks and records
  in its own environment are self-attested.
- Threat-model plan: environment definitions and images as a data store and a trust boundary; a
  threat class for environment changes made through misdirection, with its mitigations.
- Glossary: "Environment authority", distinguished from capability.
- `claude-code-reference.md`: the container or VM as the boundary; the sandbox as an optional extra
  layer, with `failIfUnavailable`, `allowUnsandboxedCommands: false` and nothing in
  `excludedCommands`; deny rules and hooks as a setting that can be evaded; the list of what runs
  outside the sandbox. The orchestrator and worker rows, and the implementation plan, kickoff
  record, common brief, slice plan, slice ledger and hand-over note templates, state environment
  authority.

Migrating

- Every profile: add the rule to every brief; send environment changes to the owner as batched
  chores; list each lane's environment needs in the plan; record changes made under environment
  authority in the slice ledger and hand-over note; stop granting environment authority; remove
  environment authority from the orchestrator.
- Lite: assign environment authority to no session; the owner makes environment changes from the
  owner's own shell or account, never through an agent session.
- Standard: remove root and sudo from every environment whose sessions hold no environment
  authority, and make system paths read-only there; replace a rootful container runtime with one
  that is rootless or confined to a VM serving one session alone; keep lab credentials only in the
  environments of sessions assigned environment authority; give each session assigned environment
  authority (environment changes of its own, or lab actions) its own environment, never also
  assigned external ingestion; recreate an environment from its definition before a session not
  assigned environment authority runs where it was held or used; list the assignments in the plan
  and the kickoff record; change environments only through their definitions; add the new kickoff
  tests.
- High: recreate VMs from the definition instead of changing them, and recreate a VM after
  environment authority was used in it.
- Briefs and slice plans that say "destructive work" say "environment authority".

New required controls: at every profile, environment changes through the owner, stated in the
brief, with any change a session makes listed in its hand-over note; environment needs per lane in
the plan; environment authority never by grant and never to the orchestrator; changes under
environment authority recorded in the slice ledger and hand-over note; the lab and shared-resource
review after a suspected compromise. At Standard: environment authority only as a planned
delegation at kickoff, held by an environment for its lifetime; its own environment for a session
assigned it, never also assigned external ingestion; recreation before a session not assigned it
runs there; denial to every other session by its environment, including the container-runtime
rule; lab credentials only in the environments of sessions assigned it, never passed on;
definition changes from the owner's checkout; the kickoff tests of refused environment changes and
the container-runtime check. At High, VMs recreated from an owner-approved definition, and
recreated after environment authority was used.

## v0.8.2 -- 2026-10-26

Added

- `related-work.md` opens with a comparison in two tables. The first compares positioning, level
  and form; the second compares authority over outward actions, enforcement, isolation,
  verification and continuity. Works enter by a stated rule: a public primary source, about AI
  coding agents, widely used or cited, that governs or structures how coding agents work. The
  works kept outside the rule, and the rows that summarise the sections below, are named.
- A "Session workflow frameworks" section in `related-work.md` describes GSD Core, GitHub Spec
  Kit, the BMAD Method, pstack and Superpowers, and names OpenSpec and Kiro specs. pstack is
  credited with structures this method also uses: a coordinator over workers, a ledger and
  decision log, standing orders re-injected on every spawn and resume, and a verifier on a
  different model family. Its difference on authority is stated: external actions such as team
  chat proceed without asking, and an autopilot playbook can merge; irreversible writes always
  wait for the user. Superpowers is linked to this method's fourth and seventh principles. The
  section states that this method specifies the controls around a team and leaves the in-session
  workflow open, and how such a framework is governed when a worker runs it inside a slice: as
  agent tooling, with its hooks and instruction files as protected paths and its hooks as
  enforcement material, without changing the session's denied permissions.
- Works cited: GSD Core and the archived GSD repository, GitHub Spec Kit, the BMAD Method, pstack,
  Superpowers, OpenSpec and the Kiro specs documentation. The CrewAI entry adds Flows, and the
  Cruz entry records that it read v0.7.

Changed

- `related-work.md`: the four-column summary table (governs, unit of control, evidence basis) is
  replaced by the two comparison tables.
- README: the related-work row of the documents table mentions the comparison.

Fixed

- GAIE: its principle that the generating agent cannot approve its own deployment corresponds to
  this method's first principle, the owner holding every merge and release at every profile, not
  to the two-party rule, which applies at High alone.
- GAIE: "uncertainty escalates upward and never downward" is described as GAIE's own principle,
  not as a position this method holds; the method does not state it.
- GAIE: its routing uses a confidence score against a threshold and a dependency-graph check, in
  addition to the four risk dimensions the paper frames.
- Madatha: "enforcement material protected from the session it governs" is no longer listed among
  its positions. Its integrity checks re-hash agent definitions at install time and detect
  accidental or automated tampering through a keyed lockfile; the paper notes that a same-user
  process could forge the lockfile stamp.
- Madatha: it names bounded delegation depth as a mitigation but describes no mechanism, rather
  than addressing no nested delegation at all.
- Madatha: it governs agent definitions across an organisation's repositories and seven IDE
  targets, with runtime enforcement by hooks in two harnesses, not one harness on one machine.
- Madatha: it is credited with the positions it reached, without a statement that it reached them
  first, in line with the document's no-priority stance.
- CrewAI: the feature list includes the Flows human-feedback decorator for approval gates and
  review points; the conclusion that it is not an owner authority over outward actions stands.
- CrewAI and MetaGPT: the orchestration-frameworks bullet says neither measures the owner's
  interrupts and waiting time during a run, and that MetaGPT measures the human revisions its
  output needs afterwards, rather than that neither carries any measure of human cost.
- CASE: the text includes its three studies on public data (62 incidents, 22 tools,
  35 deployments) and that the pipeline coding that data is MIT-licensed.
- Cruz: the body uses "corresponds to" for the article's proposals and carries no version of this
  method; the version read moves to the Works cited entry.

No change to any principle, role, control, profile or metric.

## v0.8.1 -- 2026-10-22

Changed

- `related-work.md` no longer carries correction notices or version history in its body. The
  corrections it listed are recorded in this changelog: the withdrawn novelty claim under v0.3
  and v0.4, and the source-check corrections under v0.8. The reclassification of Madatha's
  control plane is stated in the present tense, and the no-novelty statement keeps its reason.

No change to any principle, role, control, profile or metric.

## v0.8 -- 2026-10-22

Changed

- **Breaking: the configuration and scripts CI invokes are protected paths from Lite.** Test-runner,
  lint and scan configuration and allow-lists join CI workflows and agent instruction files as the
  Lite protected paths. They reach the owner as the diff; the pre-PR gate lists changed protected
  paths, and at Lite the owner's merge ritual takes them from the host's changed-file list. The
  list is closed, kept by path in a protected file. CI proves that the committed checks ran and
  passed, not that they are adequate.
- **Breaking: release credentials meet no repository code (Lite).** The step that holds release or
  publish credentials runs no repository code. It publishes, by digest, artefacts built in CI or in
  an agent environment.
- **Breaking: releases scale by profile.** Every profile follows a short release checklist: the
  owner's decision, CI green by commit, artefacts built in CI or an agent environment and published
  by digest, the owner's tag. Standard adds the user-path end-to-end gate. The rehearsed, gated kit
  and the pinned-kit freeze are High.
- **Breaking: regulated data also raises L7.** The trigger "customer production or regulated data"
  now raises L1, L3, L4 and L7 to High.
- **Breaking: the dedicated development machine is defined (Lite).** The owner never performs
  outward actions from it, it holds no owner credentials, and its enforcement material is owned by
  another OS user or delivered as managed settings.
- **Breaking: the orchestrator never executes product code and never fetches (Standard).**
  Integrated suites and hot-file regeneration run in CI; fetching subagents and research
  capabilities run in an ingestion environment. The orchestrator reads results by commit, its role
  set denies fetch tools, and its environment has a narrow egress allow-list. Subagents and
  capability runs count as sessions for the isolation rule.
- **Breaking: fetch tools are denied outside ingestion environments (Standard).** The shared worker
  role set denies them too, because some search tools pass any egress allow-list. Adding a new
  dependency is a decision recorded in the slice plan.
- **Breaking: kickoff, the start self-check and re-checks (every profile).** Kickoff proves an
  environment and its launch configuration, including a fail-closed test and refusals with every
  host credential present. A new session on a tested configuration runs the start self-check:
  identity, restated limits re-injected from the ledger, one denied action, one enforcement-material
  write, one credentialed action. The same check re-runs after every restart or resume, after every
  protected-path merge and at each vehicle. A launch command that differs only in its tier counts as
  tested. Sessions read CI with a read-only checks token per environment, never the owner's token.
  "The exact commit that will merge" requires up-to-date-branch protection or a merge queue.
- **Breaking: exceptions and residuals are owned and dated.** A CI bypass, a release despite a failed
  check, a release downgrade and a layer below its chosen profile are exceptions, each with a named
  acceptor and a review date. A review date that arrives puts the exception on the owner's queue;
  host-plan exceptions are reviewed yearly. Accepted residuals are recorded with the profile per
  layer and accepted by name. The minimal metrics add owner-wait time, open exceptions by count and
  age, exceptions taken per period, and vehicles released below their stated profile.
- **Breaking: human reading of high-risk slices (new High L7 control).** At High, a human reads the
  diff of every high-risk slice before its merge vehicle merges: a slice that touches security,
  authentication or authorisation, changes how sensitive or regulated data is handled, or changes a
  cross-cutting contract. Any reviewer can raise a slice's class; only the owner lowers it. Contract
  slices are read when they reach the integration branch. Stale approvals are dismissed on new
  commits. The host records an approval, not a reading. It extends the protected-path rule.
- **Breaking: the vehicle hand-off and the agent PR identity.** The owner pushes the vehicle branch
  from the owner's checkout. An agent identity with pull-request write only (no push, merge or tag)
  opens the PR, so the host can record the owner's review as a separate act. It is required wherever
  L7 is High and recommended below. Host rules restrict main and tags to the owner where the plan
  allows. Where only the owner's account exists, owner approvals are self-attested.
- **Breaking: the independent verifier's verdict is deterministic** (the rebuild's digests match and
  the tests pass) and is signed by a separate step that runs no repository code and no model, with a
  key outside the environment that builds and tests. The sole-owner form is reported as independent
  verification, not segregation of duties.
- **Breaking: tier escalation never forks.** A stop is the worker's report that it cannot complete
  the slice as planned; pauses, usage limits and environment failures are not stops. The failing
  session puts its work in progress on a side branch and writes its failure notes. A fresh session
  on the stronger tier starts from the slice's last reviewed commit with the failure notes as input;
  at High, in a new VM, with the stopped instance's credentials revoked and its own instance
  credential. Past the strongest tier, the slice goes to the owner as one decision.
- **Breaking: review across tiers.** "A tier's output is never reviewed by the same tier" is replaced
  by: reviews run in fresh context at the strongest tier, from a different model family where
  available.
- **Breaking: a tier is a per-slice launch parameter** of the owning worker's session, so routing
  sets the tier and ownership does not move. Where workers keep fixed tiers, a reassignment
  transfers ownership and is recorded on the board. The orchestrator launches sessions where the
  tool allows; otherwise launches are owner chores. Every high-risk slice runs at the strongest
  tier.
- **Breaking: the action classifier (High)** reads the slice plan at its approved commit, and a
  deviation enters only after re-approval. Scope is checked by a named deterministic hook step. The
  agent's description and comments are stripped from the call before classification. A grantable
  action follows the session's L4 grant path; a permission prompt is never answered by another
  session. Each decision point has its own labelled set, thresholds and coverage.
- **Breaking: context sharing follows the environment, not the L6 profile.** Fuller context may be
  shared only within one environment, never out of a session that ingested untrusted content, and
  never into reviewers, judges or the independent verifier.
- **Breaking: High L4 needs L1 or L2 at High**, so that other sessions cannot read a session's
  instance credential.
- **Breaking: composed plans.** A plan segment's pattern sets only its shape. The approver, ledger,
  escalation path and integration branch come from the plan's highest pattern. Each segment
  boundary runs the checkpoint review (at Lite, a final-diff review of the segment) before the next
  segment starts. An escalation from P2 to P4
  re-runs plan review.
- **Breaking: merge vehicles and release vehicles.** A merge vehicle (a PR to main) closes a
  checkpoint. Only a release vehicle (a tag or publish run) enters release mode, and sustain mode
  starts at the first release vehicle.
- **Breaking: the leftover sweep runs at every merge vehicle from Standard**; the leftover scan runs
  in every pre-PR gate.
- **Breaking: L2 at Standard is per environment.** "Own runtime" and "Own identity" become "Own
  runtime per environment" and "Own identity per environment". Lab credentials are per environment
  at Standard, and per session and lab slot at High. The Standard message channel uses
  per-environment credentials with the sending session's name stamped by the transport.
- **Breaking: smaller obligations.** Slice plans and hand-over notes record every assumption about
  anything outside the ownership partition, and the orchestrator checks them at plan review. A
  deviation from an approved plan takes effect only after re-approval. A P3 run that writes is
  idempotent and commits per batch. In P1 and P2 the ledger lives on the ledger ref like any other.
- **The method is split into three documents.** `method.md` keeps what the method requires and
  why, with procedures replaced by pointers; its section numbers are unchanged. The new
  `practice.md`, for whoever runs a team, holds the procedures: setting up Lite, launching
  sessions, the documentation chain, kickoff and session checks, the slice routine, routing and tier
  escalation, checkpoints, the vehicle hand-off and the release checklist, the documentation fold,
  session continuity, the sustain-mode procedure, and the incident record with the
  suspected-compromise steps. The new `glossary.md` holds the glossary, in alphabetical order, and
  every document links to it. The owner-chore budget moves into the owner-attention technique
  (method §10). The layer table in method §5 drops its "Typical controls" column, which repeated the
  profile table.
- **Residual risk at every profile.** The "below High L7" phrasing is replaced by residuals stated
  for Lite, Standard and High alike: product code outside the protected paths and outside the
  high-risk slices that High L7 sends to a human reader can reach main without a human reading it;
  untrusted text in installed dependencies, logs and tool output reaches every session. The
  Standard shared-environment residual now names the internal remote's refs, and the ledger's
  restated limits are protected against a lossy summary, not against tampering below High.
- **Principle 7 is new: "Agents forget: design for it."** Limits, state and decisions live in
  durable artefacts and are re-read and re-checked. The former principle 7 is now principle 8.
- **Proof and Kept by.** "Independent" means produced and stored outside every agent environment;
  the internal remote, transport and broker count only where they run outside. Rows that failed
  this, or proved only part of a control, are re-tagged. "Setting" holds only where it matches
  every form of the action; deny-push names its real boundary, the absent credential.
- **The L5 objective** reads "Nothing reaches main without agent review and host-enforced CI",
  with the human readers named.
- **README and the related-work summary table:** the open-source date is corrected from March to
  February 2026, and the release counts ("twelve minor releases"; "eight months, twelve releases")
  are replaced by "released continuously since". A Maturity paragraph says that the method runs at
  Lite and Standard depending on the layer, and the "working method" sentence no longer claims more
  than that.
- **The isolation trigger is defined by assignment.** "Ingests untrusted content" means assigned
  external ingestion; "destructive work" means anything targeting a store other than the session's
  own throwaway stores; "equal authority" means the same role permission set.
- **The documentation chain scales with the pattern**, and review depth at every stage follows the
  profile.
- **Learning loop:** the same failure (same rule or control) seen twice becomes a slice, not any
  root-cause class seen twice.
- **Metrics:** "idle-worker time" is replaced by "fitting work waiting while a fitting worker is
  idle". Tiers are compared within one complexity-and-risk class, with cost per tier from
  per-session telemetry. Classifier error rates hold on the calibration set; they are not a bound
  against an adaptive attacker and are not reported as a detection rate.
- "Decider tiers" are renamed "decider models", so "tier" means a session's configuration only, and
  the glossary's "Worker tier" becomes "Tier".

Added

- **A "Kept by" column** in every control table of the control reference: expectation, setting or
  boundary. A new subsection in "How to read this reference" reads each control as the agent's
  capability (what it can do) against its mandate (what it may do), the gap between them, and what
  keeps that gap closed. It explains how Kept by combines with the Proof column, and how the
  learning loop moves a rule from expectation toward setting and boundary.
- **A Proof column** in every control table, each proof tagged independent or self-attested. The
  per-layer proof lines are folded into it.
- **Periodic re-injection** (practice, session continuity): at intervals, the restated limits are
  re-injected from the ledger, with no report and no comparison. An incident is recorded only when a
  session acts against a limit.
- **End-to-end encrypted, sender-signed messages** as an option at High (control reference, L6):
  the relay holds only ciphertext, and unique IDs and timestamps stop replay, which removes the
  broker operator from the trust base. Not a requirement; the repository ledger stays the record.
- **Scope:** governing agents that run inside the product is out of scope (method §0). **Data sent
  to the model provider** is placed outside the method, with a threat class in the threat-model
  plan.
- **Credit:** `related-work.md` credits Dinis Cruz's reading of v0.7 in a newsroom run by agents, a
  practitioner's self-assessment, not an audit. It reports independent convergence on several
  principles; the Kept by column adapts its grant (here: capability), mandate, delta and barrier
  framing; and it observed in-band CI, the case the out-of-band CI rule addresses.
- **Residual risks for Standard**, parallel to Lite's: the shared worker environment, agent-written
  records, grants as prompt answers, and the sole owner releasing alone. The Lite residual on a
  dedicated machine now reads "read and write".
- **Product code and human review** (method §12): which product code a human reads (protected
  paths, and high-risk slices at High L7), and a human reader of the vehicle diff for adopters who
  need independent review.
- **Suspected compromise:** a five-step stop procedure (practice §9, with a pointer in method §11).
  It checks the internal remote and released artefacts, and recreates the affected trust domain
  with new credentials and a full kickoff before any session resumes.
- **Trust-anchor rotation** for a compromised signing key or device (High): environments are torn
  down and recreated, and what the old key signed is re-issued. With a threat class.
- **The signed kickoff manifest** has its own row in L4, the map and method §12.
- **README "Limits"** in "At a glance": links to the residuals, the scope-outs and the framework
  placement.
- Threat-model plan: the decision engine as an element; the Standard environment boundaries; the
  PR-opening token and the verifier's signing key as credentials; threat classes for misuse of a
  permitted tool, classifier evasion and calibration-set poisoning, and release credentials meeting
  repository code.
- Control reference L7 and the map list the profiled techniques from the method: property and
  mutation tests, the user-path end-to-end release gate and capability gates at Standard; refute
  passes and blind judges at High.
- An L2 row for "no standing approvals", already required at every profile.
- Glossary entries: agent PR identity, baseline (renamed from "baseline delegation"), brief, build
  and release mode, complexity-and-risk class, decision engine and calibration set, decision
  message, decision register, dedicated development machine, destructive work, enforcement
  material, equal authority, exception, external ingestion, gateway and ingestion point, grantable
  action class, hand-over note, high-risk slice, incident, ingestion environment, integration
  branch, Kept by, kickoff manifest, kickoff record, ledger, mandate, merge vehicle, message audit,
  owner-attention budget, ownership partition, plan segment, planned compaction, planned
  delegation, pre-PR gate, proof, quarantine, re-check, re-injection, recovery and pause protocol,
  refute pass, roles, shared hot files, shared worker environment, slice plan, start self-check,
  stop, trust domain, two-party rule, vehicle. "Leftover sweep" becomes "leftover scan / sweep".
  The capability entry notes the second sense used by Kept by.
- Templates: decision register entry and slice ledger; tier, assumptions, re-checks and exceptions
  fields in the plan, slice plan, board and kickoff record; the release run sheet records the
  checklist, and the hand-over note names the escalation side branch.
- `claude-code-reference.md`: GitHub settings for workflow permissions, `persist-credentials`, tag
  and branch rulesets, the publish job, plan limits with a recorded-exception path, Dependabot PRs,
  the agent PR identity and code-owner review as a path-based floor; reviewers are never fork
  subagents; `allowManagedModsOnly`; no `--permission-prompt-tool` except one that reaches only
  the owner; the launch command re-passed in full on every resume; `PreModelSwitch`; a `Stop` hook
  for re-injection; a GitHub App token for reading CI; the settings scope of `--restricted`; `/fork`
  with agent view off; merge-queue plan limits.
- `related-work.md`: a section placing the method against SSDF, SLSA, OWASP guidance on LLM and
  agentic applications, NIST AI RMF and ISO/IEC 42001, without a crosswalk.

Fixed

- **Usage and cost** (control reference, L1): the claim that separate credentials per environment
  spread usage across separate limits is withdrawn. Separate API keys within one organisation
  usually share its limits; separate allowances per account or seat are optional and depend on the
  provider's terms.
- "Will catch" and "stop it" are now "can catch" and "may stop it" where they describe what catches
  misdirected or misled work.
- Profile tables agree: the dedicated-machine option appears in the control reference, and the
  method's §12 cells carry the map's wording. §5 "Serves" matches the control tables, L7 included.
- §2.1 and §12 agree on where High L4 applies: only its cross-environment parts need separate
  environments. The "High L4 needs L1 or L2 at High" rule now also appears in the control
  reference.
- The pause protocol no longer asks a Standard owner to issue grants ahead; that works at High only.
- The "destructive work" example no longer contradicts the Lite rule on adversarial tests; the
  Lite baseline is named; the threat model is "being" built, not done.
- `claude-code-reference.md`: saved effort is a per-model `modelSettings` entry, and
  `CLAUDE_CODE_EFFORT_LEVEL` overrides `--effort`; `/model` and `/effort` save a user default unless
  the session-only option is chosen; `onFailure: "block"` needs 2.1.295 or later, works on `command`
  and `http` hooks only, and does not block on `SessionStart`; a mod-approved call skips the
  auto-mode classifier in every setup, unless the guard and a prompt tool are both present;
  `allowManagedHooksOnly` blocks agent-frontmatter hooks and plugin hooks unless the plugin is
  force-enabled; the grant check sits in the classifier hook because `deny` beats `allow`; a
  read-only workflow default does not bind same-repository PRs; `DISABLE_UPDATES`; the `clear`
  source; Dependabot secrets; the broken "Appendix B" reference; reading CI by commit with
  `gh run list --commit`.
- `related-work.md`, after a source check:
  - MOISE (2000) is by Hannoun, Boissier, Sichman and Sayettat; the structural, functional and
    deontic split belongs to MOISE+ (2002). Organizational rules enter Gaia with the 2003
    revision, not Gaia 2000; the same authors introduced the concept in 2001. The
    writers-and-editor cardinality example comes from the Moise software distribution, not the
    2002 paper.
  - Graicunas did prescribe a span limit: five, and probably four. Two uncited statements about
    later empirical spans and modern contingency views are removed.
  - CrewAI's guidance against delegation loops is in its official documentation, and its
    delegation flag now defaults to off. The earlier text called the first a community view and
    the second a documentation disagreement.
  - RFC 8693 does not require narrowing and does not enforce its delegation chain. Attenuation
    does not make authority non-relayable: the cited credentials are bearer credentials.
  - The harness-engineering study examines how coding tools are configured, not enforcement
    before a call. An uncited sentence about a line of related work is removed.
  - GAIE is the closest prior formulation of risk-tiered oversight for code generation, not its
    origin. Not all of Madatha's five tiers are allow-lists, and the two figures quoted from its
    prevalence study are computed differently. CASE's third layer treats the supervision of
    human-agent teams, not team structure.
  - Introduced in v0.7: the quotation from Anthropic's guidance was wrong. Kim et al.'s figures
    come from the first version of their study and its blog; the current version reports 260
    configurations and six benchmarks. Cemri et al. derived their taxonomy from 150 traces and
    applied it to more than 1,600.
  - Introduced in v0.6: Guo et al. do evaluate injected instructions in tool outputs. Their
    finding about context concerns fields that encode the label, so it no longer stands as
    support for the trusted-context rule. Their model is calibrated when pooled, not per
    benchmark. Gao and Wang evaluate two tasks plus a disagreement dataset, and their result is
    that their objective gains nothing over cross-entropy under annotator disagreement, not that
    a model's answer is unreliable there.
  - Three leftover phrases still stated a claim of this method's own after v0.4 withdrew every
    claim. They are rewritten, and two later gap phrasings are rewritten too.
  - CrewAI was said to have no human review and no environment isolation, and Madatha's
    write-path validation and hook coverage were overstated. Several Works-cited entries lacked
    authors or had truncated titles.
  - MetaGPT's intermediate documents are requirements, designs and interface specifications;
    test reports are not among them.
- "An auditor can hold" now reads "designed so an auditor can hold".
- The threat-model note names one author and uses the README's capitalisation.
- The README issues link is absolute.
- v0.6 and v0.7 entries below: the v0.7 breaking change moved to "Changed", and missing items added.

Profile moves: per-slot lab credentials from Standard to High; the rehearsed release kit applies at
High only. New required controls: at every profile, kickoff with the start self-check and
re-checks, periodic re-injection, the protected-path, release-credential and exception rules, the
read-only checks token and the merge and tag kickoff test, and the extended minimal metrics; for
the Lite dedicated-development-machine option, its requirements; at
Standard, the orchestrator rule, fetch tools denied outside ingestion environments and the full
leftover sweep at every merge vehicle; at High, the action classifier's approved-commit and
scope-hook rules, the coherence rule that High L4 needs L1 or L2 at High, the human reading of
high-risk slices, the agent PR identity wherever L7 is High, and the verifier's deterministic,
separately signed verdict.

## v0.7 -- 2026-10-21

Changed

- **Breaking: session isolation at Standard follows authority and input trust, not session count.**
  Standard no longer requires an environment per session. It requires the orchestrator in its own
  environment, allows workers of equal authority to share one (a worktree each), and gives its own
  environment to every session that ingests untrusted content or runs destructive work. Credentials
  are per environment at Standard and per session at High. A new subsection in the control
  reference (L1) states why sessions are separated, what that does not buy, and that separate
  credentials per environment also spread usage and cost within the provider's terms (superseded in
  v0.8: the limit-spreading claim is withdrawn).
- **Choosing a pattern by coupling, not size.** Sequential or tightly coupled work stays with one
  agent (P1, P2); multi-agent patterns are for work that splits into parts independent in
  substance. Tool load counts as a coordination cost.
- **Patterns compose.** A plan may assign a pattern per segment, for example P2 for a serial core
  and P4 for the branches that split from it.
- **P4's integration condition is explicit:** workers write only on their own branches within
  their ownership; the orchestrator alone integrates.
- **Breaking: "No idle workers" is replaced** by "idle capacity only for fitting, independent work". With
  worker tiers, assignment follows fit, not idleness; an idle worker is the right state when the
  remaining work is coupled.
- **Tier escalation states its criterion:** fork the session when the slice made progress the
  stronger tier needs; start fresh with the hand-over, assumptions and failure notes when the
  context itself is the problem (superseded in v0.8: escalation never forks).
- `claude-code-reference.md`: worker placement per profile, tier escalation by fork or fresh start
  (superseded in v0.8: escalation never forks), and the assumption and failure-note template fields.
- `method.md` §5: L1 now serves S, C.

Added

- **Assumptions and decisions taken** in slice plans and hand-over notes; the orchestrator passes
  on those that touch other slices.
- **Context sharing by profile** (control reference, L6): at Lite, where sessions share one trust
  domain, fuller context may be shared; at Standard and High, references only.
- Optional tagging of incidents with a published multi-agent failure taxonomy (learning loop).
- `related-work.md`: a section on when multiple agents help (Anthropic 2024 and 2025, Cognition
  2025 and 2026, Kim et al., Cemri et al.).

Per-session credentials moved from Standard to High; no other control moved between profiles.

## v0.6 -- 2026-10-21

Added

- **Worker tiers.** Workers can differ in configuration (model and effort level). The plan tags
  each slice with a complexity-and-risk class, and the orchestrator routes by that class first and
  by availability second. A slice that stops or fails review twice goes up a tier; a tier's output
  is never reviewed by the same tier; a tier never changes permissions. New metric: escaped
  defects and cost per tier.
- **Action classifier (High, L3).** A calibrated decision model checks the side-effecting tool
  calls the rules allowed, for misuse of a permitted tool. Its context comes only from trusted
  sources (slice plan, delegation manifest, provenance), never from the agent's own
  justification. Scope stays deterministic. It shares one engine with the L6 decision classifier.
- **Thresholds from a target error rate.** Classifier thresholds are set from an error rate the
  owner tolerates on items decided alone, and the calibration record states the coverage at that
  target. New metric: classifier coverage. Two decider tiers: a fast encoder for high-volume
  points, a calibrated reasoning model for the gateway and quarantine triage.
- `related-work.md`: a section on calibrated decision models, with two 2026 preprints.
- Local calibration may fine-tune the model with a calibrated-decision training recipe.
- Threat-model traceability: misuse of a permitted tool.
- `claude-code-reference.md`: the action-classifier hook and the reasoning model for the gateway and
  quarantine triage.

Fixed

- `claude-code-reference.md`: removed a stray review-finding label.

No control moved between profiles.

## v0.5 -- 2026-10-21

Changed

- **Breaking: required CI moves from Standard to Lite.** At Lite every other check is an agent's
  report; without CI, Lite had no independent verification. Lite requires a minimum set (build,
  test suites, type and lint checks, a secret scan); Standard runs the full suite.
- **CI is named as the out-of-band verifier.** New subsection in `control-reference.md` L5: the
  host enforces the gate, agents cannot change what verifies them, runners are separate from agent
  environments, and results are read from the host by commit. New L5 control "CI kept out of band"
  at Lite. At High, CI is the deterministic half of release verification and the second party the
  judgement half.
- Threat-model plan: a threat class for agents influencing their own verification, and CI added
  to the mitigations of fabricated release evidence.
- `claude-code-reference.md` L5: local review commands never stand in for CI; read results from
  the host by commit; how release secrets are kept from PR code on GitHub.

## v0.4 -- 2026-10-20

Changed

- **The novelty claim is withdrawn rather than restated.** v0.3 corrected the v0.2 gap claim and
  narrowed it. This version removes it. A reference method does not need to be first, and a gap
  claim is the one thing in a document like this that can be wrong at the author's expense: the
  survey behind it missed established work twice, so the claim is gone instead of hedged again.
  `related-work.md` now states that it makes no novelty and no priority claim.
- The credits stay. What Kang and Madatha published first is still recorded, and the prior art is
  kept for orientation, so a reader can see what the method is assembled from.
- "What the prior art does not cover" is now "Why the prior art does not simply apply", framed as
  a question about setting rather than a claim to territory, and it closes on "none of that makes
  this method novel".
- "What this method claims" is now "What this method is": a synthesis assembled for one setting, a
  method that instruments the owner, and one open question it does not answer.

No change to any principle, role, control, profile or metric.

## v0.3 -- 2026-10-19

Changed

- `related-work.md` substantially rewritten. The v0.2 version claimed four positions in this
  method were unaddressed in published work. That was wrong. Multi-agent organizational models
  (Gaia 2000; MOISE/MOISE+ 2002, including role cardinality) formalised the organization as a
  specified governance object two decades ago, and capability security has specified
  non-relayable, narrowing-only delegation since Hardy's confused deputy in 1988, with macaroons
  and RFC 8693 as deployed implementations. Harness engineering is named as the third layer the
  method's model-versus-organization framing omitted.
- The characterisation of Madatha corrected: his gated phase machine with delegation receipts
  and human escalation points is an authority structure, not only an enforcement primitive. The
  v0.2 classification kept the organizational ground vacant for this method and does not hold.
- Claims reduced to three: the owner's-queue measurement proposal, the synthesis into one
  adoptable baseline, and one open question this method does not answer.
- Added agent orchestration frameworks as the nearest prior art of all: CrewAI's hierarchical
  manager process with per-agent delegation flags, and MetaGPT's software-company roles with
  document hand-off between them. Also records why they are where this method's narrow claim
  survives: CrewAI delegation is a tool the manager calls, not a credential the receiver can
  verify.
- Added what the prior art does not cover, so the correction does not overshoot: Hardy is an
  operating-system result, MOISE+ and Gaia specify designed cooperative agents with no human
  owner and no policy-memory loss, and no coding-agent harness currently implements verifiable
  grants at the session boundary.
- Qualifiers added for citing others: Kang's velocity figures are modelled rather than measured,
  and Madatha's prevalence sample is public repositories with no base rate for config cloning.

No change to any principle, role, control, profile or metric. The §2.1 span figure stands as
this practice's operating limit and is now explicitly not presented as a result.

## v0.2 -- 2026-10-19

Added

- `related-work.md`: how this method relates to published work on governing AI coding agents.
  Credits what Kang's GAIE (arXiv:2606.22484) and Madatha's deterministic control plane
  (arXiv:2606.26924) reached first, states where this method differs, records that the L1/L2
  isolation techniques are not claimed as novel, and names the four positions that appear
  unoccupied.

No change to any principle, role, control, profile or metric.

## v0.1 -- 2026-10-19

First public draft. Three documents: the method, the control reference, and the Claude Code
mapping. Eight control layers, three assurance profiles chosen per layer, six execution
patterns, nine metrics.

Developed building dethernety; see the README for scope and limits.
