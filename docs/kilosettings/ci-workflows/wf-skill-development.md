---
id: wf-skill-development
title: Workflow — Skill Development
type: workflow
version: 1.0.0
status: active
trigger: "Necessidade de nova skill na stack"
participants: [skill-creator, writing-skills]
related_skills: [skill-creator, writing-skills]
related_bp: [ADR-0142.1-BP]
source: docs/dsg/volume-7/workflow-cookbook.md#workflow-6-skill-development
---

# Workflow: skill-development

## Goal
Criar nova skill seguindo padrões da stack.

## Preconditions
- [ ] Gap de capability identificado

## Inputs
- Problema a ser resolvido pela skill

## Sequence
1. **Discovery** (`skill-creator`) — problema resolvido? granularidade ideal? reutilização existente?
2. **Draft SKILL.md** — frontmatter (name, description, version, author, maturity); Mission/Scope/Non-Scope; Inputs/Outputs; Internal Reasoning Pattern; Success Criteria/Failure Modes/Recovery; Typical Consumers/Synergies; Anti-Patterns; Cost Profile; Example Sessions.
3. **Templates/Scripts** (se aplicável) — `templates/` na skill dir; `checklists/` para validação.
4. **Test** — carregar skill em nova sessão; executar example sessions; verificar success criteria.
5. **Register** — adicionar ao registry (se global); atualizar `related_skills` em skills sinérgicas; documentar no Volume 3 (Skills Handbook).

## Validation Gates
| Gate | Check |
|------|-------|
| Draft | SKILL.md cobre todas as seções obrigatórias |
| Test | Example sessions passam em sessão limpa |
| Register | Presente no registry + Vol 3 atualizado |

## Rollback
- Remover skill do registry e do Volume 3 se falhar validação; manter rascunho para iteração.

## Exit Criteria
- [ ] SKILL.md completo e testado
- [ ] Registrada no registry global
- [ ] Documentada no Volume 3

## Metrics
- Tempo de criação
- Taxa de reuso da nova skill

## Source Reference
DSG Vol 7, Workflow 6 — docs/dsg/volume-7/workflow-cookbook.md
