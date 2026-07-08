---
id: wf-agent-onboarding
title: Workflow — Agent Onboarding
type: workflow
version: 1.0.0
status: active
trigger: "Novo agente (humano ou IA) entra na stack"
participants: []
related_skills: [writing-plans]
related_bp: [ADR-0142.1-BP]
source: docs/dsg/volume-7/workflow-cookbook.md#workflow-5-agent-onboarding
---

# Workflow: agent-onboarding

## Goal
Novo agente (humano ou IA) produtivo em < 30 min.

## Preconditions
- [ ] Acesso ao repositório / DSG

## Inputs
- Nenhum (auto-guiado)

## Sequence
1. Ler **Volume 0** (Executive Overview) — 5 min.
2. Ler **Volume 14** (Quick Reference) — 5 min.
3. Ler **Volume 1** (Mental Model) — 10 min.
4. Praticar: carregar skill → executar task simples — 10 min (`skill find-skills`, `skill writing-plans`, criar mini plan).
5. Ler **Volume 3/4** (Handbook do seu papel) — conforme necessário.

> Ver também `docs/dsg/AGENTS.md` (protocolo de inicialização obrigatório para agentes IA).

## Validation Gates
| Gate | Check |
|------|-------|
| Compreensão | Consegue invocar skill corretamente |
| Modelo mental | Entende agent/skill/MCP/command/ADR |
| Execução | Executa tarefa simples end-to-end |

## Rollback
- N/A (onboarding não muta estado). Reiniciar do Volume 0 se contexto corrompido.

## Exit Criteria
- [ ] Invoca skill corretamente
- [ ] Entende os 5 conceitos primários
- [ ] Executa tarefa simples ponta a ponta

## Metrics
- Tempo até primeira task produtiva
- Taxa de erro de invocação pós-onboarding

## Source Reference
DSG Vol 7, Workflow 5 — docs/dsg/volume-7/workflow-cookbook.md
