# Governed semi-autonomous development: Practice

*This document is for whoever runs a team under the method: an orchestrator, an owner, or tooling that launches teams. It holds the procedures; the rules they serve and their reasons are in [method.md](method.md), the controls in [control-reference.md](control-reference.md), the terms in [glossary.md](glossary.md), and one tool mapping with the templates in [claude-code-reference.md](claude-code-reference.md).*

---

## 1. Documentation chain

Shape and Plan run as a chain of documents. Each has a review gate and a decision step, and every decision goes to the decision register. The chain's depth follows the pattern: at P1, a note with acceptance criteria; at P2, one combined specification and plan; the full chain at P4 to P6. Review depth at every stage follows the profile ([method.md](method.md), §6, §9).

| Stage | Document | Content | Review gate | Decision step |
|---|---|---|---|---|
| a | **Functional specification** | What and why, no mechanism; written mainly in open conversation with the owner | The owner, plus agent lenses for domain, UX and risk as the profile requires | Owner accepts the specification |
| b | **Technical specification, architecture and gap analysis** | How; the gap analysis verifies the existing system by reading it, never by assuming it | Review scaled to the profile, including a simplicity check that removes what is not needed | Owner accepts the design and its open decisions |
| c | **Implementation plan** | Shaped by the execution pattern ([method.md](method.md), §2.1, Plan form) | Reviewed before execution | Owner "go" |
| d | **Slice execution** | The slice routine (§3) | Per slice | Approver per pattern |
| e | **Close-out** | Status, archive, retro | Retro | Owner accepts method, template and profile changes |

## 2. Setup, kickoff and session checks

### Setting up Lite

Before the first kickoff, set up in this order:

1. Check the tool against [control-reference.md](control-reference.md) (What the tool must provide), and record each missing row.
2. The environment: a dedicated development machine or one isolated environment ([control-reference.md](control-reference.md), L1).
3. Deny rules for push, publish and secret reads, with the enforcement material read-only to the session (control reference, L3).
4. The ledger ref and the decision register ([method.md](method.md), §3).
5. Required CI on the host, or the recorded host-plan exception (control reference, L5).
6. The protected-path list, in a protected file (control reference, L5).
7. The start self-check and the re-injection of limits (below, and §7).
8. The pre-PR gate and post-merge rituals (control reference, L7).

### Launching sessions

The orchestrator launches sessions within its delegation, from launch commands recorded in the ledger, where the tool allows. Otherwise launching is an owner chore, counted in the owner-attention budget ([method.md](method.md), §10). The owner launches whenever the launch configuration changes.

### Kickoff tests

Kickoff tests each environment and its launch configuration in both directions:

- a denied action is tried and refused;
- each planned delegation is exercised once, harmlessly, and allowed;
- each piece of enforcement material is modified and the write refused; in an environment whose session holds environment authority, the write is tried without root and recorded as self-attested ([control-reference.md](control-reference.md), L1);
- each enforcing check is run with its input removed, and the guarded action is refused; this includes a grantable action tried with its grant source removed;
- a credentialed action is tried and refused with every host credential in the environment: a push to the public host, and a merge or tag through the host's API;
- from Standard, in each environment whose sessions hold no environment authority, a privilege escalation, a write to a system path and a lab action are tried, from a process the session starts, and refused; any container runtime a session reaches is confirmed rootless or confined to a VM serving that session alone.

At Lite, items without a counterpart (planned delegations, grants) are skipped. The results go into a committed kickoff record, which the board links to.

### Start self-check and re-checks

A new session started with an already-tested launch command, in an already-tested environment, runs the **start self-check**. It confirms its identity, re-injects the restated limits from the ledger, and tries one denied action, one enforcement-material write and one credentialed action. It needs no owner action. It runs from the tool's start hook, or, where the tool has none, the command that launches the session gives it to the session as its first task. A launch command that differs only in its tier (model and reasoning setting) counts as tested. Any other change to the launch configuration needs a kickoff. An environment rebuilt from a definition change counts as tested when the change touches no enforcement material, OS user or group, mount, credential or network setting; its sessions run the start self-check and, from Standard, the environment-authority refusals and the container-runtime check, and a newly planned delegation is exercised once. Any other definition change needs a kickoff.

