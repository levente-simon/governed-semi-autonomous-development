# Governed semi-autonomous development: Reference implementation (Claude Code)

*This document is for teams who implement the method on Claude Code: it maps each role, layer, pattern and ritual to Claude Code features, with the caveats the documentation states. The method is in [method.md](method.md), the procedures in [practice.md](practice.md), the terms in [glossary.md](glossary.md), and the detail of every control in [control-reference.md](control-reference.md).*

---

## Scope of this mapping

This document maps the method to Claude Code. It is the Claude Code form of the requirements in [control-reference.md](control-reference.md) (What the tool must provide). Features named here appear in the official documentation (code.claude.com). Where Claude Code has no native mechanism, the reference implementation adds one, and the text says so. Items not confirmed are marked "(to verify)". Where this document says the reference implementation adds, runs or ships a mechanism for a High control, the message bus or a classifier, that mechanism is specified and proposed, not in use in the originating practice (README, Maturity).

### Roles

| Role | Mechanism |
|---|---|
| Owner | The human operator: applies settings, performs outward actions from a checkout agents cannot write, answers decision messages and permission prompts |
| Orchestrator | A named session (`claude -n <name>`): in its own worktree at Lite; in its own container or VM at Standard; in its own VM at High. From Standard, its role set denies fetch tools (`WebFetch`, `WebSearch` and tool-server tools that fetch), and it runs no product code: integrated suites and hot-file regeneration run in CI, and it reads the results by commit. It holds no environment authority; from Standard, the owner changes its environment through its definition, never from inside it |
| Worker | A session per worktree (`claude --worktree <name>`) at Lite; at Standard, a worktree inside the shared worker container or VM, or its own container or VM when it is assigned external ingestion or environment authority; at High, its own VM. The slice's tier is set at launch with `--model` and `--effort`, which apply to that session only. `/model` saves the choice as the user's default for later sessions unless `s` is pressed in its picker, and `/effort` saves most picks per model, so in a shared environment set tiers only with launch flags. The saved default effort is a per-model `modelSettings` entry. A top-level `effortLevel` in user settings is legacy, and newer models ignore it; a role-wide default goes in `effortLevel` in project, managed or `--settings` scope. Neither key accepts `max`. Keep `CLAUDE_CODE_EFFORT_LEVEL` unset in agent environments, because it overrides `--effort`. A `PreModelSwitch` hook can block `/model` switches, which enforces launch-flag tiers for the model; no hook covers `/effort`. Record the launch command, tier included, in the ledger. From Standard, the shared worker role set denies `WebFetch`, `WebSearch` and fetching tool-server tools. `WebSearch` runs at the provider, so an egress allow-list does not stop it |
| Specialist | Custom subagents in `.claude/agents/`. Each starts with a fresh context window, except a fork subagent, which inherits the conversation; reviewers are never fork subagents. Frontmatter sets `tools`, `disallowedTools`, `model`, `effort`, `permissionMode` and `isolation: worktree`. Claude can pass `model` and `effort` per invocation, which override the frontmatter, so a specialist's tier is set at invocation and recorded with its output. `permissionMode` is ignored when the parent session runs in `bypassPermissions`, `acceptEdits` or `auto` mode. Plugin agents ignore `permissionMode`, `hooks` and `mcpServers`, so specialists that need those live in `.claude/agents/` |
| Capability | Dynamic workflows saved in `.claude/workflows/`: rerunnable scripts that orchestrate many subagents. Skills (`SKILL.md`) hold human-run procedures. From Standard, a workflow that fetches external content runs in an ingestion environment, not in the orchestrator's |
| Peer | Another session with its own ledger and owner-applied settings |

### Layers

**L1.** Claude Code supports development containers. The documented reference container includes an egress firewall, and the documentation advises against mounting host secrets such as `~/.ssh` into the container. The sandboxed Bash tool enforces filesystem and network limits at OS level on shell commands. The documentation states that file tools, MCP servers, hooks, plugin monitors, LSP servers, the status-line and `apiKeyHelper` commands, and `excludedCommands` matches run outside that sandbox. Container or VM isolation therefore remains the boundary. `permissions.disableBypassPermissionsMode: "disable"` prevents bypass mode.

