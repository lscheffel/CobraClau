---
id: ci-doc-reconciliation
title: CI — Documentation Reconciliation
type: ci-flow
version: 1.0.0
status: active
trigger: "CI pipeline / pre-merge / scheduled (daily)"
participants: [documentation-reconciliation]
related_skills: [documentation-reconciliation]
related_bp: [ADR-0142.1-BP]
source: docs/dsg/volume-7/workflow-cookbook.md#workflow-8-documentation-sync
---

# CI: Documentation Reconciliation

## Goal
Detectar e tratar deriva entre código e documentação; bloquear merge em drift crítico.

## Preconditions
- [ ] `documentation-reconciliation` skill operacional

## Inputs
- Tree de código + docs (README, CHANGELOG, ADRs, API docs)

## Sequence
1. **Scan** — varrer arquivos de doc e código.
2. **Compare** — código vs docs; classificar drift severity:
   - `critical` — API/contrato mudou, doc desatualizado (quebra consumidor)
   - `warn` — descrição imprecisa, exemplo quebrado
   - `info` — formatação, link morto
3. **Auto-fix** — trivial (links, formatação) corrigido automaticamente.
4. **If critical** — bloquear merge (CI fail); atribuir a Docs Specialist; exigir fix antes do merge.

## Validation Gates
| Gate | Enforcement | Pass Criteria |
|------|-------------|---------------|
| Drift scan | pipeline | 0 critical |
| Auto-fix | pipeline | trivial aplicado; resto reportado |

## Rollback / Fail Policy
- **Fail policy: fail-closed para `critical`, fail-open para `warn`/`info`.** Critical ⇒ merge bloqueado. Warn/info ⇒ reportado, não bloqueia.
- Auto-fix trivial é seguro (apenas links/formatação); reverter via `git revert` se necessário.

## Exit Criteria
- [ ] Scan executado sem critical drift
- [ ] Métricas de drift registradas

## Metrics
- Drift rate (por semana)
- Time to fix
- Doc coverage %

## Source Reference
DSG Vol 7, Workflow 8 (documentation-sync) — docs/dsg/volume-7/workflow-cookbook.md
