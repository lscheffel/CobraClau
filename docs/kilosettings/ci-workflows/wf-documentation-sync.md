---
id: wf-documentation-sync
title: Workflow — Documentation Sync
type: workflow
version: 1.0.0
status: active
trigger: "Continuous / CI / pre-merge / scheduled (daily)"
participants: [documentation-reconciliation]
related_skills: [documentation-reconciliation]
related_bp: [ADR-0142.1-BP]
source: docs/dsg/volume-7/workflow-cookbook.md#workflow-8-documentation-sync
---

# Workflow: documentation-sync

## Goal
Manter docs sincronizados com código automaticamente.

## Preconditions
- [ ] `documentation-reconciliation` skill disponível

## Inputs
- Tree de código + docs

## Sequence
1. **Reconciliation** (`documentation-reconciliation`) — scan (README, CHANGELOG, ADRs, API docs); comparar código vs docs; reportar drift severity (critical/warn/info); auto-fix trivial (links, formatting).
2. **If critical drift** — bloquear merge (CI fail); atribuir a Docs Specialist; exigir fix antes do merge.
3. **Metrics** — drift rate (por semana); time to fix; coverage (% arquivos com docs).

> Ver também `ci-doc-reconciliation.md` (definição do gate de CI).

## Validation Gates
| Gate | Check |
|------|-------|
| Drift scan | Sem critical drift (ou bloqueio de merge) |
| Auto-fix | Trivial aplicado; resto reportado |

## Rollback
- N/A (somente leitura + auto-fix trivial). Reverter auto-fix se quebrar formatação via `git revert`.

## Exit Criteria
- [ ] Scan executado
- [ ] Critical drift tratado (fix ou bloqueio)
- [ ] Métricas registradas

## Metrics
- Drift rate semanal
- Time to fix
- Doc coverage %

## Source Reference
DSG Vol 7, Workflow 8 — docs/dsg/volume-7/workflow-cookbook.md
