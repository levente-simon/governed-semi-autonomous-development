# Changelog

Versions are dated and additive unless a change is marked **breaking**, which here means a
control was removed, renamed, or moved to a different assurance profile -- anything that
changes what an adopter has to do.

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
