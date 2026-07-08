# Volume 14 — Quick Reference

> **Objetivo:** Consulta em menos de 30 segundos — "Quero fazer X → Use Y".

---

## Quick Decision Matrix

| Quero... | Use |
|---|---|
| **Planejar** feature/ADR | `architect` + `writing-plans` |
| **Implementar** ADR aprovada | `implementation` skill |
| **Executar** plano aprovado | `executing-plans` skill |
| **Criar ADR** | `adr-generator` skill |
| **Refatorar** legacy | `reverse-engineering-specs` → `refactoring` |
| **Debug** sistemático | `systematic-debugging` skill |
| **Testes** do zero | `test-driven-development` |
| **Validar** antes de done | `verification-before-completion` |
| **Review** código | `code-review` skill |
| **Docs** escrever/atualizar | `docs-specialist` agent + `documentation` skill |
| **UI/React/TS** | `frontend-specialist` agent + `frontend-design` |
| **API design** | `api-design` skill |
| **CI/CD** setup | `deployment` + `senior-devops` |
| **Performance** | `performance-optimization` |
| **Security audit** | `security-review` |
| **MCP docs** library | `context7-mcp` |
| **GitHub ops** | `github` MCP |
| **Notion** pages/DB | `notion` MCP |
| **Google Drive/Sheets** | `google-workspace` MCP |
| **Knowledge graph** | `memory` MCP |
| **Browser automation** | `playwright` MCP |
| **Git avançado** | `git` MCP |
| **Filesystem** | `filesystem` MCP |
| **Nova skill** | `skill-creator` + `writing-skills` |
| **Descobrir skill** | `find-skills` |
| **Multi-agent** coordenação | `agent-orchestration` |
| **Parallel subagents** | `dispatching-parallel-agents` |
| **Onboard** novo agente | Workflow 5 (agent-onboarding) |
| **Incident response** | Workflow 4 (incident-response) |

---

## Agent Quick Pick

| Task | Agent |
|---|---|
| "Planeje isto" | Architect |
| "Revise este PR" | Code Reviewer |
| "Documente isto" | Docs Specialist |
| "Componente React" | Frontend Specialist |
| "Escreva testes" | Test Engineer |
| "Implemente feature" | Code (default) |

---

## Skill Categories (Top 20 Most Used)

| Categoria | Skills |
|---|---|
| **Governance** | `adr-generator`, `writing-plans`, `implementation`, `executing-plans`, `governance`, `release`, `spec-writing`, `prd-generation` |
| **Code Quality** | `clean-code`, `refactoring`, `architecture-review-kilo`, `ddd`, `systematic-debugging` |
| **Testing** | `test-driven-development`, `testing`, `testing-strategy`, `acceptance-testing`, `code-review`, `verification-before-completion` |
| **Agent Meta** | `agent-development`, `agent-orchestration`, `find-skills`, `skill-creator`, `writing-skills`, `using-superpowers`, `using-toolkit` |
| **Docs** | `documentation`, `documentation-reconciliation`, `tech-docs-generator`, `changelog-generator`, `repo-bootstrap` |
| **Frontend** | `frontend-design`, `ui-ux-pro-max`, `ui-design-system`, `react-best-practices`, `webapp-testing` |
| **Backend** | `api-design`, `database-schema-design`, `data-modeling`, `deployment`, `observability`, `senior-backend`, `senior-devops` |
| **AI/Prompting** | `prompt-engineering`, `senior-prompt-engineer`, `llm-as-judge` |
| **Utilities** | `git`, `git-commit-helper`, `using-git-worktrees`, `context7-mcp`, `mcp-builder`, `pdf-processing`, `xlsx-processing` |

---

## MCP Selection Guide

| Necessidade | MCP |
|---|---|
| Arquivos locais | `filesystem` |
| Browser moderno | `playwright` |
| Browser com launch options | `puppeteer` |
| Docs de lib/framework | `context7` |
| GitHub (PR, issues, files) | `github` |
| Git local avançado | `git` |
| Notion pages/DBs | `notion` |
| Knowledge graph | `memory` |
| Google Drive/Sheets/Gmail | `google-workspace` |

---

## Commands Essenciais

| Comando | Descrição |
|---|---|
| `/help` | Ajuda completa |
| `/agents` | Trocar agente |
| `/models` | Trocar modelo |
| `/mcps` | Toggle MCPs |
| `/new` / `/clear` | Nova sessão |
| `/compact` | Summarizar contexto |
| `/sessions` | Listar sessões |
| `/share` | Compartilhar sessão |
| `/undo` | Desfazer mensagem |
| `/themes` | Trocar tema |
| `/status` | Status sistema |

---

## Validation Gates (Obrigatórios)

```
✅ Build pass
✅ Lint pass (zero warnings se configured)
✅ Typecheck pass
✅ Unit tests pass
✅ Integration tests pass (se exist)
✅ Architectural validation (vs ADR)
✅ Documentation sync verified
```

**Regra:** Task ≠ "Concluída" sem TODOS passarem.

---

## Execution Loop (Por Task)

```
1. Verify deps = Concluído
2. Mark = Em andamento
3. Generate task-progress.md
4. Execute changes
5. Continuous Validation (all gates)
6. If PASS:
   ├── Update docs
   ├── Mark = Concluído
   └── Update task-progress.md
7. If FAIL (max 3 retries):
   ├── Analyze root cause
   ├── Fix & re-validate
   └── If still fail → Mark = Bloqueado
8. Re-evaluate dependent tasks
```

---

## File Locations Quick Ref