Enforcement can drift after kickoff, so the same check runs again as a **re-check**:

- after every restart or resume;
- after every merge that changes a protected path;
- at each vehicle.

The results are appended to the kickoff record. The checks are mechanised, not left to discipline. Where the tool cannot block a session from its start hook, a failed check is logged and the session stops itself, or the orchestrator stops it.

*Serves S, C. Cost: one kickoff per environment and launch configuration; one check per new session and per re-check trigger. Profile: all.*

## 3. The slice routine

1. The worker confirms it understands the slice.
2. It writes the slice plan and reviews it itself.
3. It sends "plan ready".
4. The orchestrator reviews the plan, with specialist lenses scaled to the slice's risk, checks its assumptions against those of the slices in flight, logs each assumption it passes on to another slice, checks any use of environment authority against the owner's assignment, and says "go" or returns corrections. The approved plan's commit is recorded in the ledger.
5. The worker implements, red-first for fixes ([method.md](method.md), §9).
6. It runs the tests the way CI runs them.
7. A final review covers the slice diff together with the test output.
8. The worker commits a hand-over note.
9. The orchestrator merges into the integration branch. In P1 and P2, the agent session merges on the owner's approval. Integrated suites and hot-file regeneration run in CI on the integration branch, or at Lite in a worker-authority environment. The orchestrator reads the result by commit, and commits regenerated hot files that the run returns as an artefact. One CI run per vehicle stays the target, so slice merges are batched before an integrated run.

The approver depends on the pattern ([method.md](method.md), §2.1, One slice routine).

Mechanical slices that a reviewed batch plan covers may skip a per-slice plan until they deviate. A deviation updates the plan first and takes effect only after the approver's "go" on the updated plan.

## 4. Routing and escalation by tier

### Routing

The orchestrator sets each slice's tier from its complexity-and-risk class ([method.md](method.md), §10):

- The strongest tier takes slices that change a plan, touch security or state, cut across the codebase, or change a contract between lanes. Every high-risk slice runs at the strongest tier.
- Cheaper tiers take mechanical slices behind deterministic guards.
- The tier is a launch parameter of the session the owning worker starts for the slice. Where workers keep fixed tiers instead, reassigning a slice transfers ownership of its files. The transfer is recorded on the board, and the previous owner stops touching them.
- The ledger records the tier per slice. Escaped defects per tier are compared within one complexity-and-risk class, and cost per tier comes from per-session telemetry.

### Tier escalation

When a slice meets the escalation trigger ([method.md](method.md), §10):

1. The failing session commits its work in progress to a side branch and writes a hand-over note with its assumptions so far and its failure notes, then stops. If it cannot, the orchestrator writes the failure notes from the review and the log. The stop is recorded in the ledger. At High, the stopped instance's credentials are revoked.
2. A fresh session is launched on the stronger tier (§2, Launching sessions) with the recorded launch command and its own session name; at High, in a new VM. It starts from the slice's last reviewed commit (the approved plan commit if no code has passed review). Its input is the slice plan, the slice ledger, the reviewer's written findings and the failure notes. The side branch is a reference, not a starting point, and the failure notes are hypotheses, not findings.
3. At High, the new instance receives its own instance credential. The session runs the start self-check (§2), and at High its grants are re-issued to it.

## 5. Checkpoints and vehicles

### Merge vehicles and checkpoints

Every merge vehicle closes a checkpoint. The vehicle's pre-PR gate carries the checkpoint duties:

- the leftover scan; from Standard, the full leftover sweep;
- the list of changed protected paths in the PR body;
- from Standard, the acceptance-criteria trace for the criteria the vehicle's slices claim. At Lite, those criteria are checked in the final-diff review;
- at High L7, the list of the vehicle's high-risk slices in the PR body, so that the human reading of each one ([control-reference.md](control-reference.md), L7) is done and recorded before the vehicle merges.