**L1, environment authority.** From Standard, for a session not assigned environment authority:

- **The boundary is the container or VM.** File tools, MCP servers, hooks and other helper processes run outside the sandbox, so the boundary sits in the environment. The agent's OS user has no root or sudo; system paths are read-only; no socket of a runtime that is neither rootless nor confined to a VM serving that session alone is mounted; the owner applies image and definition changes. A mounted Docker socket gives access to the host, and membership of the `docker` group is root. A session that needs a container runtime runs in an environment where the runtime is rootless (for example rootless Docker or Podman) or confined to a VM serving that session alone.
- **The sandbox is an optional extra layer.** It limits where shell commands, and everything they start, can write. If it is used, managed settings or `--settings` set `sandbox.enabled: true`; `sandbox.failIfUnavailable: true`, because otherwise a sandbox that cannot start runs commands unsandboxed; and `sandbox.allowUnsandboxedCommands: false`, which makes Claude Code ignore the model's `dangerouslyDisableSandbox` retry. Nothing is listed in `sandbox.excludedCommands`: excluded commands run with full user access and are matched on their text. `docker` is incompatible with the sandbox, so a session that needs a container runtime runs without it. Installs under the sandbox need their package caches in `sandbox.filesystem.allowWrite` and their registries in `sandbox.network.allowedDomains`; where the sandbox blocks a toolchain download, the environment's definition provides the toolchain. In an unprivileged container, the Linux sandbox needs `sandbox.enableWeakerNestedSandbox`, which weakens it, so the container stays the boundary. Some sandbox behaviours depend on the CLI version; kickoff verifies them on the pinned version.
- **Deny rules and hooks are a setting that can be evaded.** `deny` rules such as `Bash(sudo *)` or `Bash(npm * -g *)`, and `PreToolUse` hooks on `Bash`, match command text: a full path, `sh -c`, or a script written first and run later evades them. They catch mistakes only.

From Standard, a session assigned environment authority runs in its own container or VM. It holds root there, so its hooks and local checks are self-attested.

**L2.**

- **Worktrees.** `--worktree` gives each session its own worktree and branch. Inside one, Claude Code blocks file edits and git commands aimed at the main checkout. A shell command that writes into the main checkout without git, such as `cp` or a redirect, is not refused.
- **Shared state.** Worktrees share the repository's `.git` directory. A "Yes, and don't ask again" approval for a Bash command given in one worktree is saved for every worktree of the repository.
- **No standing approvals (all profiles).** No session gives standing approvals. The rule also covers a workflow's "Yes, and don't ask again" option. In auto mode, any "Yes" to a workflow launch writes consent to user settings, so workflow launches need care there too. Every rule lives in a settings file that the owner applies. The managed setting `allowManagedPermissionRulesOnly` hides the always-allow choices and stops rule saving, which mechanises this rule.
- **Standard and High.** A clone per environment; inside a shared worker environment, `--worktree` per worker. At High, one clone per VM. The resource registry is a project convention.

**L3.**

- **Rules.** Settings files hold `allow`, `ask` and `deny` rules. They are evaluated deny first, then ask, then allow. Examples are `Bash(git push *)` and `Read(./.env)` in `deny`.
- **Rule limits.** The documentation states that a Bash rule matches command text and "isn't a security boundary around the program". Pair rules with the sandbox or a hook.
- **Hooks.**
  - `PreToolUse` hooks block a call by exiting with code 2 or by returning `permissionDecision: "deny"`. They can also return `"ask"`, `"allow"` or `"defer"`; `defer` works only in non-interactive (`-p`) runs, when the turn makes a single tool call, and ends the turn. When several hooks answer, `deny` beats `defer`, which beats `ask`, which beats `allow`.
  - By default, a hook that crashes or times out does not block. Every enforcing hook sets `"onFailure": "block"` so that it fails closed. That setting needs Claude Code 2.1.295 or later, and it works only on `command` and `http` hooks, so every enforcing hook is one of those two types. It has no effect on `async` hooks or on `Stop`, `SubagentStop`, `TaskCompleted` and `TeammateIdle` hooks, and on `SessionStart` a failure does not block.
