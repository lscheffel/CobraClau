# ADR-0142 — Definitive Stack Guide (DSG)

## Status

Proposed

---

## Context

A stack evoluiu além do limiar onde:

* README é suficiente;
* documentação técnica fragmentada é navegável;
* descoberta emergente produz resultados consistentes;
* novos agentes conseguem atingir proficiência operacional rapidamente.

Atualmente a stack contém:

* ~20 Skills
* ~9 Agents
* ~6 Commands
* ~5 MCP integrations
* Rules
* Governance
* Workflows
* ADRs
* Operational conventions
* Behavioral contracts

A ausência de um guia canônico produz:

* subutilização de capacidades;
* deriva operacional;
* uso inadequado de agentes;
* combinações ineficientes;
* onboarding lento;
* perda de conhecimento tácito.

---

## Decision

Será criado um artefato canônico denominado:

# Definitive Stack Guide (DSG)

Objetivo:

> tornar explícito todo conhecimento operacional necessário para utilização ótima da stack.

O guia NÃO será:

* inventário técnico;
* catálogo de arquivos;
* documentação de implementação;
* registry semântico;
* mecanismo de descoberta.

O guia SERÁ:

* manual operacional;
* handbook;
* referência definitiva;
* material de treinamento humano e agêntico;
* fonte canônica de verdade operacional.

---

# Blueprint

---

# Volume 0 — Executive Overview

Objetivo:

Permitir compreensão global da stack em menos de 5 minutos.

Conteúdo:

* propósito da stack;
* problemas resolvidos;
* filosofia operacional;
* princípios fundamentais;
* mapa conceitual;
* componentes principais;
* casos de uso ideais;
* limites da solução.

---

# Volume 1 — Mental Model

## Capítulo 1

### O que é um agente

Cobertura:

* definição;
* responsabilidades;
* autonomia;
* limites;
* ciclo de vida;
* ownership.

---

## Capítulo 2

### O que é uma skill

Cobertura:

* abstração funcional;
* granularidade ideal;
* escopo;
* reutilização;
* acoplamento aceitável.

---

## Capítulo 3

### O que é um workflow

Cobertura:

* coordenação;
* orquestração;
* sincronização;
* pontos de decisão;
* rollback.

---

## Capítulo 4

### O que é uma regra

Cobertura:

* enforcement;
* precedência;
* conflito;
* exceções.

---

## Capítulo 5

### O que é um MCP

Cobertura:

* papel;
* fronteira;
* segurança;
* autorização;
* degradação graciosa.

---

## Capítulo 6

### O que é um comando

Cobertura:

* ergonomia;
* automação;
* aceleração operacional.

---

# Volume 2 — Philosophy

## Design Principles

Exemplo:

* Explicit over implicit
* Governance over improvisation
* Specialization over generalism
* Reproducibility over creativity
* Determinism over stochastic exploration
* Documentation over tribal knowledge
* Review over trust
* Context preservation over speed

---

## Tradeoffs

Explicar deliberadamente:

* onde a stack perde velocidade;
* onde ganha qualidade;
* onde aumenta custo;
* onde reduz risco.

---

# Volume 3 — Skills Handbook

Para cada skill:

---

## Identity

Nome

Versão

Autor

Status

Maturidade

---

## Mission

Problema resolvido.

---

## Scope

O que faz.

---

## Non-Scope

O que deliberadamente não faz.

---

## Inputs

Entradas esperadas.

---

## Outputs

Saídas produzidas.

---

## Internal Reasoning Pattern

Exemplo:

* decomposition
* critique loop
* refinement
* verification

---

## Success Criteria

Como saber que executou corretamente.

---

## Failure Modes

Como falha.

---

## Recovery Strategies

Como recuperar.

---

## Typical Consumers

Quais agentes normalmente utilizam.

---

## Synergies

Quais skills funcionam bem juntas.

---

## Anti-Patterns

Usos incorretos.

---

## Cost Profile

* baixo
* médio
* alto

---

## Example Sessions

Múltiplos exemplos reais.

---

# Volume 4 — Agent Handbook

Para cada agente:

---

## Persona

---

## Expertise

---

## Decision Authority

---

## Preferred Work Domain

---

## Forbidden Domains

---

## Escalation Conditions

---

## Delegation Strategy

---

## Context Consumption Pattern

---

## Reasoning Depth

---

## Creativity Bias

---

## Risk Tolerance

---

## Hallucination Risk Profile

---

## Typical Tool Usage

---

