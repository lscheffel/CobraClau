---
id: ci-pre-merge-gate
title: CI — Pre-Merge Gate
type: ci-flow
version: 1.0.0
status: active
trigger: "PR aberto / PR atualizado"
participants: [code-review, security-review, documentation-reconciliation]
related_skills: [code-review, security-review, documentation-reconciliation, testing]
related_bp: [ADR-0142.1-BP]
source: docs/dsg/volume-7/workflow-cookbook.md#workflow-2-new-feature
---

# CI: Pre-Merge Gate

## Goal
Garantir que nenhum PR mergeie sem revisão de código, segurança e reconciliação de doc.

## Preconditions
- [ ] PR aberto com diff completo
- [ ] `ci-validation-gates` verde

## Inputs
- PR (diff + metadados)

## Sequence
1. **Code Review** (`code-review`) — padrões, legibilidade, SOLID, smell check; aprovação necessária.
2. **Security Review** (`security-review`, se aplicável) — secrets, injeção, deps inseguras; aprovação necessária.
3. **Doc Reconciliation** (`ci-doc-reconciliation`) — sem critical drift.
4. **Merge** — somente após todos os gates verdes + aprovações.

## Validation Gates
| Gate | Enforcement | Pass Criteria |
|------|-------------|---------------|
| Code review | pipeline + human | aprovado |
| Security review | pipeline + human | aprovado (ou N/A) |
| Doc reconciliation | pipeline | 0 critical |
| Validation gates | pipeline | 6/6 verdes |

## Rollback / Fail Policy
- **Fail policy: fail-closed.** Qualquer gate vermelho ou falta de aprovação ⇒ merge bloqueado.
- Se merge errôneo ocorrer: `git revert` do commit; reabrir correção.

## Exit Criteria
- [ ] Code review aprovado
- [ ] Security review aprovado/N-A
- [ ] Doc reconciliation verde
- [ ] Merge autorizado

## Metrics
- Tempo de aprovação de PR
- % de PRs com retrabalho pós-review

## Source Reference
DSG Vol 7, Workflow 2 (new-feature, etapa 5) — docs/dsg/volume-7/workflow-cookbook.md