- **Mods.** Hook decisions never override deny or ask rules. A plugin mod that handles `tool.check` answers after rules and hooks, and a built-in guard decides what it may approve.
  - The guard loads when the machine has managed settings, or when the session's credential is a Team or Enterprise login. A long-lived `CLAUDE_CODE_OAUTH_TOKEN` does not count as such a login. API-key and cloud-provider sessions get the guard only where managed settings exist.
  - With the guard loaded, a mod cannot approve over a deny rule, an ask rule, a hook prompt or any `PreToolUse` hook block. The managed guard option `allowModsToOverrideDenyRules` re-opens the deny-rule case, so it stays off.
  - Without the guard, a mod can approve over all of them.
  - In auto mode, a call that a mod approves skips the classifier in every setup, unless the guard is loaded and the session runs with `--permission-prompt-tool` or an SDK `canUseTool` callback.
  - Rules and hooks do not cover a mod's own file and process calls.

  Agent sessions therefore run with managed settings deployed. The managed `allowManagedModsOnly` option admits only built-in mods and mods installed the organisation's way, through a managed marketplace and managed settings. A plugin copied into the cache from a GitHub, git, URL or npm source counts as a user's, even when managed `enabledPlugins` enables it, so approved mods are installed the organisation's way.
- **`dontAsk` mode.** It denies calls that would otherwise prompt. It is not a pure allow-list: file reads inside the working directories, read-only Bash commands and calls that a hook approves still run. Pair it with `Read` deny rules and hooks.
- **Managed settings.** They have the highest precedence. `allowManagedPermissionRulesOnly` makes them the only source of permission rules. It ignores rules in user, project and local settings and in `--settings`.
- **Auto mode.** Setting it up for delegations is covered under L4. The behaviour that matters here:
  - Explicit permission rules resolve first. A narrow `permissions.allow` rule, such as `Bash(npm test)`, resolves before the classifier and skips it. Broad rules such as `Bash(*)` are dropped in auto mode.
  - The classifier then reads an `autoMode` settings block with `environment`, `allow`, `soft_deny` and `hard_deny` entries, written as prose. It reads that block only from user settings, managed settings or `--settings`, never from project settings.
  - `autoMode.allow` entries are exceptions to `soft_deny` only. `hard_deny` entries block unconditionally.
  - A hook's `"ask"` still forces a prompt in auto mode, but the classifier can deny the call before the prompt appears. A grantable action can therefore be refused before the owner sees it.
  - The owner's explicit approval in conversation clears a soft block, but only when it names the action and its specifics. It never clears `hard_deny` or `permissions.deny`. Under this method such an approval is not a grant ([control-reference.md](control-reference.md), L4).
  - Refused actions are listed in `/permissions` under "Recently denied". After repeated blocks, auto mode pauses and prompting resumes. In non-interactive runs without a permission prompt tool, a blocked action is skipped.
- **Action classifier (High).** A `PreToolUse` `command` or `http` hook with `"onFailure": "block"`, matched to side-effecting tools only (`Bash`, file writes outside the worktree, tool-server tools with effects), calls the local decision engine.
  - It strips the model's stated reason before classification: the `description` field of a `Bash` call, and comments in the command.
  - It reads the slice plan, or the batch plan that covers the slice, at the commit the ledger records as approved, plus the delegation manifest and the provenance of preceding content.
  - A named step in the same hook checks scope deterministically against the approved plan and the closed action classes, before the engine is called.
  - Where signed grants are used, the same hook checks for a valid grant first, because when several hooks answer, `deny` beats `allow`: a separate grant hook could not override the classifier's refusal. A call that a verified grant covers is not re-asked; the classifier can still deny it for misuse.
  - It returns `"deny"` with the reason recorded for batch review. For a grantable action class it follows the session's L4 grant path: `"ask"` at Standard L4, which shows the permission prompt in the receiving session; `"deny"` naming the class at High L4.
  - A hook's `"allow"` does not override deny or ask rules.
  - No `--permission-prompt-tool` is configured, except one that reaches only the owner. A prompt tool wired to the mailbox would let the orchestrator answer, which is a relayed grant.