## Ideal Collaborators

---

## Conflict Patterns

---

## Example Dialogues

---

# Volume 5 — Commands Reference

Para cada comando:

* propósito;
* sintaxe;
* parâmetros;
* exemplos;
* erros frequentes;
* troubleshooting;
* incompatibilidades.

---

# Volume 6 — MCP Handbook

Para cada MCP:

---

## Purpose

---

## Authentication

---

## Permissions

---

## Latency Expectations

---

## Failure Modes

---

## Retry Policies

---

## Caching Recommendations

---

## Security Risks

---

## Operational Costs

---

## Best Practices

---

## Common Misuses

---

# Volume 7 — Workflow Cookbook

Cada workflow deve conter:

---

## Goal

---

## Preconditions

---

## Inputs

---

## Participants

---

## Sequence

---

## Decision Points

---

## Parallel Sections

---

## Validation Gates

---

## Rollback Procedures

---

## Exit Criteria

---

## Metrics

---

## Example Run

---

## Postmortem Template

---

# Volume 8 — Best Practices

Categorias:

## Architecture

## Refactoring

## Prompting

## Context Management

## Token Economy

## Review

## Testing

## Governance

## Security

## MCP Usage

## Human-in-the-loop

---

# Volume 9 — Anti-pattern Encyclopedia

Formato:

## Symptom

## Root Cause

## Example

## Consequences

## Detection

## Prevention

## Recovery

Exemplos:

* agent overlap;
* responsibility ambiguity;
* excessive delegation;
* context explosion;
* prompt drift;
* authority inversion;
* hidden coupling.

---

# Volume 10 — Troubleshooting Manual

Categorias:

* context;
* memory;
* MCP;
* orchestration;
* hallucination;
* governance;
* performance;
* cost.

---

# Volume 11 — Case Studies

Cada caso:

* contexto;
* objetivo;
* estratégia;
* agentes envolvidos;
* skills utilizadas;
* erros;
* correções;
* resultado.

---

# Volume 12 — FAQ

Objetivo:

capturar conhecimento recorrente e implícito.

---

# Volume 13 — Glossary

Definições formais.

Nenhum termo operacional poderá existir sem definição explícita.

---

# Volume 14 — Quick Reference

Objetivo:

consulta em menos de 30 segundos.

Formato:

## Quero fazer X

Use:

* agente
* skill
* workflow
* MCP
* comando

---

# Best Practices

## BP-001

Toda capability deve responder:

* o que faz;
* quando usar;
* quando não usar;
* qual alternativa existe.

---

## BP-002

Todo agente deve possuir limites explícitos.

---

## BP-003

Toda skill deve possuir anti-patterns documentados.

---

## BP-004

Todo workflow deve possuir rollback.

---

## BP-005

Toda integração externa deve possuir estratégia de degradação.

---

## BP-006

Todo comportamento emergente recorrente deve ser promovido a documentação explícita.

---

## BP-007

Nenhum conhecimento operacional crítico pode permanecer exclusivamente implícito.

---

# TODO

## Fase 1

* [ ] Criar estrutura de diretórios.
* [ ] Criar template global.
* [ ] Definir taxonomia oficial.

---

## Fase 2

* [ ] Documentar todos os agentes.
* [ ] Documentar todas as skills.
* [ ] Documentar todos os comandos.
* [ ] Documentar todos os MCPs.

---

## Fase 3

* [ ] Documentar workflows oficiais.
* [ ] Documentar playbooks.
* [ ] Documentar anti-patterns.

---

## Fase 4

* [ ] Produzir casos reais.
* [ ] Produzir troubleshooting.
* [ ] Produzir FAQ.

---

## Fase 5

* [ ] Revisão cruzada.
* [ ] Gap analysis.
* [ ] Cobertura de 100%.

---

## Definition of Done

O guia será considerado concluído quando:

* um humano sem conhecimento prévio atingir proficiência operacional;
* um agente recém-instanciado atingir utilização eficiente da stack;
* nenhuma funcionalidade relevante exigir conhecimento tribal;
* todas as decisões operacionais importantes puderem ser tomadas utilizando exclusivamente o guia.

---

## Consequences

Benefícios esperados:

* onboarding drasticamente reduzido;
* menor dependência do autor original;
* aumento da reutilização;
* redução de erros operacionais;
* maior portabilidade entre frameworks;
* maior longevidade da stack.

Risco principal:

* obsolescência documental.

Mitigação:

* documentação versionada e tratada como código.
