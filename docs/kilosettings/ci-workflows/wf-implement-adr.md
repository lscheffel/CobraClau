---
id: wf-implement-adr
title: Workflow — Implement ADR
type: workflow
version: 1.0.0
status: active
trigger: "ADR status = Accepted"
participants: [implementation, writing-plans, code, testing, code-review, documentation]
related_skills: [implementation, writing-plans, testing, code-review, documentation]
related_bp: [ADR-0142.1-BP]
source: docs/dsg/volume-7/workflow-cookbook.md#workflow-1-implement-adr
---

# Workflow: implement-adr

## Goal
Implementar uma ADR aprovada via Execution Contract + DAG de tarefas + validação contínua.

## Preconditions
- [ ] ADR existe em `docs/adr/ADR-XXX.md` com status `Accepted`
- [ ] Blueprint existe em `docs/adr/ADR-XXX-BP.md`
- [ ] TODO existe em `docs/adr/ADR-XXX-TODO.md` com tasks e dependências
- [ ] Branch limpa (não main/master sem PR)
- [ ] Workspace limpo (sem uncommitted changes)

## Inputs
- ADR ID (ex: `ADR-0142`)

## Sequence
1. **Artifact Resolution** — carregar ADR, Blueprint, TODO; extrair `related_skills`; mapear arquivos impactados. *Checkpoint: artifacts existem e são coerentes.*
2. **Execution Contract** — validar status=Accepted, Blueprint tem tasks, TODO tem tasks com estados, branch/workspace limpos; extrair acceptance + rollback criteria. *Checkpoint: contract assinado.*
3. **Dependency Analysis** — construir DAG das dependências do TODO; detectar ciclos (falha se houver); topological sort; identificar paralelizáveis; estimar tempo. *Checkpoint: DAG válido.*
4. **Incremental Execution** (por task na ordem do DAG):
   - verificar dependências = Concluído
   - marcar = Em andamento; gerar `task-progress.md`
   - executar mudanças (código + invocações de skill)
   - **Validação contínua:** Build → Lint → Typecheck → Unit → Integration → Architectural (vs ADR) → Documentation
   - PASS → atualizar docs, marcar Concluído; FAIL (máx 3 retries) → analisar causa, fixar, revalidar; se persistir → Bloqueado
5. **Documentation Sync** — ADR/Blueprint/README/`related_skills` em dia. *Checkpoint: sem doc drift.*
6. **Execution Report** — gerar `execution-report.md` (summary, tasks, validations, risks, debt, recommendations).

## Validation Gates
| Gate | Checks | Pass Criteria |
|------|--------|---------------|
| Pre-execution | Campos do contract | Todos válidos |
| Per-task | Build+Lint+Typecheck+Tests | Todos pass |
| Architectural | Código bate com decisão da ADR | Sem drift |
| Documentation | Docs sincronizam com código | `documentation-reconciliation` passa |

## Rollback
```bash
# Task falha irrecuperavelmente:
git reset --hard HEAD~1                 # reverte último commit da task
git reset --hard <commit-before-first-task>  # múltiplas tasks
# Sempre gerar rollback-report.md: motivo, tasks revertidas, ações corretivas
```

## Exit Criteria
- [ ] Todas as tasks = Concluído
- [ ] Todos os validation gates passam
- [ ] Documentação sincronizada
- [ ] Execution report gerado
- [ ] Branch pronta para PR/merge

## Metrics
- Tasks completadas / total
- Taxa de passo de validação
- Tempo por task vs estimado
- Contagem de rollbacks

## Source Reference
DSG Vol 7, Workflow 1 — docs/dsg/volume-7/workflow-cookbook.md
