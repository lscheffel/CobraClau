# Volume 13 — Glossary

> **Objetivo:** Definições formais de todos os termos operacionais. Nenhum termo crítico pode existir sem definição explícita.

---

## A

**Acceptance Criteria** — Condições mensuráveis que definem quando uma task/feature está "done". Extraídas do TODO, validadas por `acceptance-testing` skill.

**ADR (Architecture Decision Record)** — Documento versionado capturando: Context, Decision, Consequences, Status. Formato: `docs/adr/ADR-XXX.md`. Status: Proposed → Accepted → Superseded.

**Agent** — Persona operacional especializada com: modelo LLM, permissões, prompt de sistema, modo (primary/subagent/all). Carrega AGENTS.md + skills + MCPs do escopo.

**Anti-pattern** — Padrão recorrente de falha operacional. Documentado com: Symptom, Root Cause, Example, Consequences, Detection, Prevention, Recovery.

**Artifact** — Qualquer arquivo produzido/consumido pela stack: ADR, Blueprint, TODO, Execution Contract, Plan, Report, Code, Docs.

**Artifact Map** — Mapa consolidado descoberto pelo Workflow 1 (Artifact Resolution): ADR, Blueprint, TODO, related_skills, impacted files.

---

## B

**Big Bang** — Anti-pattern: implementar todas as tasks de uma vez sem validação intermediária. Proibido pela `implementation` skill.

**Blueprint** — Plano detalhado derivado da ADR: tarefas, dependências, rollback criteria, validation gates. Arquivo: `ADR-XXX-BP.md`.

**Branch by Abstraction** — Técnica de refactoring: criar interface comum, migrar incrementalmente, swap implementation. Usado em `refactoring` skill.

---

## C

**Capability** — Qualquer unidade operacional da stack: skill, agent, command, MCP, workflow, rule. Deve satisfazer BP-001 (Capability Contract).

**Capability Contract** — BP-001: Toda capability deve responder: O que faz? Quando usar? Quando não usar? Qual alternativa?

**CI (Continuous Integration)** — Pipeline automatizado: build + lint + typecheck + tests + doc reconciliation. Fail = block merge.

**Command** — Atalho invocável via `/nome` em arquivo `.md` nos diretórios `command/`. Suporta variáveis `$1`, `$ARGUMENTS`, `@file`, `` !`cmd` ``.

**Context** — Janela de tokens + estado da sessão (AGENTS.md, skills, MCPs, history). Gerenciado via `/compact`, `/new`, `kilo_local_recall`.

**Context Explosion** — Anti-pattern: agent carrega contexto excessivo (>70%), qualidade degrada, alucinações aumentam.

---

## D

**DAG (Directed Acyclic Graph)** — Grafo de dependências entre tasks. Topological sort define ordem de execução. Construído no Workflow 3.

**Degradação Graciosa** — BP-005: Falha de MCP não derruba sessão. Try/catch + fallback (cache, manual, skip).

**Decision Authority** — Domínio de decisão exclusivo de um agente. Ex: Architect = technology selection, service boundaries.

**Doc Drift** — Divergência entre documentação e código. Detectado por `documentation-reconciliation` skill.

**DSG (Definitive Stack Guide)** — Este guia. 14 volumes. Fonte canônica de verdade operacional.

---

## E

**Execution Contract** — Contrato obrigatório pré-implementação. Valida: ADR status, Blueprint, TODO, branch, workspace, acceptance criteria, rollback criteria. Workflow 2.

**Execution Loop** — Modelo incremental: Select → Execute → Validate → Document → Mark Done. Uma task por vez.

**Execution Report** — Relatório final de implementação: summary, tasks, validations, risks, debt, recommendations.

---

## F

**Failure Mode** — Como uma skill/workflow/agent falha. Documentado no `SKILL.md` e `Anti-pattern Encyclopedia`.

**Fan-out / Fan-in** — Paralelismo: fan-out = dispatch tasks independentes; fan-in = consolidar resultados. `dispatching-parallel-agents` skill.

---

## G

**Gate** — Ponto de validação obrigatório. Exemplos: Execution Contract, Validation Gate (build/lint/test), Documentation Sync.

**Glossary** — Este volume. Definições formais de todos os termos operacionais.

**Governance** — Camada de processos: ADR lifecycle, review gates, versioning, permissions, compliance.

---

## H

**Hallucination** — Agent gera output não grounded em realidade (inventa APIs, files, schemas). Mitigado por `verification-before-completion` + cite sources.

**Handoff** — Transferência de task entre agents. Requer `task-progress.md` + contexto extraído (subagents não herdam MCP).

**Hard Constraint** — Regra enforceada por código/build (camada 1 de precedência). Ex: typecheck fail = build fail.

---

## I

**Implementation** — Skill que executa ADR aprovada via 8 workflows (Artifact Resolution → Execution Report).

