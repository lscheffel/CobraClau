---
id: wf-incident-response
title: Workflow — Incident Response
type: workflow
version: 1.0.0
status: active
trigger: "Alert firing ou report de incidente em produção"
participants: [systematic-debugging]
related_skills: [systematic-debugging]
related_bp: [ADR-0142.1-BP]
source: docs/dsg/volume-7/workflow-cookbook.md#workflow-4-incident-response
---

# Workflow: incident-response

## Goal
Responder a incidente em produção: detect → diagnose → mitigate → resolve → postmortem.

## Preconditions
- [ ] Alert firing ou report recebido
- [ ] On-call disponível

## Inputs
- Alert / descrição do sintoma

## Sequence
1. **Detect & Triage** — confirmar incidente (não false positive); severidade SEV-1/2/3; abrir incident channel/log; designar incident commander.
2. **Diagnose** (`systematic-debugging`) — Fase 1 reproduzir/evidência; Fase 2 hipótese; Fase 3 testar hipótese (check mínimo); Fase 4 confirmar root cause; documentar.
3. **Mitigate** (imediato) — aplicar workaround/rollback/scale; verificar mitigação; comunicar status.
4. **Resolve** (fix permanente) — root cause fix; testar em staging; deploy com monitoramento; verificar resolução.
5. **Postmortem** (blameless) — timeline; root cause (5 whys); impacto; action items (prevenir recorrência); compartilhar aprendizados.

## Validation Gates
| Gate | Check |
|------|-------|
| Mitigation | Serviço restaurado ao SLA |
| Fix | Testes passam + sem regressão |
| Postmortem | Action items atribuídos e rastreados |

## Rollback
- Rollback de deploy ou escala de volta conforme mitigação; reverter fix se introduzir regressão.

## Exit Criteria
- [ ] Serviço restaurado (SLA)
- [ ] Root cause confirmado e fixado
- [ ] Postmortem blameless completo com action items

## Metrics
- MTTR
- SEV distribution
- Nº de recorrências da mesma causa

## Source Reference
DSG Vol 7, Workflow 4 — docs/dsg/volume-7/workflow-cookbook.md
