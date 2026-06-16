---
name: agent-release-governance
description: Agent full/user dual-repository release governance for AI Agent products. Use when managing a private full repo plus user-facing repo, exporting user packages from a manifest, running privacy scans, QA/regression gates, issue triage, release notes, installed-skill locking, or checking whether frontend/backend Agent workflow changes are safe to publish.
---

# Agent Release Governance

## Purpose

Use this skill to ship an Agent product safely from a full/private source repo into a smaller user-facing repo or installable skill package.

This skill is for release governance, not the Agent's business workflow. Keep product logic in the Agent repo; use this skill to protect boundaries, verify changes, and manage release evidence.

## Core Rules

1. Treat the full/private repo as the source of truth.
2. Generate the user-facing repo from an explicit manifest or export script. Do not manually copy files unless the user explicitly asks for an emergency patch.
3. Keep governance, eval, regression reports, private handover notes, demo-only assets, and maintainer tooling in full unless they are needed by VM/user runtime.
4. Keep user-facing repos focused on runnable workflow entrypoints, minimal setup guidance, config examples, and safe UI/runtime files.
5. Before closing issues or publishing, connect each claim to a commit, test, or code path.
6. Do not mutate git state, remotes, auth account, installed skill files, or release branches unless the user explicitly asks in the current turn.

## Default Workflow

1. **Resolve repos**: identify full repo, user repo, current branch, active GitHub account, and remotes.
2. **Classify the task**:
   - release/export
   - issue triage
   - privacy/audit
   - QA/regression
   - installed-skill sync/lock
   - full/user boundary cleanup
3. **Read local release contract**: locate `user-release-manifest.yaml`, `scripts/release_user.py`, privacy scan scripts, QA tests, frontend build scripts, and lock scripts if present.
4. **Run the smallest safe verification** before changes. Prefer read-only inspection unless the user asked to mutate.
5. **Apply changes in full first**, then export user version through the release script or manifest.
6. **Verify user output** with privacy scan, tests, frontend build, and manifest consistency checks.
7. **Push with correct account routing** only after successful checks.
8. **Triage issues**: close only concrete fixed issues; keep governance epics open until structural work is complete.

## Read When Needed

- For detailed checklists, issue labels, release gates, and repo split rules, read `references/release-governance.md`.

## Output Style

Report in four buckets:

- `已完成`: changes and evidence
- `仍需验证`: user-facing runtime checks not yet performed
- `风险阻塞`: auth, permissions, failing tests, privacy hits, stale branches
- `下一步`: concrete commands or actions, in order