From Standard, a separate checkpoint review runs for long build stretches without a vehicle; the plan sets that trigger as a number of merged slices since the last vehicle. It also runs at every plan-segment boundary, before the next segment's work starts; at Lite, a final-diff review of the segment takes its place. A criterion never checked at a checkpoint becomes a defect that users find.

At each vehicle or checkpoint, the orchestrator also runs the in-flight incident review ([method.md](method.md), §11) and the documentation fold (§6). The incident review puts each exception past its review date on the owner's queue ([method.md](method.md), §12). Integration, PR and CI cadence follow the merge vehicle, not the slice.

### Vehicle hand-off

1. The pre-PR gate runs in the agent environment and records its result against the vehicle commit.
2. The owner fetches that commit into the owner's checkout and pushes the vehicle branch to the host.
3. The agent PR identity opens the PR from that branch ([control-reference.md](control-reference.md), L5). Where only the owner's account exists, the owner opens it.
4. The owner merges on the host's conclusion for that commit, with the changed protected paths taken from the host's changed-file list (control reference, L5).

### Release vehicles

Release mode's entry, exit and freeze are set in [method.md](method.md) (§4). The checklist at every profile, recorded in the run sheet:

1. Record the owner's release decision in the ledger, with the vehicle's contents.
2. Confirm that CI is green for the release commit, read from the host by commit.
3. Build the artefacts in CI or in an agent environment, and record their digests. Publish by digest, from a step that runs no repository code ([control-reference.md](control-reference.md), L5).
4. The owner tags.
5. Record the exit in the ledger: published, or rolled back.

Standard adds the user-path end-to-end gate on the published artefact ([method.md](method.md), §9). High replaces steps 3 and 4 with the pinned, rehearsed kit run with its gates ([control-reference.md](control-reference.md), L7), and adds the two-party rule (L4).

The first release vehicle starts sustain mode (§8).

## 6. Documentation fold

Working documents are not durable documentation. At each checkpoint and each vehicle, they are folded into the durable documents: architecture documentation and decision records, user documentation, and project and agent instruction files. A technical-writer specialist performs the fold. The leftover scan (from Standard, the sweep) then checks that no reference to a working document leaked into code or durable documents: no paths, no slice IDs, no decision IDs. Working drafts never go into product or public documentation. Agent instruction files are protected paths ([control-reference.md](control-reference.md), L5), so the fold's changes to them reach the owner as the diff.

*Serves C, S. Cost: one fold per checkpoint and vehicle. Profile: all.*

## 7. Session continuity

These procedures apply principle 7 ([method.md](method.md), §1): at a restart, anything not in the ledger, the repository or the decision register is lost.

### Planned compaction

Context reduction happens at a safe point: after a merge or a checkpoint, before execution starts. The session first writes a brief for its compacted self, and afterwards re-reads its limits from the ledger. Compaction in the middle of an execution step loses exactly the detail that step needs.

### Recovery protocol

A restart is a recovery. The session rebuilds its picture from the ledger, re-verifies status from output, not memory, re-establishes its identity and grants from their sources, and runs the re-check (§2). A grant remembered from before the restart is not a grant.

The orchestrator rebuilds from the ledger alone:

1. Check board freshness. If *last updated* is older than the newest log entry, rebuild the board from the log first. This check is mechanised at session start.
2. Read the restated limits, then the board.
3. Read the current log segment.
4. Check each active grant against its source and expiry.
5. Confirm worker status from each worker's branch head and latest hand-over note. Ask only about uncommitted work.

Workers do the same with their slice plan, slice ledger and hand-over notes.

*Serves C, E. Cost: a freshness check at session start. Profile: all.*

### Periodic re-injection

Limits can drop out of a session's context between restarts and compactions. So at intervals set at kickoff, the restated limits are re-injected from the ledger into the session's context. There is no report and no comparison. An incident is recorded only when a session acts against a limit. Re-injection is mechanised where the tool allows.

*Serves S, C. Cost: context per interval. Profile: all.*

