# Changelog

Versions are dated and additive unless a change is marked **breaking**, which here means a
control was removed, renamed, or moved to a different assurance profile -- anything that
changes what an adopter has to do.

## v0.7 -- 2026-10-21

Changed

- **Choosing a pattern by coupling, not size.** Sequential or tightly coupled work stays with one
  agent (P1, P2); multi-agent patterns are for work that splits into parts independent in
  substance. Tool load counts as a coordination cost.
- **Patterns compose.** A plan may assign a pattern per segment, for example P2 for a serial core
  and P4 for the branches that split from it.
- **P4's integration condition is explicit:** workers write only on their own branches within
  their ownership; the orchestrator alone integrates.
- **"No idle workers" is replaced** by "idle capacity only for fitting, independent work". With
  worker tiers, assignment follows fit, not idleness; an idle worker is the right state when the
  remaining work is coupled.
- **Tier escalation states its criterion:** fork the session when the slice made progress the
  stronger tier needs; start fresh with the hand-over, assumptions and failure notes when the
  context itself is the problem.

Added

- **Assumptions and decisions taken** in slice plans and hand-over notes; the orchestrator passes
  on those that touch other slices.
- **Context sharing by profile** (control reference, L6): at Lite, where sessions share one trust
  domain, fuller context may be shared; at Standard and High, references only.
- Optional tagging of incidents with a published multi-agent failure taxonomy (learning loop).
- `related-work.md`: a section on when multiple agents help (Anthropic 2024 and 2025, Cognition
  2025 and 2026, Kim et al., Cemri et al.).

- **Breaking: session isolation at Standard follows authority and input trust, not session count.**
  Standard no longer requires an environment per session. It requires the orchestrator in its own
  environment, allows workers of equal authority to share one (a worktree each), and gives its own
  environment to every session that ingests untrusted content or runs destructive work. Credentials
  are per environment at Standard and per session at High. A new subsection in the control
  reference (L1) states why sessions are separated, what that does not buy, and that separate
  credentials per environment also spread usage and cost within the provider's terms.

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
