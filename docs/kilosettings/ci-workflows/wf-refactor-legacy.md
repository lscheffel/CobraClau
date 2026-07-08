---
id: wf-refactor-legacy
title: Workflow — Refactor Legacy
type: workflow
version: 1.0.0
status: active
trigger: "Tech debt identificado em código legado"
participants: [reverse-engineering-specs, refactoring, test-driven-development]
related_skills: [reverse-engineering-specs, refactoring, test-driven-development]
related_bp: [ADR-0142.1-BP]
source: docs/dsg/volume-7/workflow-cookbook.md#workflow-3-refactor-legacy
---

# Workflow: refactor-legacy

## Goal
Refatorar código legacy com segurança: specs → characterization tests → refactor incremental.

## Preconditions
- [ ] Código legacy identificado
- [ ] Testes atuais passam (baseline)

## Inputs
- Alvo de refatoração (módulo/arquivo)

## Sequence
1. **Reverse Engineering** (`reverse-engineering-specs`) — rastrear code paths exaustivamente; specs comportamentais (implementation-free); edge cases documentados.
2. **Characterization Tests** (`test-driven-development`) — capturar comportamento atual como testes; cobrir happy path/edges/errors; baseline = todos passam.
3. **Strangler Fig / Branch by Abstraction** (`refactoring`) — criar camada de abstração; migração incremental; testes passam a cada passo; código antigo deletado quando coberto.
4. **Validation** — characterization tests passam; novos testes passam; benchmarks de performance; docs atualizadas.

## Validation Gates
| Gate | Checks | Pass Criteria |
|------|--------|---------------|
| Baseline | Testes atuais | Todos verdes antes de começar |
| Per-step | Characterization + novos | Sem regressão |
| Performance | Benchmarks | Dentro da margem |

## Rollback
```bash
# Camada de abstração permite rollback instantâneo:
git revert <migration-commits>   # código antigo ainda funciona atrás da abstração
```

## Exit Criteria
- [ ] Specs comportamentais documentadas
- [ ] Characterization tests verdes
- [ ] Código novo coberto e antigo removido
- [ ] Performance mantida

## Metrics
- Cobertura de teste ganha
- Regressões introduzidas
- Tempo de migração

## Source Reference
DSG Vol 7, Workflow 3 — docs/dsg/volume-7/workflow-cookbook.md
