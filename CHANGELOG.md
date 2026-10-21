# Changelog

Versions are dated and additive unless a change is marked **breaking**, which here means a
control was removed, renamed, or moved to a different assurance profile -- anything that
changes what an adopter has to do.

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
