# INDEX — CI Flows & Workflow Utilities

> **Ponteiro mestre** dos arquivos utilitários extraídos do DSG (ADR-0142.1, Implementado).
> Fonte canônica de cada fluxo = arquivo individual abaixo. DSG Vol 7 = narrativa ponteiro.
> Contrato de arquivo: [ADR-0142.1-BP.md](../adr/archive/ADR-0142.1-BP.md) · Tracking: [ADR-0142.1-TODO.md](../adr/archive/ADR-0142.1-TODO.md) · ADR pai: [ADR-0142 (DSG, Implementado)](../adr/archive/ADR-0142.md)

---

## Workflows (type: workflow)

| ID | Arquivo | Trigger | Participants | Status |
|----|---------|---------|--------------|--------|
| `implement-adr` | [wf-implement-adr.md](./wf-implement-adr.md) | ADR status = Accepted | implementation, writing-plans, code, testing, code-review, documentation | ✅ active |
| `new-feature` | [wf-new-feature.md](./wf-new-feature.md) | Idea / Request | spec-writing, adr-generator, implementation, code-review, release | ✅ active |
| `refactor-legacy` | [wf-refactor-legacy.md](./wf-refactor-legacy.md) | Tech debt identificado | reverse-engineering-specs, refactoring, tdd | ✅ active |
| `incident-response` | [wf-incident-response.md](./wf-incident-response.md) | Alert / report | systematic-debugging | ✅ active |
| `agent-onboarding` | [wf-agent-onboarding.md](./wf-agent-onboarding.md) | Novo agente | — | ✅ active |
| `skill-development` | [wf-skill-development.md](./wf-skill-development.md) | Need new skill | skill-creator, writing-skills | ✅ active |
| `mcp-integration` | [wf-mcp-integration.md](./wf-mcp-integration.md) | New MCP needed | — | ✅ active |
| `documentation-sync` | [wf-documentation-sync.md](./wf-documentation-sync.md) | Continuous / CI / pre-merge | documentation-reconciliation | ✅ active |

## Fluxos CI (type: ci-flow)

| ID | Arquivo | Trigger | Enforcement | Fail Policy | Status |
|----|---------|---------|-------------|-------------|--------|
| `validation-gates` | [ci-validation-gates.md](./ci-validation-gates.md) | Todo commit / PR | local + pipeline | fail-closed | ✅ active |
| `doc-reconciliation` | [ci-doc-reconciliation.md](./ci-doc-reconciliation.md) | CI / pre-merge / scheduled | pipeline | fail-closed (critical) | ✅ active |
| `pre-merge-gate` | [ci-pre-merge-gate.md](./ci-pre-merge-gate.md) | PR aberto | pipeline | fail-closed | ✅ active |

---

## Como consumir

```bash
# Agente precisa de um fluxo:
read docs/kilosettings/ci-workflows/<id>.md

# Subagente recebe como único contrato de contexto:
#  → cole o conteúdo do arquivo no prompt do subagente
#  → ele saberá: ownership, forbidden, escalation, gates, rollback
```

---

## Regra de sincronia

Arquivo utilitário = fonte canônica. DSG Vol 7 = ponteiro. Em dúvida, o utilitário vence. Todo arquivo tem `source` no frontmatter apontando à seção do DSG (checkpoint de deriva).

---

*Gerado por ADR-0142.1 · 2026-07-08.*
