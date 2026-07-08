# Volume 0 — Executive Overview

> **Objetivo:** Compreensão global da stack em menos de 5 minutos.

> **Status de Implementação:** O DSG foi **Implementado** (ADR-0142, arquivado em `docs/adr/archive/ADR-0142.md`). Os 8 workflows e 3 fluxos CI foram extraídos para utilitários individuais em `docs/kilosettings/ci-workflows/` (ADR-0142.1, Implementado) e contam com **pipeline actions reais** em `.github/workflows/` (`ci-validation-gates.yml`, `ci-doc-reconciliation.yml`, `ci-pre-merge-gate.yml`).

---

## Propósito da Stack

A **Kilo Stack** é um ecossistema operacional para desenvolvimento de software assistido por IA, projetado para:

- **Orquestrar agentes especializados** com papéis, permissões e modelos bem definidos
- **Fornecer skills reutilizáveis** como unidades de conhecimento operacional versionável
- **Integrar ferramentas externas** via MCP (Model Context Protocol) de forma segura e auditável
- **Garantir governança** através de ADRs, blueprints, TODOs e validação contínua
- **Eliminar conhecimento tribal** tratando documentação como código

---

## Problemas Resolvidos

| Problema | Solução da Stack |
|---|---|
| Agentes genéricos sem especialização | 5 agentes com domínios, permissões e modelos dedicados |
| Skills descobertas por acaso | 97 skills organizadas, versionadas e carregáveis via URL |
| Integrações externas ad-hoc | 9 MCPs configurados com auth, permissões e degradação graciosa |
| Decisões arquiteturais perdidas | ADR + Blueprint + TODO + Execution Contract |
| Onboarding lento | DSG + Quick Reference + Mental Model |
| Deriva operacional | Governance skill + validação contínua + rollback procedures |

---

## Filosofia Operacional

```
Explicit over Implicit
Governance over Improvisation
Specialization over Generalism
Reproducibility over Creativity
Determinism over Stochastic Exploration
Documentation over Tribal Knowledge
Review over Trust
Context Preservation over Speed
```

---

## Princípios Fundamentais

1. **Tudo é versãoável** — Skills, agents, commands, ADRs, docs vivem em git
2. **Contrato antes de execução** — Execution Contract valida pré-condições
3. **Uma tarefa por vez** — Execution Loop incremental com validação contínua
3. **Rollback como feature** — Todo workflow tem procedimento de reversão
4. **Observabilidade nativa** — Build, lint, test, typecheck a cada alteração
5. **Degradação graciosa** — MCPs falham sem derrubar a sessão

---

## Mapa Conceitual

```
┌─────────────────────────────────────────────────────────────┐
│                      KILO STACK                             │
├─────────────────────────────────────────────────────────────┤
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐   │
│  │  Skills  │  │  Agents  │  │ Commands │  │   MCPs   │   │
│  │   (97)   │  │   (5)    │  │   (N)    │  │   (9)    │   │
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘   │
│         │           │           │           │              │
│         └───────────┼───────────┼───────────┘              │
│                     ▼                                       │
│         ┌───────────────────────┐                           │
│         │   Governance Layer    │                           │
│         │ ADR • Blueprint • TODO│                           │
│         │ Execution Contract    │                           │
│         │ Validation Gates      │                           │
│         └───────────────────────┘                           │
└─────────────────────────────────────────────────────────────┘
```

---

## Componentes Principais

| Componente | Quantidade | Descrição |
|---|---|---|
| **Skills** | 97 | Unidades de conhecimento operacional (automações, patterns, workflows) |
| **Agents** | 5 | Personas especializadas com modelo, permissões e domínio próprios |
| **Commands** | N | Atalhos operacionais em `.kilo/command/*.md` |
| **MCPs** | 9 | Integrações externas (GitHub, Notion, Google Workspace, Filesystem, etc.) |
| **ADRs** | 142+ | Decisões arquiteturais versionadas com status |
| **Workflows** | 8 | Sequências coordenadas de agents + skills + MCPs (utilitários em `docs/kilosettings/ci-workflows/`) |
| **Fluxos CI** | 3 | `validation-gates`, `doc-reconciliation`, `pre-merge-gate` (pipeline actions em `.github/workflows/`) |

---

## Casos de Uso Ideais

- **Desenvolvimento full-cycle** — Da ADR ao deploy com validação automática
- **Refatoração governada** — Strangler Fig, Branch by Abstraction com rollback
- **Onboarding de agentes** — Novo agente produtivo em minutos via skills
- **Auditoria de decisões** — Rastreabilidade completa ADR → código → docs
- **Operação multi-MCP** — Google Workspace + GitHub + Notion + Memory coordenados

---

## Limites da Solução

| Limite | Mitigação |
|---|---|
| Curva de aprendizado inicial | Volume 0 + 14 (Quick Reference) = 5 min |
| Manutenção de docs | Documentation skill + sync automático |
| Obsolescência de skills | Versionamento semântico + skill-audit-bulletin |
| Custo de tokens | Token Economy best practices (Vol 8) |
| Dependência de MCPs externos | Degradação graciosa + cache local |
| Complexidade de debugging | Systematic-debugging skill + troubleshooting (Vol 10) |

---

## Próximos Passos

| Se você é... | Comece por... |
|---|---|
| **Humano novo** | Volume 1 (Mental Model) → Volume 14 (Quick Reference) |
| **Agente recém-instanciado** | Volume 13 (Glossary) → Volume 3/4 (Handbook do seu papel) |
| **Arquiteto** | Volume 2 (Philosophy) → Volume 7 (Workflows) |
| **Operador diário** | Volume 14 (Quick Reference) → Volume 8 (Best Practices) |
| **Em incidente** | Volume 10 (Troubleshooting) → Volume 9 (Anti-patterns) |