- **Protecting enforcement material.**
  - Deliver permission rules and hooks as managed settings, with `allowManagedHooksOnly` so that only managed hooks run. That option also blocks hooks declared in agent frontmatter, and plugin hooks unless managed `enabledPlugins` force-enables the plugin by its full `plugin@marketplace` ID. The enforcement plugin is therefore force-enabled there, and specialists rely on no frontmatter hooks.
  - Writes to Claude Code's protected paths are never auto-approved, except in bypass mode or in plan mode when bypass is available. In a session started with `--restricted`, the classifier cannot approve protected-path writes. That mode also removes command-running tools unless they are named, so it suits read-only specialist and verifier sessions. It loads only managed settings and `--settings`, so a restricted session's role set goes there.
  - Keep the trust anchor, manifest and revocation records outside the working directories, owned by another OS user and read-only to the session's user. This holds for any subprocess, not just Claude Code's file tools.
- **Agent tooling.**
  - `strictKnownMarketplaces` (managed) restricts the plugin marketplaces users can install from.
  - `allowedMcpServers` restricts which MCP servers can be added.
  - Pin the CLI version in the environment definition, at 2.1.295 or later for `onFailure`, and set `DISABLE_UPDATES`, which also blocks a manual `claude update`.
  - Project-level `.mcp.json` and plugin enablement are protected paths.

**L4.**

- **Permission sets.** Role permission sets are settings files. Apply them at kickoff through `--settings` or as managed settings in the environment image, from files the session cannot write. Where `allowManagedPermissionRulesOnly` is set, they must be managed settings. No settings file Claude Code reads for the session, user and local settings included, is writable by it, and no location Claude Code reads settings from is in a directory it can write. Whether the worktree's `.claude/settings.json` and `.claude/settings.local.json` locations can be kept unwritable is not confirmed (to verify). A plugin cannot carry them: the documentation states that a plugin's `settings.json` applies only the `agent` and `subagentStatusLine` keys.
- **Planned delegations.**
  - Each planned delegation becomes a narrow `allow` rule in the role's permission set.
  - Where the session runs in auto mode, it also becomes an `autoMode` `allow` or `environment` entry in managed or `--settings` scope, or in user scope only where the session cannot write user settings.
  - An `autoMode.allow` entry clears only soft blocks. A delegation that touches a `hard_deny` class needs the narrow `permissions.allow` rule, which resolves before the classifier. The other option is a change to that `hard_deny` entry, which is impossible for managed entries.
  - `claude auto-mode config` prints the effective configuration for the kickoff record.
- **Standard grants.** Grantable action classes sit in `ask`. The owner answers the permission prompt in the receiving session. No `PermissionRequest` hook or prompt tool answers for the owner. With no standing approvals, each answer covers one action.
- **High grants.** The delegation manifest and signed grants have no native mechanism. The reference implementation adds them:
  - The owner's public key is written into the environment image. The private key stays on the owner's device.
  - A `PreToolUse` hook, with `"onFailure": "block"`, checks each grantable action against the signed manifest entries. It checks the signature, the receiving session instance, the action class and expiry, and the absence of a revocation on the owner-only ref. It records each use in the slice ledger, and it denies by default. Where the action classifier runs, this check is the first step of the classifier hook (L3).
  - Grantable actions stay out of `deny` and `ask`, because a hook cannot override those lists. Actions that are never grantable stay in `deny`.
  - Whether a hook's `"allow"` decision also bypasses the auto-mode classifier is not documented (to verify). High sessions with grantable actions therefore run without auto mode, or keep grantable classes clear of classifier blocks.
- **Forks and branches.** Session approvals do not carry into a `--fork-session` process, because it is a new process. `/branch` runs in the same process and does carry them. `/fork`, a background copy, carries the model, permission mode, effort level, added directories and "don't ask again" grants. With agent view turned off, `/fork` starts a forked subagent that inherits the conversation, so it is never used for review. None of these is ever a grant, and tier escalation uses none of them (Execution patterns, below).

**L5.** Branch protection, required CI, code-owner rules, signed tags and reproducible builds live on the git host and the build system.