### Pause protocol

Usage limits, the owner's absence and overnight periods use one protocol:

1. Before the owner leaves, the orchestrator lists the grants and environment changes the queued slices will need, and the owner applies the environment changes first. At High, the owner issues signed grants with an expiry covering the pause. At Standard, a grant cannot be issued ahead, so slices that need grantable actions run while the owner is present, or are re-planned.
2. Stop at a safe point.
3. Commit work in progress.
4. Schedule the resume.
5. Continue only on standing instructions recorded in the ledger.

Owner chores during and around pauses follow the owner-attention budget ([method.md](method.md), §10). Tool-specific practice belongs in the operator's handbook ([claude-code-reference.md](claude-code-reference.md)).

## 8. Sustain mode

Sustain mode ([method.md](method.md), §4) runs in a named sustain session with its own ledger:

- **Recurring dependency sweep**, carried by one consolidated merge vehicle per run.
- **Emergency vulnerability path**, outside the cadence, with a narrow scope and the same gates. It may preempt a release freeze.
- **Advisory triage.** Each advisory gets exactly one verdict with its reason: already fixed, override, upgrade, code change, quarantine, or false positive.
- **Ledgers for decisions that outlive a run:** overrides (forced or pinned versions, with reason and review date), dismissals (advisories judged not applicable, with reason and re-check date), and abandonments (sweeps wound down without a merge, with reason).
- **Rollback with symmetric reopen.** If a sweep's merge is reverted, everything the sweep closed is reopened: update PRs, advisories, dismissals. The bookkeeping then matches the code again.
- **Mandatory post-merge finalisation.** After the vehicle merges, a finalisation step records the merge, closes the items, archives the run state and runs the incident review ([method.md](method.md), §11). It is not automatic, and the run is not finished until it runs.

Three invariants hold:

1. **The authoritative repository decides.** Update PRs opened by bots are reconciled into the sweep and closed, never merged directly.
2. **No forward-fix on red CI.** The sweep stops. The failure is analysed before any further change.
3. **Every remote mutation leaves a reviewable ledger entry**: closing a PR, dismissing an alert, pushing a branch and every similar action.

Finalisation records go to the sustain ledger's ref, so recording a merge never needs another merge.

*Serves S, C. Cost: one sweep per cadence period, plus finalisation. Profile: all profiles that maintain a released product.*

## 9. The incident record

Each incident ([method.md](method.md), §11) is recorded in the log with what happened, the impact, the resolution and the root-cause class:

| Class | Meaning |
|---|---|
| Process gap | The method had no step for this |
| Tooling | A tool behaved unexpectedly or lacked a check |
| Permission model | Authority or enforcement did not match intent |
| Environment | Infrastructure, network, disk or runtime failed or differed |
| Communication | A message, brief or state file was ambiguous or wrong |
| Review miss | A review stage should have caught it |
| Estimate | A cost or time assumption was wrong |

Optionally, a record also carries the category of a published multi-agent failure taxonomy (system design, inter-agent misalignment, task verification), which makes incidents comparable with published failure data ([related-work.md](related-work.md)).

### Suspected compromise

When a session may be compromised or misled:

1. Halt the agent sessions, and stop scheduled resumes.
2. Revoke the affected credentials and grants; at High, publish signed revocations.
3. Freeze integration: no merge, tag or publish until the review below is done.
4. Diff what landed since the last known-good vehicle, using the ledger, the host's merge history and the message audit, and decide what to revert. Check what sits on the internal remote too: open branches, integration branches and the ledger ref. For every release vehicle since then, yank, re-release or notify users, through the emergency vehicle and the adopter's incident process. Review the lab and shared resources the affected sessions could reach, from records held outside the suspect environments, such as the lab provider's logs. Run no revert command and restore no backup that a suspect session wrote.
5. Recreate every environment in the affected trust domain from its definition, with new credentials, and run a full kickoff before any session resumes. Keep the old environments, or a snapshot, until step 4 is done.

The event is then an incident with a root-cause class like any other.
