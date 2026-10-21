# Governed semi-autonomous development

A tool-agnostic reference method for governing teams of AI agents that build production software. Agents do the work; humans keep authority over everything irreversible or outward-facing. Trust comes from layered controls, not from the agents behaving well.

The method is designed around three properties at once: **secure**, **correct** and **efficient**. Efficiency never overrides the other two. It is not a security guide: it describes how to organise, run and control agent teams, with security as one property among three.

**Status:** v0.5 draft, 2026-10-21. Corrections, questions and adoption reports are
welcome as [issues](../../issues).

## Documents

| Document | For | Content |
|---|---|---|
| [method.md](method.md) | Leads and founders deciding whether to adopt it | Principles, roles and execution patterns, artefacts and the ledger, lifecycle and documentation chain, control layers in summary, specialists, capabilities, federation, quality and efficiency techniques, learning loop, assurance profiles, metrics, glossary |
| [control-reference.md](control-reference.md) | Security and platform teams implementing it | Every control per layer (what it prevents, what it serves, its cost, its profile, its proof), the delegation and signing model, the message bus, the decision classifier, the release kit, a threat-model plan, the control-to-profile map |
| [claude-code-reference.md](claude-code-reference.md) | Teams implementing it on Claude Code | How roles, layers, patterns and rituals map to Claude Code features, with the documented caveats; templates; the planned operator's handbook |
| [related-work.md](related-work.md) | Readers placing this against published work | The bodies of work this method is built out of, why they do not simply apply to this setting, and what the method is. No novelty claim |

Start with [method.md](method.md). The others go deeper on demand.

## At a glance

- **Roles:** owner, orchestrator, workers, specialists, capabilities, peers.
- **Execution patterns:** single session, sequential slices, scripted fan-out, project team, function teams, matrix.
- **Eight control layers:** infrastructure isolation, session isolation, tool control, permissions and delegation, repository, messages, process, state and audit.
- **Assurance profiles:** Lite, Standard and High, chosen per layer.

## Origin

This method comes out of building [dethernety](https://dether.net), a commercial,
graph-native threat modeling platform: a solo build since 2025, open-sourced in March 2026,
and delivered through twelve minor releases since. The method is not how that work started.
It emerged partway through, as the work outgrew a single session and moved to agent teams,
and the controls here are what that transition produced.

It is a working method rather than a proposal. Most of its controls exist because something
went wrong without one; the learning loop in [method.md](method.md) §11 is how they arrived.
The evidence base is a single product, which is stated here because it bounds what the
method can claim.

## License

© 2026 Levente Simon. This work is licensed under the [Creative Commons Attribution 4.0 International License](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0). You may share and adapt it for any purpose, provided you give appropriate credit, link to the license and indicate if changes were made. The full text is in [LICENSE](LICENSE).

Suggested attribution: *"Governed semi-autonomous development" by Levente Simon, licensed under CC BY 4.0.*