- **Local review is an input, not CI.** The local `/code-review` and `/security-review` commands are inputs to the deliverable review. `/code-review ultra` runs the review in the cloud rather than in the agent environment. Either way, the result is an agent's report and never stands in for CI.
- **Reading CI.** Sessions read results from the host by commit, never from another session's report. On GitHub, for example: `gh run list --commit <sha> --json name,status,conclusion`. `conclusion` is empty while a run is queued or in progress: that is "pending", not green. `gh run list` shows Actions runs only; required checks from other CI show in `gh pr checks`.
- **The token for reading CI.** A GitHub App installation token with Checks, Actions, Commit statuses and Metadata read, issued per environment, never the owner's `gh` login. Fine-grained personal tokens have no Checks permission; with Actions read they cover `gh run list` only. Kickoff tries `gh pr merge` and a tag creation through `gh api` with it, and records the refusal.
- **Exact commit.** Branch protection requires branches to be up to date before merging, or the repository uses a merge queue.
- **Secrets and tokens.** On GitHub, `pull_request` withholds Actions secrets only from fork and Dependabot PRs; branches pushed to the same repository receive them, and Dependabot runs receive Dependabot secrets. A read-only default for workflow permissions is raised by a workflow file that asks for more, and only fork PRs are forced down to read. So "no write token for agent changes" holds only where agent PRs come from a fork; otherwise it rests on the measures below:
  - set the default workflow permissions to read-only, and keep repository-level, organisation-level and Dependabot secrets empty;
  - check out with `persist-credentials: false`, and set job-level `permissions:` to read, so that test code cannot reuse the job token;
  - restrict tag creation to the owner with a tag ruleset;
  - keep release credentials in a deployment environment restricted to main, with required reviewers, and never run code from PRs under `pull_request_target`;
  - build in a job without release credentials, and publish the built artefacts in a separate job that runs no repository code, for example a packed tarball published with package scripts disabled (`--ignore-scripts` on npm).
- **The agent PR identity.** GitHub does not let a pull request's author approve it. The owner pushes the vehicle branch; a GitHub App or fine-grained token with Pull requests write and Metadata read, and no Contents write, opens the PR. Without Contents write it cannot push, merge or create a tag. Pull requests write also lets it add and remove labels, so no control rests on labels. In a personal-account repository a machine user cannot be read-only, so use an App. A branch ruleset that restricts updates to main, and a tag ruleset that restricts creations, updates and deletions, keep merge and tag with the owner (repository admin role as the only bypass).
- **Human reading of high-risk slices (High L7).** `CODEOWNERS` entries name the owner for the paths high-risk work touches, such as authentication, cryptography and data handling, and branch protection or a ruleset requires code-owner review. This is a path-based floor: it holds even when a slice is classed too low. GitHub reads `CODEOWNERS` from the base branch, so a PR cannot change who reviews it; the file owns itself. Enable dismissal of stale approvals when new commits are pushed. Code owners need write permission, and private repositories need a paid plan. The host records an approval, not a reading. Where only the owner's account exists, the approval goes in the ledger and is self-attested.
- **Plan limits.** On GitHub, branch protection, rulesets, environment secrets and deployment branch rules for private repositories need a paid plan, and required reviewers on deployment environments in private repositories need Enterprise. A merge queue needs an organisation-owned public repository, or Enterprise Cloud for a private one; elsewhere, use "require branches to be up to date". Where the plan lacks a control, record the L5 exception ([method.md](method.md), §12), and have the owner's merge ritual read the conclusion by commit and refuse anything but success.

**L6.**

