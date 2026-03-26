# Changelog

## 1.0.0 (2026-03-25)

Initial release.

- Orchestrator agent (ak-audit-team) with full team coordination workflow
- 8 specialist reviewer agents: security, code quality, infrastructure, devops, compliance, accessibility, performance, documentation
- `/ak-audit-agent:audit` skill with repo and lens selection
- Configurable scope: choose which repos and lenses to include per run
- Standardized finding format across all lenses
- Consolidated report generation with executive summary
- Cross-reference against existing beads issues for delta detection
- Automatic beads issue creation for net-new findings
