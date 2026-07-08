---
id: wf-new-feature
title: Workflow — New Feature
type: workflow
version: 1.0.0
status: active
trigger: "Idea / Request de nova funcionalidade"
participants: [spec-writing, adr-generator, writing-plans, implementation, code-review, security-review, release, documentation]
related_skills: [spec-writing, adr-generator, writing-plans, implementation, executing-plans, code-review, security-review, documentation-reconciliation, release]
related_bp: [ADR-0142.1-BP]
source: docs/dsg/volume-7/workflow-cookbook.md#workflow-2-new-feature
---

# Workflow: new-feature

## Goal
Da ideia à implementação validada: spec → plan → execute → review → release.

## Preconditions
- [ ] Problema/objetivo claro
- [ ] Stakeholder disponível para decisões

## Inputs
- Descrição da ideia/request

## Sequence
1. **Spec Writing** (`spec-writing`) — framework JTBD, acceptance criteria, SLC release planning.
2. **ADR Creation** (`adr-generator`) — se impacto arquitetural: Context/Decision/Consequences; status Proposed→Accepted; Blueprint + TODO auto-gerados.
3. **Planning** (`writing-plans`) — decomposição em fases, task cards com arquivos/critérios/deps, estimates de complexidade.
4. **Execution** (`implementation` ou `executing-plans`) — por `wf-implement-adr`; validation gates por task.
5. **Review & Merge** — `code-review` (pre-merge), `security-review` (se aplicável), `documentation-reconciliation` (CI).
6. **Release** (`release`) — changelog, version bump, tag + deploy, rollback plan documentado.

## Validation Gates
| Gate | Checks | Pass Criteria |
|------|--------|---------------|
| Spec review | Stakeholder alinhado | Iterar spec se não; não prosseguir |
| Plan approval | Complexidade alta | Human approval gate |
| Pre-merge | code-review + security + doc-reconciliation | Todos passam |

## Rollback
- Reverter merge via `git revert` ou rollback de deploy documentado na etapa 6.
- ADR pode ser reaberta para revisão se premissa mudar.

## Exit Criteria
- [ ] Spec aprovada
- [ ] ADR (se aplicável) Accepted
- [ ] Plano executado com gates verdes
- [ ] Review + reconciliation passam
- [ ] Release publicado com rollback plan

## Metrics
- Tempo idea→release
- Taxa de retrabalho pós-review
- Débito de doc aberto

## Source Reference
DSG Vol 7, Workflow 2 — docs/dsg/volume-7/workflow-cookbook.md
