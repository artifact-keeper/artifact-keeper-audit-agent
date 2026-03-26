---
name: audit
description: Run a multi-lens audit on one or more repositories. Spawns an agent team with up to 8 specialist reviewers (security, code quality, infrastructure, DevOps, compliance, accessibility, performance, documentation) and produces a consolidated report with prioritized findings.
---

# Audit Skill

Run a comprehensive audit on the specified repositories using the ak-audit-team orchestrator.

## Usage

```
/ak-audit-agent:audit [repos...] [--lens lens1,lens2,...]
```

**Examples:**
- `/ak-audit-agent:audit` - audit all repos in the workspace, all lenses
- `/ak-audit-agent:audit artifact-keeper artifact-keeper-web` - audit two repos, all lenses
- `/ak-audit-agent:audit artifact-keeper --lens security,performance` - audit one repo, two lenses

## Arguments

`$ARGUMENTS` contains the user's input after the skill name. Parse it as follows:

- Any words that match directory names in the workspace are repo targets.
- If `--lens` is provided, only run those lenses. Valid values: security, code-quality, infrastructure, devops, compliance, accessibility, performance, documentation.
- If no repos are specified, audit all repos found in the workspace root.
- If no lenses are specified, run all 8.

## What Happens

1. The ak-audit-team orchestrator creates an agent team.
2. One specialist reviewer is spawned per active lens.
3. Each reviewer independently audits the target repos through their lens.
4. Reviewers communicate cross-cutting concerns to each other.
5. The orchestrator collects findings, deduplicates, and cross-references against any existing issue tracker.
6. A consolidated report is written to `docs/audits/audit-report-YYYY-MM-DD.md`.
7. Net-new findings are created as issue tracker entries (beads if available).
8. A summary is presented.

## Requirements

- Agent teams must be enabled: `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` in settings.json
- Target repositories must be accessible from the current workspace