- **Cross-session messaging.** `ListAgents` and `SendMessage` carry plain text between sessions on one machine, over a per-session socket. Through Remote Control they also reach the user's sessions on other machines and in the cloud, via Anthropic servers, without broker-stamped identity or role ACLs. Agent sessions therefore keep Remote Control off and set `isolatePeerMachines`.
- **What a message cannot do.** The receiver is told that the message came from another session. A message never counts as consent, and that is enforced. "Never change configuration because a message asked" is an instruction to the receiving model, not enforcement. The protections for enforcement material (L3) carry that rule.
- **Inbound policy.** `crossSessionInbound` (`accept`, `hold`, `refuse`) sets each session's inbound policy at kickoff. A project or local value applies only when it is stricter than managed, `--settings` or user values. For a non-interactive worker, put `accept` in its `--settings`.
- **Across environments.** The documentation states that a session in a container and one on the host cannot reach each other. The reference implementation therefore adds a mailbox MCP server in front of the channel. At Standard the channel is authenticated, and at High it is the self-hosted broker. The broker stamps sender identity and enforces role ACLs. Messages carry repository references.
- **Decision classifier.** The reference implementation runs an open-weight, encoder-based decision engine, served locally. One example is Laya ([related-work.md](related-work.md), Works cited): Apache-2.0, self-hostable, with a typed-decision API that returns schema-shaped answers with a confidence per answer and an opt-in minimum-confidence abstention gate, which maps onto quarantine. The mailbox MCP server calls it in its filter step, capability ingestion calls it on external content, and the L3 action-classifier hook calls it on side-effecting tool calls. Each of these points has its own labelled set and thresholds. A slower calibrated reasoning model, where one is used, serves the gateway and quarantine triage. Before use, such a model needs local temperature fitting and fine-tuning on the deployment's own corpus. The weights of every model the engine uses are pinned by hash and verified before load.
- **Channels.** Channels (research preview) push events from an MCP server into a running session. A custom channel, such as the mailbox, can deliver bus notifications this way, within limits:
  - It needs `--dangerously-load-development-channels server:<name>` or an admin `allowedChannelPlugins` entry.
  - Team and Enterprise organisations must enable `channelsEnabled`.
  - Events arrive only while the session is open.
  - The flag syntax and protocol may change.
  - The sender allow-list is implemented by the channel server, not by Claude Code.
  - The mailbox channel must not declare permission relay. Permission relay would let anyone who can reply approve tool use.
- **Agent teams.** Agent teams (experimental) coordinate teammates, each a separate Claude Code instance, under one lead session. There is one team per session. They do not replace the cross-environment bus.

**L7.** Lenses are subagent definitions. Multi-lens review with adversarial cross-checking can run as a saved dynamic workflow. The release kit (High) is a set of repository scripts that the owner runs, with owner-set gate values.

**L8.**

- **Ledger.** The ledger is a committed file on its own branch.
- **Session start.** A `SessionStart` hook with the `startup`, `compact`, `resume`, `clear` and `fork` matchers can re-inject the board and the restated limits. `--fork-session`, `/branch` and `/fork` fire `fork`, not `resume`; `/clear` fires `clear`. Injected context is capped at 10,000 characters, so the board plus the limits must fit. The same hook runs the board-freshness check ([practice.md](practice.md), §7), the start self-check for a new session and the re-check on resume ([practice.md](practice.md), §2), and appends the result to the kickoff record. A `SessionStart` hook cannot stop the session, so a failed check is logged, and the session stops itself or the orchestrator stops it.
- **Periodic re-injection.** A `Stop` hook fires at the end of every turn. Once the configured interval has passed, it returns the restated limits as `additionalContext` ([practice.md](practice.md), §7). `Stop` does not fire in subagents; use `SubagentStop` there.
- **Compaction.** A `PreCompact` hook can block compaction (exit code 2). Blocking an auto-compaction triggered by a context-limit error makes the request fail. Use the hook to commit the ledger first, not to refuse compaction.
- **Telemetry.** OpenTelemetry export supports metrics, including per-session token use for cost per tier.
- **Transcripts.** Their format is documented as internal, so the ledger stays authoritative.

### Rituals

Rituals are skills (`SKILL.md`). A human runs one as a slash command (`/skill-name`). Set `disable-model-invocation: true` on any ritual with side effects, so that the model cannot start it on its own. The reference set, by purpose:

| Ritual | Runs |
|---|---|
| Pre-PR gate | The checks for the session's profile ([control-reference.md](control-reference.md), L7), the re-check and, for a merge vehicle, the checkpoint duties; lists changed protected paths and, at High L7, the high-risk slices in the PR body; refuses to open the PR if any check fails |
| Post-merge ritual | Sync, branch pruning after confirmation, the next integration branch, the re-check after a protected-path merge, follow-up list (for example a pending mirror push) |
| Public-mirror push | Pre-flight scans, push, mirror-side PR |
| Maintenance sweep | The sustain-mode loop, with separate entry points for the full sweep, the emergency path, the read-only audit, finalisation and abandonment |

