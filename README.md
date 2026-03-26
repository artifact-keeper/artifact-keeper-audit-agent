# ak-audit-agent

A Claude Code plugin that runs automated multi-lens audits on your codebase using agent teams.

When invoked, the plugin creates a team of 8 specialist reviewers that work in parallel, each examining your code through a different lens. The orchestrator coordinates their work, deduplicates findings, cross-references against your existing issue tracker, and produces a consolidated report.

## Audit Lenses

| Lens | What It Checks |
|------|---------------|
| Security | OWASP Top 10, auth flows, crypto, secrets, input validation, supply chain |
| Code Quality | Dead code, duplication, test coverage, error handling, complexity |
| Infrastructure | Helm charts, Terraform, ArgoCD, K8s resources, storage, networking |
| DevOps | CI/CD pipelines, Docker builds, GitHub Actions, release automation |
| Compliance | License compatibility, data handling, privacy, regulatory alignment |
| Accessibility | WCAG 2.2 AA on web frontends, keyboard nav, screen reader, ARIA |
| Performance | Database queries, API design, caching, bundle size, resource usage |
| Documentation | API docs accuracy, README freshness, docs site consistency |

## Requirements

- Claude Code v2.1.32 or later
- Agent teams enabled: set `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS` to `1` in your settings.json

## Installation

### From the Artifact Keeper marketplace

```
/plugin marketplace add artifact-keeper/claude-marketplace
/plugin install ak-audit-agent@ak-marketplace
```

### Local development

```bash
claude --plugin-dir ./artifact-keeper-audit-agent
```

## Usage

### Via the skill

```
/ak-audit-agent:audit                                    # all repos, all lenses
/ak-audit-agent:audit artifact-keeper artifact-keeper-web # specific repos
/ak-audit-agent:audit artifact-keeper --lens security,performance  # specific lenses
```

### Via natural language

When the plugin is enabled, the orchestrator is the default agent:

```
Audit artifact-keeper and artifact-keeper-web
Run a security-only audit on the backend
Full audit, all repos
```

## Output

The audit produces two things:

1. **Consolidated report** at `docs/audits/audit-report-YYYY-MM-DD.md` with findings grouped by severity and lens, an executive summary, and a cross-reference against existing issues.

2. **Issue tracker entries** for net-new findings. If beads is configured, issues are created automatically. Otherwise, findings are listed in the report for manual triage.

## How It Works

1. You invoke the audit with a scope (repos and lenses).
2. The orchestrator creates an agent team and spawns one reviewer per lens.
3. Each reviewer independently scans the target repos through their specialist lens.
4. Reviewers message each other about cross-cutting concerns.
5. The orchestrator collects all findings, deduplicates, and cross-references against existing issues.
6. A consolidated report is written and issue tracker entries are created for new findings.
7. The team is cleaned up.

## Customization

The reviewer agents are in the `agents/` directory. You can modify their audit checklists, severity guidelines, or scope definitions to match your project's needs.

## License

MIT