**Internal Reasoning Pattern** — Padrão cognitivo da skill. Ex: decomposition, critique loop, refinement, verification.

---

## K

**Kilo** — Produto comercial (kilo.ai) com stack de agents/skills/MCPs/commands/governance.

**KiloCode** — Fork open-source do Kilo. Config legacy em `~/.kilocode/`.

---

## M

**MCP (Model Context Protocol)** — Protocolo padronizado para expor tools externas como functions chamáveis. 9 MCPs configurados.

**MCP Tool Permission** — Permissão no `kilo.json` para tools MCP. Padrão: `{server}_{tool}` (ex: `github_create_pull_request`).

**Mental Model** — Volume 1. Modelo compartilhado humano/agente sobre: Agent, Skill, Workflow, Rule, MCP, Command.

---

## N

**Non-Scope** — Seção obrigatória no `SKILL.md`: o que a skill deliberadamente NÃO faz. Previne misuse.

---

## P

**Plan** — Arquivo `.md` em `.kilo/plans/` produzido por `architect` agent. Contém: context, decisions, risks, validation steps, ordered task list.

**Prompt Drift** — Anti-pattern: agent behavior muda ao longo da sessão, ignora constraints iniciais. Fix: `/compact` periódico.

---

## Q

**Quick Reference** — Volume 14. Consulta < 30s: "Quero fazer X → Use: agent/skill/workflow/MCP/command".

---

## R

**Recovery Strategy** — Como recuperar de failure mode. Documentado no `SKILL.md` e `Anti-pattern Encyclopedia`.

**Rollback** — Reversão controlada: `git reset --hard <commit>` + `rollback-report.md`. Obrigatório em todo workflow (BP-004).

**Rule** — Constraint operacional com precedência definida. 7 camadas: Hard Constraint → Execution Contract → Validation Gates → Agent Permissions → Skill Criteria → Best Practices → Conventions.

---

## S

**Scope** — Seção obrigatória no `SKILL.md`: o que a skill faz. Define quando usar.

**Skill** — Unidade de conhecimento operacional reutilizável, versionável, invocável. Arquivo `SKILL.md` + templates/scripts. 97 skills globais.

**Skill Shopping** — Anti-pattern: carregar muitas skills para task simples. Fix: `find-skills` → 1-2 skills.

**Strangler Fig** — Padrão de migração legacy: nova impl lado a lado, gradual cutover, old code deleted quando covered.

**Subagent** — Agent instanciado via `task` tool. Não herda MCPs. Kilo passa dados no prompt.

**Success Criteria** — Como saber que skill/task executou corretamente. Obrigatório no `SKILL.md` e TODO.

**Synergy** — Combinação de skills/agents que funcionam bem juntos. Documentado no `SKILL.md`.

---

## T

**Task** — Unidade de trabalho no TODO. Estados: Pendente → Em andamento → Concluído / Bloqueado / Pausado. Máximo 1 "Em andamento".

**TODO** — Lista de tasks com estados + dependências. Arquivo: `ADR-XXX-TODO.md`. Input para DAG.

**Token Economy** — Gestão de custo: right model (temp), `/compact`, batch ops, subtasks, cache, search antes de read.

**Tribal Knowledge** — Conhecimento crítico não documentado. Anti-pattern (BP-007). Regra: se perguntado 2x → documentar AGORA.

---

## V

**Validation Gate** — Checkpoint obrigatório: Build + Lint + Typecheck + Tests + Architectural + Doc Sync. Falha = task bloqueada.

**Vibe Coding** — Modo intent-driven: developer guia agent com direção, não comandos detalhados. Skill `vibe-coding`.

---

## W

**Workflow** — Sequência declarativa orquestrando: agents, skills, MCPs, human decisions, validation gates. 8 workflows oficiais no Volume 7.

**Workflow Cookbook** — Volume 7. Workflows prontos: implement-adr, new-feature, refactor-legacy, incident-response, etc.

---

## Referência Cruzada Rápida

| Termo | Volume Principal | Volumes Relacionados |
|---|---|---|
| ADR | 0, 1, 2, 7 | 3 (adr-generator), 4 (architect), 8 (BP) |
| Agent | 0, 1, 4 | 3 (agent-development), 7, 8 |
| Blueprint | 1, 7 | 2, 8 |
| Command | 0, 1, 5 | 7 |
| Context | 1, 10 | 8 |
| DAG | 7 | 1 (workflow), 8 |
| DSG | 0 | All |
| Execution Contract | 1, 7 | 2, 8 |
| MCP | 0, 1, 6 | 7, 8, 10 |
| Rollback | 1, 7 | 2, 8, 10 |
| Skill | 0, 1, 3 | 4, 7, 8 |
| TODO | 1, 7 | 2, 8 |
| Workflow | 0, 1, 7 | 4, 8 |