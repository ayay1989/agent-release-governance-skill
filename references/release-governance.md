# Agent Release Governance Reference

## Full/User Boundary

Use this split by default:

| Area | Full/private repo | User-facing repo |
| --- | --- | --- |
| Runtime workflow scripts | Yes | Yes, only needed scripts |
| Frontend workbench | Yes | Yes, if part of user runtime |
| Config examples | Yes | Yes, sanitized |
| User onboarding | Yes | Yes |
| Governance plans | Yes | Usually no |
| Eval/regression implementation | Yes | Only if needed by user package checks |
| Privacy scan tooling | Yes | Optional; avoid confusing VM users |
| Demo-only assets | Yes | No |
| Internal handover | Yes | No |
| Secrets/local config | No commit | No commit |

If unsure, keep the file in full and add it to the user manifest only after proving it is needed by VM runtime.

## Release Gate

Before publishing user-facing changes, run the project-specific equivalent of:

1. `git status --short --branch` in full and user repos.
2. Privacy scan over tracked files.
3. Business regression tests.
4. Issue regression / QA tests.
5. Frontend build or typecheck if a frontend exists.
6. User export script, preferably manifest-driven.
7. User package consistency check.
8. Final `git diff --check`.

Do not treat a passing build as production validation. It only proves the package compiles.

## Five QA Chains

For frontend/backend Agent changes, cover these chains:

1. **E2E Critical Path**: the user action reaches the intended workflow step and checkpoint.
2. **Service Call Chain**: frontend action maps to the correct script/tool/API and parameters.
3. **Data Consistency Chain**: writebacks use the right fields, types, states, and record IDs.
4. **Integration Point Chain**: Lark/GitHub/mail/storage/LLM boundaries fail safely.
5. **UI Interaction Chain**: buttons, batch actions, mode switches, and checkpoint controls reflect true backend state.

If only some chains are covered, say so explicitly.

## Issue Triage Rules

Classify open issues as:

- `close`: concrete bug fixed, commit pushed, evidence posted.
- `needs-verification`: code fix exists but user/runtime validation is still needed.
- `keep-open`: governance epic, structural refactor, or long-term capability.
- `full-only`: belongs to private maintainer repo, not user-facing repo.
- `duplicate`: merged into a broader tracked issue.

Before closing an issue, comment with:

- commit hash or branch
- root cause
- fix summary
- regression evidence
- any remaining follow-up moved elsewhere

## GitHub Account Routing

When projects use separate GitHub owners, always check active account before push:

- full/private repo: maintainer personal or private-org account
- user-facing repo: public/team account

After pushing full, switch back to the user-facing account if the user usually works from that identity.

## Installed Skill Sync

When the user has an installed skill copy:

1. Do not overwrite local secrets or config files.
2. Sync only released runtime files.
3. If files are locked read-only, use the project lock/unlock script if present.
4. Lock core files after sync; keep only user config writable.
5. Explain that lock is a guardrail, not a security boundary.

## Privacy Checks

Search for:

- API keys, GitHub tokens, SMTP passwords, LLM keys
- personal emails, phone numbers, ID numbers, bank accounts
- raw resumes or contract text
- real Lark base URLs if the repo is public and the user expects full desensitization
- private filenames that reveal candidate names or internal customers

If a repo intentionally keeps internal-only Lark IDs, state that the risk is access-bound but still not public-clean.

## Release Note Shape

Use user-facing categories:

- Bug fixes
- Workflow improvements
- New capabilities
- Known limitations

Avoid exposing maintainer-only governance details in VM/user README unless they help users operate the Agent.

## Safe Final Checklist

End release work with:

- full branch/commit pushed
- user branch/commit pushed
- open issues remaining and why
- tests/builds run
- frontend/server status if applicable
- account switched back to expected default