```
Global Config:     ~/.config/kilo/kilo.json + kilo.jsonc
Global Agents:     ~/.config/kilo/agents/*.md
Global Skills:     ~/.config/kilo/skills/{name}/SKILL.md
Global Commands:   ~/.config/kilo/command/*.md
Project Config:    .kilo/kilo.json + .kilo/agents/ + .kilo/commands/ + .kilo/skills/
ADRs:              docs/adr/ADR-XXX.md + ADR-XXX-BP.md + ADR-XXX-TODO.md
DSG (this guide):  docs/dsg/volume-{0-14}/
```

---

## Permission Patterns

```json
// Agent permissions
"edit": { "*.md": "allow", "*": "deny" }
"bash": "allow"
"mcp": "deny"

// MCP tool permissions
"github_*": "ask",
"github_get_file_contents": "allow",
"github_delete_file": "deny"
```

---

## Model Selection Guide

| Task Type | Temp | Top-p | Model Suggestion |
|---|---|---|---|
| Code/Implementation | 0.4 | 0.9 | `kilo/poolside/laguna-m.1:free` |
| Planning/Architecture | 0.5 | 0.9 | `kilo/nvidia/nemotron-3-ultra-550b-a55b:free` |
| Brainstorming/Creative | 1.0 | 0.95 | `kilo/stepfun/step-3.7-flash:free` |
| Debug/Analysis | 0.25 | 0.9 | `kilo/poolside/laguna-m.1:free` |

---

## Anti-pattern Quick Check

```
☐ Skill shopping? (use find-skills → 1-2 skills)
☐ Implicit skill use? (always explicit skill tool)
☐ Big bang? (1 task → validate → commit)
☐ Skip validation? (hard rule: no)
☐ Doc drift? (sync same commit)
☐ MCP no fallback? (always try/catch + fallback)
☐ Over-permissioned MCP? (minimum needed)
☐ Authority inversion? (check Decision Authority)
☐ Hidden coupling? (explicit contracts)
☐ Prompt drift? (/compact every 2-3 tasks)
☐ Tribal knowledge? (document NOW if asked 2x)
```

---

## Emergency Commands

| Situação | Comando |
|---|---|
| Session corrupted | `/new` → reload AGENTS.md + Vol 0 + Vol 14 |
| All MCPs down | Disable in kilo.json → use native tools |
| Validation gates failing | Check local → check CI → architecture-review |
| Knowledge graph corrupt | `memory_read_graph` → backup → delete dupes → recreate |

---

## Token Economy Cheatsheet

| Ação | Tokens Economizados |
|---|---|
| `/compact` proativo | ~40%/session |
| `read_multiple_files` vs N `read` | ~70% |
| Subtask para ops longas | Isola contexto caro |
| Right model per task | Evita overkill |
| Cache MCP (memory) | ~90% repeat calls |
| Search antes de read | Evita reads desnecessários |

---

## Glossary Minimal (Termos Críticos)

| Termo | Definição |
|---|---|
| **ADR** | Architecture Decision Record — decisão arquitetural versionada |
| **Blueprint** | Plano detalhado derivado da ADR (tasks, deps, rollback) |
| **Execution Contract** | Validação pré-implantação (artifacts, env, criteria) |
| **DAG** | Directed Acyclic Graph — ordem de execução via topological sort |
| **Capability** | Skill, agent, command, MCP, workflow, rule |
| **Degradação Graciosa** | MCP falha → fallback, não crash |
| **Tribal Knowledge** | Conhecimento não documentado (proibido: BP-007) |
| **Context Explosion** | Contexto >70% → qualidade cai exponencialmente |

---

## One-Page Workflow: New Feature

```
IDEA
  │
  ▼
spec-writing (JTBD + acceptance criteria)
  │
  ▼
[Arquitetural?] ──Yes──▶ adr-generator → Accepted
  │                            │
  No                           ▼
  │                     writing-plans (Blueprint + TODO)
  ▼                            │
writing-plans                  ▼
  │                     implementation (Execution Contract → DAG → Loop)
  │                            │
  ▼                            ▼
  └─────────────▶ validation gates (build+lint+test+typecheck+arch+docs)
                        │
                        ▼
                   Release (changelog+tag+deploy)
```

---

## DSG Navigation

| Volume | Para que serve | Quando usar |
|---|---|---|
| **Vol 0** | Executive Overview | Primeiro contato (<5 min) |
| **Vol 1** | Mental Model | Entender blocos fundamentais |
| **Vol 2** | Philosophy | Entender *por que* decisões |
| **Vol 3** | Skills Handbook | Escolher skill certa (97 skills) |
| **Vol 4** | Agent Handbook | Escolher agent certo (5 agents) |
| **Vol 5** | Commands Reference | Criar/usar comandos |
| **Vol 6** | MCP Handbook | Configurar/usar 9 MCPs |
| **Vol 7** | Workflow Cookbook | Workflows prontos (8 workflows) |
| **Vol 8** | Best Practices | Regras de ouro (BP-001 a BP-007) |
| **Vol 9** | Anti-patterns | Diagnosticar/evitar falhas |
| **Vol 10** | Troubleshooting | Resolver problemas por categoria |
| **Vol 11** | Case Studies | Exemplos reais (5 cases) |
| **Vol 12** | FAQ | Perguntas recorrentes |
| **Vol 13** | Glossary | Definições formais |
| **Vol 14** | Quick Reference | **ESTÁ AQUI** — consulta <30s |

---

**Última atualização:** 2026-07-08  
**Versão DSG:** 1.0 (ADR-0142)  
**Status:** Complete — 14 volumes, 97 skills, 5 agents, 9 MCPs, 8 workflows