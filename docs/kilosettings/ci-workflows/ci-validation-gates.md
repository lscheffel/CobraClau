---
id: ci-validation-gates
title: CI — Validation Gates
type: ci-flow
version: 1.0.0
status: active
trigger: "Todo commit de task / abertura de PR"
participants: [testing, code-review, architecture-review]
related_skills: [testing, code-review, architecture-review, documentation-reconciliation]
related_bp: [ADR-0142.1-BP]
source: docs/dsg/volume-7/workflow-cookbook.md#validation-gates
---

# CI: Validation Gates

## Goal
Impor 6 gates obrigatórios antes que uma task possa ser marcada **Concluída** ou um PR seja mergeado.

## Preconditions
- [ ] Código com build/lint/typecheck configurados
- [ ] Suíte de testes presente

## Inputs
- Diff da task / PR

## Sequence
1. **Build** — compilação/empacotamento passa.
2. **Lint** — sem violações de estilo.
3. **Typecheck** — sem erros de tipo.
4. **Tests** — unit + integration passam.
5. **Architectural** — código bate com decisão da ADR (sem drift).
6. **Documentation** — docs sincronizam com código (`documentation-reconciliation`).

## Validation Gates
| # | Gate | Enforcement | Pass Criteria |
|---|------|-------------|---------------|
| 1 | Build | local + pipeline | exit 0 |
| 2 | Lint | local + pipeline | 0 warnings |
| 3 | Typecheck | local + pipeline | 0 errors |
| 4 | Tests | local + pipeline | 100% pass |
| 5 | Architectural | pipeline (architecture-review) | sem drift vs ADR |
| 6 | Documentation | pipeline (doc-reconciliation) | reconciled |

## Rollback / Fail Policy
- **Fail policy: fail-closed.** Qualquer gate vermelho ⇒ task NÃO é "Concluída" e PR NÃO mergeia.
- Local: agente revalida antes de marcar concluído.
- Pipeline: bloqueio automático; path de degradação em `docs/dsg/volume-6` por MCP.

## Exit Criteria
- [ ] Os 6 gates verdes localmente
- [ ] Pipeline verde no PR

## Metrics
- Gate pass rate
- Tempo médio de correção de gate vermelho

## Source Reference
DSG Vol 7 (Validation Gates, em `implement-adr`) — docs/dsg/volume-7/workflow-cookbook.md · DSG AGENTS.md (Validação Obrigatória)