A ritual that fans out to many reviewers can call a saved dynamic workflow for that step. Interactively, the launch prompts each time, because every profile forbids standing approvals. In non-interactive runs it needs a `Workflow(<name>)` allow rule. Plugin workflows are namespaced (`/plugin:name`).

### Execution patterns

| Pattern | Mechanism |
|---|---|
| P1 Single session | One interactive session; specialists as subagents |
| P2 Sequential slices | A fresh session per slice, or `/compact` with a brief at each slice boundary; the plan file on the ledger ref holds per-slice status |
| P3 Scripted fan-out | Dynamic workflows (saved scripts in `.claude/workflows/`) |
| P4–P6 Teams | Named sessions per role (`claude -n <name>`): orchestrator, function leads and workers, each in its own worktree or environment |

Tier escalation follows [practice.md](practice.md) (§4). Launch the fresh session with the recorded launch command and the stronger tier's `--model` and `--effort`, with its own `-n` name, in the slice's worktree checked out at the last reviewed commit; at High, in a new VM. Its `SessionStart` hook runs the start self-check.

### Sessions and recovery

| Need | Mechanism |
|---|---|
| Resume | `claude --resume <name>` (resumes directly when the name matches one session, otherwise opens the picker) or `--continue` |
| Branch an experiment | `--fork-session` with `--resume` or `--continue`, `/branch`, or `/fork` for a background copy |
| Reduce context | `/compact [instructions]` at a safe point, with the written brief as the instructions ([practice.md](practice.md), §7, Planned compaction) |
| Restore launch configuration | `--settings`, `--add-dir`, `--mcp-config`, `--plugin-dir` and similar flags are not restored on resume. Record each session's launch command in the ledger, and re-pass it in full on every resume |

### Packaging

The reference implementation ships as:

1. **templates** (see Templates, below);
2. **the enforcement layer, as a plugin plus a settings bundle (or managed settings).**
   - The plugin carries the hooks, ritual skills, capability workflows and the mailbox MCP server. Managed `enabledPlugins` force-enables it, or `allowManagedHooksOnly` blocks its hooks.
   - The settings bundle carries the role permission sets, the `autoMode` entries and the specialist definitions that need `permissionMode` or `mcpServers`. Plugin agents ignore those fields. Specialists carry no frontmatter hooks, which `allowManagedHooksOnly` blocks; their hooks live in managed settings or the plugin.
   - The bundle's `autoMode` entries are applied in managed or `--settings` scope, or in user scope only where the session cannot write user settings, because the classifier ignores them in project and local settings.


## Templates

Each template lists its purpose and required fields. The artefacts themselves are described in [method.md](method.md) (§3) and [practice.md](practice.md).

