# Governed semi-autonomous development

A tool-agnostic reference method for governing teams of AI agents that build production software. Agents do the work; humans keep authority over everything irreversible or outward-facing. Trust comes from layered controls, not from the agents behaving well.

The method is designed around three properties at once: **secure**, **correct** and **efficient**. Efficiency never overrides the other two. It is not a security guide: it describes how to organise, run and control agent teams, with security as one property among three.

**Status:** v0.10.2 draft, 2026-10-28. Corrections, questions and adoption reports are
welcome as [issues](https://github.com/levente-simon/governed-semi-autonomous-development/issues).

## Documents

| Document | For | Content |
|---|---|---|
| [method.md](method.md) | Leads and founders deciding whether to adopt it | What the method requires and why: principles, roles and execution patterns, artefacts and the ledger, the lifecycle in outline, control layers in summary, specialists, capabilities, federation, quality and efficiency techniques, learning loop, assurance profiles, metrics |
| [practice.md](practice.md) | Whoever runs a team: an orchestrator, an owner, or tooling that launches teams | The procedures: documentation chain, kickoff and re-checks, slice routine, routing and escalation by tier, checkpoints and vehicles, documentation fold, session continuity, sustain mode, the incident record |
| [glossary.md](glossary.md) | Every reader | The terms used across the documents, in alphabetical order |
| [control-reference.md](control-reference.md) | Security and platform teams implementing it | Every control per layer (what it prevents, what it serves, its cost, its profile, what keeps it, its proof and whether that proof is independent), the delegation and signing model, out-of-band CI, the message bus, the action and decision classifiers, the release kit, a threat-model plan, the control-to-profile map |
| [claude-code-reference.md](claude-code-reference.md) | Teams implementing it on Claude Code | How roles, layers, patterns and rituals map to Claude Code features, with the documented caveats; templates; the planned operator's handbook |
| [related-work.md](related-work.md) | Readers placing this against published work | A comparison with current frameworks and published work, the bodies of work this method is built out of, why they do not simply apply to this setting, and what the method is. No novelty claim |
| [standards-map.md](standards-map.md) | Security leaders and auditors who report against a framework | A crosswalk from MITRE ATLAS, the OWASP Top 10 for Agentic Applications and the NIST SSDF to the method's controls, with a relation and a note per item, and the gaps it shows. Placement, not a compliance claim |

Start with [method.md](method.md), and keep [glossary.md](glossary.md) at hand. Read [practice.md](practice.md) when you run a team. If you arrive with a security framework, start with [standards-map.md](standards-map.md). The others go deeper on demand.

## At a glance

- **Roles:** owner, orchestrator, workers, specialists, capabilities, peers.
- **Execution patterns:** single session, sequential slices, scripted fan-out, project team, function teams, matrix.
- **Eight control layers:** infrastructure isolation, session isolation, tool control, permissions and delegation, repository, messages, process, state and audit.
- **Assurance profiles:** Lite, Standard and High, chosen per layer.
- **Limits:**
  - what is accepted at each profile, including product code that reaches main without a human reading it: [method.md](method.md) §12, residual risks;
  - what is out of scope, such as the data sent to the model provider and agents inside the product: [method.md](method.md) §0;
  - placement against security frameworks: [related-work.md](related-work.md), Standards and frameworks;
  - a crosswalk to MITRE ATLAS, the OWASP agentic Top 10 and the NIST SSDF, which is not a compliance claim: [standards-map.md](standards-map.md).

## Origin

This method comes out of building [dethernety](https://dether.net), a commercial,
graph-native threat modeling platform: a solo build since 2025, open-sourced in February 2026,
and released continuously since. The method is not how that work started.
It emerged partway through, as the work outgrew a single session and moved to agent teams,
and the controls here are what that transition produced.

It is a working method rather than a proposal, at the profiles the Maturity paragraph states. Most of its controls exist
because something went wrong without one; the learning loop in [method.md](method.md) §11 is
how they arrived.
The evidence base is a single product, which is stated here because it bounds what the
method can claim.

**Maturity.** In the originating practice, the method runs at Lite and Standard, depending on the layer. The High controls, the message bus and the classifiers are specified and proposed. The method is a draft and changes quickly; the changelog marks breaking changes.

## License

© 2026 Levente Simon. This work is licensed under the [Creative Commons Attribution 4.0 International License](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0). You may share and adapt it for any purpose, provided you give appropriate credit, link to the license and indicate if changes were made. The full text is in [LICENSE](LICENSE).

Suggested attribution: *"Governed semi-autonomous development" by Levente Simon, licensed under CC BY 4.0.*