| Template | Purpose | Required fields |
|---|---|---|
| **Functional specification** | What and why, with no mechanism | Problem and users; goals and non-goals; user-visible behaviour; acceptance criteria (numbered); constraints; risks; open questions; decision references |
| **Technical specification** | How, with architecture and gap analysis | Specification reference; architecture and data flows; gap analysis (existing system as verified, with evidence); design decisions with alternatives; what was removed by the simplicity review; security considerations; open decisions; criteria-to-design trace |
| **Implementation plan** | The work, shaped by the execution pattern | Execution pattern (per plan segment where patterns compose); plan form for that pattern; slices or lanes with dependencies; complexity-and-risk class (including whether the slice is high-risk) and tier per slice; environment needs per lane, and environment authority per lane or slice with its scope; checkpoints, merge vehicles and release vehicles; ownership partition including shared hot files; acceptance-criteria mapping; drift rule; checkpoint-review trigger; estimates in calibrated units |
| **Kickoff prompt** | Starts a session with role, limits and recovery source | Role; session identity and instance; execution pattern; pinned orchestrator; environment; tier; profile per layer; ledger ref; brief path; restated limits; launch command |
| **Kickoff record** | Proves the configuration in both directions | Environment and launch configuration; session; profile per layer; refused denied action; each planned delegation exercised and allowed; each enforcement-material write refused; fail-closed test; credentialed-action refusals (push to the public host, merge and tag through the host's API); from Standard, environment-authority assignments, the environment-authority refusals and the container-runtime check; effective configuration reference; references to recorded exceptions; re-check entries (trigger, checks run, result); date (system clock) |
| **Common brief** | Rules every agent follows | Scope; environment constraints, including that environment changes go through the owner and who holds environment authority; routing rule; slice routine; claims-from-output rule; incident reporting |
| **Role brief** | Rules for one role or lane | Owned files and resources; permitted tools; forbidden actions; test commands as CI runs them; hand-over expectations |
| **Ledger: board** | Current state on one screen | Waiting on owner (item, since, blocks); slices with owner, status, complexity-and-risk class and tier; workers with declared configuration; checkpoints; active grants with ID, receiver instance, scope and expiry; open exceptions with review dates; link to the kickoff record; restated limits; last updated (system clock) |
| **Ledger: log** | Append-only history | Segment header with carry-forward summary; timestamp (system clock); actor; event type; artefact or commit reference; outcome |
| **Decision message** | One decision for the owner | Decision ID; TL;DR; context reference; lettered options; recommendation with reasons; safe default and its window (within baseline only); signing rendering, for grants |
| **Decision register entry** | One owner decision on record | Decision ID; date (system clock); question; options; choice; rationale; decided by; for an exception: kind, named acceptor and review date; superseded by |
| **Delegation manifest** | Who may do what | Kickoff list of grantable action classes; per grant: unique grant ID, grantor, receiving session instance, action class, target, expiry, baseline or grant, signature (High); revocations (signed, referencing grant ID, on the owner-only ref) |
| **Slice plan** | The worker's plan before work | Slice ID; goal; complexity-and-risk class (including whether the slice is high-risk) and tier; criteria referenced; files and resources touched; hot-file changes requested; approach; assumptions and decisions taken, including every assumption about anything outside the ownership partition; tests (red-first for fixes); risks; environment needs, and any use of environment authority with the assignment it rests on; deviations, each re-approved |
| **Slice ledger** | The worker's record of one slice | Slice ID; state; approved plan commit; tier and launch command per session; grant uses (grant ID, action, time); changes made under environment authority; escalations with their reason |
| **Hand-over note** | The worker's account after work | Slice ID; commits; changes; assumptions and decisions taken, including every assumption about anything outside the ownership partition; tests run with excerpted, secret-scanned output; environment changes made; criteria met; open items; leftovers removed; for an escalation, written before the failing session stops: the side branch holding its work in progress, the failure notes and the assumptions so far, which the fresh session receives together with the reviewer's written findings (if the session cannot write it, the orchestrator writes the failure notes) |
| **Release run sheet** | The checklist or kit run of one release vehicle | Vehicle ID; contents; CI conclusion for the release commit; artefact digests; the publish step, which runs no repository code; tag; user-path end-to-end result (Standard); at High, kit revision, phases with gates and gate owners, rollback anchors, evidence per phase, freeze scope and window; pre-release check results; at High L7, the human reader of each high-risk slice; verifier verdict (High, sole owner: signed verdict over the release digest, or the decision reference for a recorded downgrade to Standard) |
| **Override entry** | Records a forced or pinned dependency version | Package; forced version; reason; advisory references; owner decision reference; review date; removal condition |
| **Dismissal entry** | Records an advisory judged not applicable | Advisory ID; affected package and version; verdict; reason with evidence; decided by; re-check date; status (active or reopened) |
| **Abandonment entry** | Records a sweep wound down without a merge | Run ID; vehicle reference; reason; items reopened; decided by; follow-up |
| **Sweep run state** | Tracks one maintenance run to finalisation | Run ID; scope (full, emergency, partial); start (system clock); items reconciled with verdicts; vehicle reference; CI result; merge reference; finalisation status; ledger entries written |
| **Retro record** | Output of the learning loop | Incidents with root-cause classes; rules and mechanisms added; metrics summary; changes to method, templates and profiles |


## Operator's handbook (planned)

Tool-specific operating practice is kept out of the method and will live in a separate operator's handbook. Planned topics:

- terminal multiplexer layout for orchestrator, worker and specialist sessions;
- resume, branch and fork: which command to use when, and what each restores;
- compaction briefs: what the brief contains and when to write it;
- scheduled wake-ups across usage limits and the owner's absence;
- remote access to running sessions, and its limits under this method;
- restart order after a machine or environment restart;
- permission prompts and reconnects: batching them into one owner visit.
