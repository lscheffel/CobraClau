# Volume 3 — Skills Handbook

> **Objetivo:** Referência completa de todas as 97 skills disponíveis na stack global.

---

## Taxonomia de Skills

| Categoria | Skills | Propósito |
|---|---|---|
| **Governance & Planning** | 12 | ADRs, planning, roadmap, governance, implementation |
| **Development & Code** | 18 | Clean code, refactoring, TDD, DDD, architecture review |
| **Agent & Skill Meta** | 10 | Agent development, orchestration, skill creation, find-skills |
| **Documentation** | 8 | Tech docs, reconciliation, ADR generation, changelog |
| **Testing & Quality** | 10 | Testing strategies, acceptance, debugging, code review |
| **Frontend & UI** | 8 | Design systems, React, canvas, web guidelines |
| **Backend & Infrastructure** | 10 | API design, database, deployment, observability, security |
| **Data & AI** | 5 | Data modeling, ML pipelines, prompt engineering |
| **Utilities & Specialized** | 16 | Git, MCP, context7, email, PDF, Excel, etc. |

---

## Índice Rápido (Todas as 97 Skills)

| Skill | Categoria | Maturidade | Uso Principal |
|---|---|---|---|
| `acceptance-testing` | Testing | Stable | Validação comportamental com gates |
| `adr-generator` | Governance | Stable | Cria ADRs padronizados |
| `agent-development` | Agent Meta | Stable | Build AI agents com tool use, memory |
| `agent-md-refactor` | Agent Meta | Stable | Refatorar AGENTS.md/CLAUDE.md |
| `agent-orchestration` | Agent Meta | Stable | Multi-agent coordination |
| `agents-md-generator` | Agent Meta | Stable | Gera AGENTS.md adaptativo |
| `api-design` | Backend | Stable | REST/GraphQL contracts |
| `architecture-review-kilo` | Code Quality | Stable | SOLID, Clean Arch, DDD review |
| `artifacts-builder` | Frontend | Stable | HTML/CSS/JS standalone apps |
| `auto-improvement` | Meta | Experimental | Self-improving loops |
| `autonomous-loop` | Meta | Experimental | Ralph-style iterative dev |
| `brainstorming` | Planning | Stable | Exploração pré-implementação |
| `canvas-design` | Frontend | Stable | HTML Canvas, D3, visualizações |
| `changelog-generator` | Docs | Stable | Changelogs de git commits |
| `circuit-breaker` | Meta | Stable | Rate limits, cooldown, recovery |
| `clean-code` | Code Quality | Stable | Smells, SOLID, refactoring |
| `code-review` | Testing | Stable | Pre-commit/pre-merge validation |
| `content-creator` | Content | Stable | Marketing copy, social, ads |
| `content-research-writer` | Content | Stable | Whitepapers, research, citations |
| `context7-mcp` | Utility | Stable | Library docs via Context7 |
| `database-schema-design` | Backend | Stable | SQL/NoSQL schema, migrations |
| `data-modeling` | Backend | Stable | Relational/NoSQL modeling |
| `ddd` | Code Quality | Stable | Domain-Driven Design |
| `deployment` | Backend | Stable | CI/CD, deploy checklists |
| `dispatching-parallel-agents` | Agent Meta | Stable | Fan-out/fan-in parallel tasks |
| `documentation` | Docs | Stable | README, ADR, API docs patterns |
| `documentation-reconciliation` | Docs | Stable | Audit docs vs code |
| `docx-processing` | Utility | Stable | Word docs, templates, mail merge |
| `email-composer` | Utility | Stable | Professional emails |
| `executing-plans` | Governance | Stable | Executa planos com TDD/gates |
| `file-organizer` | Utility | Stable | Project structure, monorepo |
| `find-skills` | Agent Meta | Stable | Descobre skills instaladas |
| `finishing-a-development-branch` | Git | Stable | PR prep, merge, cleanup |
| `frontend-design` | Frontend | Stable | Production-grade UI components |
| `frontend-ui-design` | Frontend | Stable | Component architecture, state |
| `git` | Git | Stable | Conventional commits, branching |
| `git-commit-helper` | Git | Stable | Commits, versioning, changelog |
| `governance` | Governance | Stable | Review processes, versioning |
| `implementation` | Governance | Stable | ADR→Blueprint→TODO→Execution |
| `index.json` | Registry | - | Skills registry index |
| `laravel-boost` | Backend | Stable | Laravel AI-assisted dev |
| `laravel-specialist` | Backend | Stable | Eloquent, Livewire, Pest |
| `llm-as-judge` | Testing | Stable | Subjective quality eval |
| `mcp-builder` | Utility | Stable | Build MCP servers |
| `mobile-design` | Frontend | Stable | React Native, Flutter, SwiftUI |
| `observability` | Backend | Stable | Logging, metrics, tracing |
| `pdf-processing` | Utility | Stable | PDF gen, extract, forms |
| `performance-optimization` | Backend | Stable | Web Vitals, DB, bundles |
| `php-specialist` | Backend | Stable | Modern PHP 8.x |
| `planning` | Governance | Stable | Strategic/tactical planning |
| `prd-generation` | Governance | Stable | PRD from idea to spec |
| `prompt-engineering` | AI | Stable | Few-shot, CoT, structured output |
| `ralph-status` | Meta | Stable | Autonomous loop reporting |
| `react-best-practices` | Frontend | Stable | Hooks, Server Components |
| `receiving-code-review` | Testing | Stable | Handle review feedback |
| `refactoring` | Code Quality | Stable | Strangler Fig, BbA, legacy |
| `release` | Governance | Stable | Release process, semver |
| `repo-bootstrap` | Utility | Stable | New repo governance structure |
| `requesting-code-review` | Testing | Stable | Pre-merge validation |
| `resilient-execution` | Meta | Stable | 3+ approaches before escalate |
| `reverse-engineering-specs` | Code Quality | Stable | Specs from legacy code |
| `roadmap-planning` | Planning | Stable | Strategic roadmap |
| `roadmap-update` | Planning | Stable | Reprioritize roadmap |
| `security-review` | Backend | Stable | Vulns, secrets, crypto |
| `self-learning` | Meta | Stable | Auto-discovers project context |
| `senior-architect` | Backend | Stable | System design, ADRs, scaling |
| `senior-backend` | Backend | Stable | API, microservices, events |
| `senior-data-scientist` | Data | Stable | ML pipelines, stats, viz |
| `senior-devops` | Backend | Stable | CI/CD, Docker, K8s, IaC |
| `senior-frontend` | Frontend | Stable | React/Next.js/TS production |
| `senior-fullstack` | Backend | Stable | End-to-end TypeScript |
| `senior-prompt-engineer` | AI | Stable | Prompt design, optimization |
| `seo-optimizer` | Frontend | Stable | Technical SEO, Core Web Vitals |
| `skill-audit-bulletin` | Agent Meta | Stable | Audit skill quality |
| `skill-creator` | Agent Meta | Stable | Write new skills |
| `spec-writing` | Governance | Stable | JTBD specs, acceptance criteria |
| `subagent-driven-development` | Agent Meta | Stable | Parallel subagent execution |
| `systematic-debugging` | Testing | Stable | 4-phase debugging |
| `task-decomposition` | Planning | Stable | WBS, critical path |
| `task-management` | Planning | Stable | Track multi-step impl |
| `tech-docs-generator` | Docs | Stable | API refs, arch docs from code |
| `test-driven-development` | Testing | Stable | Strict RED-GREEN-REFACTOR |
| `testing` | Testing | Stable | Unit/integration/E2E patterns |
| `testing-strategy` | Testing | Stable | Framework, coverage, infra |
| `ui-design-system` | Frontend | Stable | Design tokens, Tailwind v4 |
| `ui-ux-pro-max` | Frontend | Stable | Full UI/UX intelligence |
| `using-git-worktrees` | Git | Stable | Parallel dev environments |
| `using-superpowers` | Meta | Stable | Skill discovery protocol |
| `using-toolkit` | Meta | Stable | All 64 toolkit skills |
| `ux-researcher-designer` | Frontend | Stable | User research, personas |
| `verification-before-completion` | Testing | Stable | 5-step HARD-GATE protocol |
| `vibe-coding` | Meta | Stable | Intent-driven AI dev |
| `webapp-testing` | Testing | Stable | Playwright E2E, a11y, visual |
| `web-design-guidelines` | Frontend | Stable | Web Interface Guidelines |
| `writing-plans` | Governance | Stable | Implementation plans |
| `writing-skills` | Agent Meta | Stable | Create skills/commands/agents |
| `xlsx-processing` | Utility | Stable | Excel reports, formulas |

---

## Skills por Categoria (Detalhadas)

---

### 🏛️ Governance & Planning (12)

#### `adr-generator`
**Mission:** Cria Architecture Decision Records padronizados com contexto, decisão, consequências, status.
**Quando usar:** Nova decisão arquitetural, tradeoff técnico, mudança de stack.
**Non-scope:** Implementação da decisão (use `implementation`).
**Inputs:** Título, contexto, opções consideradas.
**Outputs:** `docs/adr/ADR-XXX.md` + `ADR-XXX-BP.md` + `ADR-XXX-TODO.md`.
**Pattern:** Template-driven → validação de campos obrigatórios.
**Synergies:** `writing-plans`, `implementation`, `governance`.

#### `writing-plans`
**Mission:** Gera planos de implementação estruturados a partir de specs/ADRs.
**Quando usar:** Transição ADR→execução, breakdown de features, roadmap técnico.
**Non-scope:** Execução do plano (use `implementation`/`executing-plans`).
**Outputs:** Lista numerada de tasks com arquivos, critérios, dependências, complexidade.
**Pattern:** Spec analysis → phase division → task cards → validation.

#### `implementation`
**Mission:** Executa ADR aprovada via Execution Contract + DAG + validação contínua.
**Quando usar:** ADR status "Aceito" + Blueprint + TODO existem.
**Non-scope:** Planejamento (use `writing-plans`), ADR creation (use `adr-generator`).
**Workflows:** 8 workflows (Artifact Resolution → Execution Report).
**Anti-patterns:** Big Bang, skip validation, ignore contract.
**Synergies:** `testing`, `git`, `documentation`, `governance`.

#### `executing-plans`
**Mission:** Executa plano aprovado batch-by-batch com TDD, checkpoints, verification gates.
**Quando usar:** Plano existe (de `writing-plans`), precisa execução governada.
**Diff vs `implementation`:** Mais leve, foco em execução de plano já aprovado.

#### `planning`
**Mission:** Planejamento estratégico/tático — épicos, features, tasks, estimativas, priorização.
**Quando usar:** Início de projeto, roadmap, breakdown de iniciativa grande.
**Outputs:** Roadmap, backlog priorizado, estimativas.

#### `governance`
**Mission:** Define processos de review, aprovação, branching, semver, issues/PRs.
**Quando usar:** Setup de equipe, padronizar workflows, definir policies.

#### `release`
**Mission:** Processo de release, changelog, tag, deploy, rollback.
**Quando usar:** Preparar releases, publicar pacotes, versionamento semântico.

#### `spec-writing`
**Mission:** Especificações JTBD com acceptance criteria, sem detalhes de implementação.
**Quando usar:** Antes de qualquer feature/projeto, definir "o quê" não "como".

#### `prd-generation`
**Mission:** Product Requirements Document de ideia high-level para spec estruturada.
**Quando usar:** Nova feature/produto, discovery questions → structured PRD.

#### `roadmap-planning`
**Mission:** Roadmap estratégico — prioritização, épicos, stakeholder alignment, sequencing.

#### `roadmap-update`
**Mission:** Atualizar/reprioritizar roadmap — new initiatives, dependency slips, Now/Next/Later.

---

### 💻 Development & Code Quality (18)

#### `clean-code`
**Mission:** Code quality review, refactoring guidance, SOLID, naming, complexity, DRY.
**Quando usar:** Code smell detection, refactoring planning, naming review.

#### `refactoring`
**Mission:** Safe incremental refactoring — extraction, Strangler Fig, Branch by Abstraction.
**Quando usar:** Improve existing structure, migrate legacy, eliminate technical debt.
**Key techniques:** Tests first, small steps, characterization tests.

#### `architecture-review-kilo`
**Mission:** Detecta violações SOLID, Clean Arch, Hexagonal, DDD, code smells estruturais.
**Quando usar:** Arquitetura review, design evaluation, structural analysis.

#### `ddd`
**Mission:** Domain-Driven Design — Entities, VOs, Aggregates, Repositories, Domain Events.
**Quando usar:** Rich domain modeling, refactor anemic entities, bounded contexts.

#### `test-driven-development`
**Mission:** Strict RED-GREEN-REFACTOR — no production code without failing test.
**Quando usar:** New feature, bug fix, refactor, add behavior to existing modules.

#### `testing`
**Mission:** Test patterns — unit, integration, E2E, contract. Pyramid, naming, practices.
**Quando usar:** Write tests, review coverage, define test strategy.

#### `testing-strategy`
**Mission:** Choose testing approach — frameworks, coverage thresholds, infra, patterns.
**Quando usar:** New project setup, CI/CD design, coverage audit, framework migration.

#### `acceptance-testing`
**Mission:** Spec-to-code validation with behavioral gates — prevents false completion.
**Quando usar:** Feature completion, pre-merge, release readiness.

#### `code-review`
**Mission:** Senior engineer review — quality, security, performance, maintainability.
**Quando usar:** Task completion, pre-commit, pre-merge, post-refactor.

#### `receiving-code-review`
**Mission:** Handle feedback rigorously — verify, don't blindly implement.
**Quando usar:** Receiving PR reviews, especially unclear/questionable suggestions.

#### `requesting-code-review`
**Mission:** Prepare for review — verify work meets requirements before merge.
**Quando usar:** Completing tasks, major features, pre-merge.

#### `systematic-debugging`
**Mission:** 4-phase investigation — prevents shotgun debugging.
**Quando usar:** Test failure:** Test failure, runtime error, unexpected behavior, prod incident.

#### `verification-before-completion`
**Mission:** 5-step HARD-GATE — fresh evidence before claiming done.
**Quando usar:** Before ANY completion claim.

#### `resilient-execution`
**Mission:** 3 genuinely different approaches before escalating.
**Quando usar:** Task fails, approach doesn't work, tempted to say "can't do this".

#### `senior-backend`
**Mission:** API design, microservices, event-driven, DB, caching, observability.
**Quando usar:** REST/GraphQL, service arch, message queues, rate limiting.

#### `senior-fullstack`
**Mission:** End-to-end TypeScript — DB → API → UI with tRPC, Prisma, Next.js.

#### `senior-architect`
**Mission:** System design, ADRs, scalability, tradeoffs, NFRs, infra topology.

---

### 🤖 Agent & Skill Meta (10)

#### `agent-development`
**Mission:** Build AI agents — tool use, memory, planning, multi-agent, eval, guardrails.
**Quando usar:** "Build an agent", tool use patterns, agent loops, evaluation.

#### `agent-orchestration`
**Mission:** Multi-agent coordination — task decomp, model routing, handoff contracts, parallelism.
**Quando usar:** Complex tasks needing multiple agents, role definition, handoffs.

#### `agents-md-generator`
**Mission:** Generates adaptive AGENTS.md — detects project type, tech, patterns, governance.

#### `agent-md-refactor`
**Mission:** Refactor bloated AGENTS.md/CLAUDE.md — progressive disclosure, split files.

#### `find-skills`
**Mission:** Discover/install skills when user asks "how do I do X" or "skill for X".

#### `skill-creator`
**Mission:** Guide for creating effective skills — SKILL.md, triggers, testing.

#### `writing-skills`
**Mission:** Create new skills/commands/agents for Claude Code — SKILL.md, triggers.

#### `skill-audit-bulletin`
**Mission:** Audit existing skills — quality, completeness, actionability, risk scores.

#### `using-superpowers`
**Mission:** Skill discovery protocol — invoke skill BEFORE any response including questions.

#### `using-toolkit`
**Mission:** All 64 toolkit skills — find and use before any response.

---

### 📚 Documentation (8)

#### `documentation`
**Mission:** High-quality tech docs — README, ADRs, API guides, arch docs, docs-as-code.

#### `documentation-reconciliation`
**Mission:** Audit/reconcile canonical docs (README, CHANGELOG) + specific (ADR, BP, TODO) vs code.

#### `tech-docs-generator`
**Mission:** Generate/update docs from code — API refs, arch docs, README, component docs.

#### `changelog-generator`
**Mission:** User-facing changelogs from git commits — categorize, transform to customer-friendly.

#### `adr-generator` (ver Governance)

#### `spec-writing` (ver Governance)

#### `repo-bootstrap`
**Mission:** Initial repo structure — README, AGENTS.md, CHANGELOG, CONTRIBUTING, CI/CD examples.

#### `writing-plans` (ver Governance)

---

### 🧪 Testing & Quality (10)

#### `testing` / `testing-strategy` / `test-driven-development` / `acceptance-testing` / `code-review` / `receiving-code-review` / `requesting-code-review` / `systematic-debugging` / `verification-before-completion` / `llm-as-judge`

**Ver seção Development & Code Quality acima.**

---

### 🎨 Frontend & UI (8)

#### `frontend-design`
**Mission:** Distinctive production-grade UI — components, pages, artifacts, dashboards.

#### `frontend-ui-design`
**Mission:** Component architecture, responsive layouts, design systems, state management.

#### `ui-design-system`
**Mission:** Design tokens, component libraries, theme systems, Tailwind v4 responsive patterns.

#### `ui-ux-pro-max`
**Mission:** Full UI/UX intelligence — styles, palettes, fonts, UX guidelines, charts, accessibility.

#### `canvas-design`
**Mission:** HTML Canvas, SVG, data viz, generative art, D3.js, interactive graphics.

#### `react-best-practices`
**Mission:** React patterns — hooks, composition, Server Components, error boundaries, optimization.

#### `web-design-guidelines`
**Mission:** Review UI for Web Interface Guidelines — accessibility, UX, best practices.

#### `webapp-testing`
**Mission:** Playwright E2E — screenshots, browser logs, visual regression, a11y, network mocking.

---

### ⚙️ Backend & Infrastructure (10)

#### `api-design`
**Mission:** RESTful/GraphQL APIs — endpoints, versioning, error contracts, pagination, idempotency.

#### `database-schema-design` / `data-modeling`
**Mission:** SQL/NoSQL schemas, migrations, relationships, indexes, performance strategies.

#### `deployment`
**Mission:** CI/CD pipelines, deploy configs, checklists, staging/prod, monitoring.

#### `observability`
**Mission:** Structured logging, metrics, distributed tracing, alerting for microservices.

#### `security-review`
**Mission:** Vulnerabilities, secrets, crypto, insecure dependencies in code.

#### `senior-devops`
**Mission:** CI/CD, Docker, K8s, IaC, monitoring, zero-downtime deployments.

#### `performance-optimization`
**Mission:** Slow loads, Web Vitals, DB timeouts, bundle size, scaling prep.

#### `mcp-builder`
**Mission:** Build MCP servers — tools, resources, prompts, transport, client integration.

#### `laravel-specialist` / `laravel-boost` / `php-specialist`
**Mission:** Laravel/PHP modern development — Eloquent, Livewire, queues, Pest, PHP 8.x.

---

### 📊 Data & AI (5)

#### `senior-data-scientist`
**Mission:** ML pipelines, statistical analysis, preprocessing, feature engineering, viz.

#### `prompt-engineering`
**Mission:** Effective prompts — structure, few-shot, CoT, role prompting, constraints.

#### `senior-prompt-engineer`
**Mission:** Prompt design, optimization, few-shot, structured output, evaluation, versioning.

#### `llm-as-judge`
**Mission:** LLM-based eval for subjective criteria — tone, aesthetics, UX, readability.

#### `context7-mcp`
**Mission:** Up-to-date library docs via Context7 — setup, code gen, framework references.

---

### 🔧 Utilities & Specialized (16)

#### `git` / `git-commit-helper` / `using-git-worktrees` / `finishing-a-development-branch`
**Mission:** Git workflows — conventional commits, branching, worktrees, PR prep, versioning.

#### `file-organizer`
**Mission:** Project structure — monorepo, feature-based, naming, barrel exports, config placement.

#### `docx-processing`
**Mission:** Word docs — generation, templates, formatting, mail merge, DOCX→PDF.

#### `pdf-processing`
**Mission:** PDFs — generation, extraction, forms, OCR, merge/split, watermark, metadata.

#### `xlsx-processing`
**Mission:** Excel — read/write, formulas, charts, conditional formatting, pivot tables.

#### `email-composer`
**Mission:** Professional emails — drafting, tone, templates, strategy, follow-ups.

#### `content-creator` / `content-research-writer`
**Mission:** Marketing copy vs research/whitepapers — different audiences, rigor.

#### `dispatching-parallel-agents`
**Mission:** Fan-out/fan-in parallel subagents for independent subtasks.

#### `subagent-driven-development`
**Mission:** Multi-task plans with independent subagent execution + two-stage review gates.

#### `task-decomposition` / `task-management`
**Mission:** Hierarchical breakdown, dependency mapping, effort estimation, WBS.

#### `self-learning`
**Mission:** Auto-discovers project context — codebase analysis, pattern recognition.

#### `auto-improvement` / `autonomous-loop` / `circuit-breaker` / `ralph-status` / `vibe-coding`
**Mission:** Meta-skills for autonomous iterative development loops.

---

## Como Escolher a Skill Certa

```
Pergunta                                    → Skill
"Como faço X?"                              → find-skills
"Nova decisão arquitetural"                 → adr-generator
"Plano de implementação"                    → writing-plans
"Executar ADR aprovada"                     → implementation
"Executar plano aprovado"                   → executing-plans
"Refatorar código legacy"                   → refactoring + reverse-engineering-specs
"Review de arquitetura"                     → architecture-review-kilo
"Debug sistemático"                         → systematic-debugging
"Testes do zero"                            → test-driven-development
"Validação antes de done"                   → verification-before-completion
"Build MCP server"                          → mcp-builder
"Docs da lib X"                             → context7-mcp
"UI production-grade"                       → frontend-design + ui-ux-pro-max
"API design"                                → api-design
"CI/CD setup"                               → deployment + senior-devops
"Multi-agent coordination"                  → agent-orchestration
"Parallel subagents"                        → dispatching-parallel-agents
"Onboarding novo projeto"                   → self-learning + repo-bootstrap
"Skill discovery"                           → using-superpowers / using-toolkit
```

---

## Anti-Patterns Comuns (Todas Skills)

| Anti-pattern | Sintoma | Prevenção |
|---|---|---|
| **Skill shopping** | Carrega 10 skills para tarefa simples | `find-skills` → 1-2 skills max |
| **Implicit skill use** | "O agente sabe" sem invocar | Sempre `skill` tool explícita |
| **Wrong tool for job** | `writing-plans` para debug | Mental Model (Vol 1) → escolha correta |
| **Skip validation** | Marca done sem gates | `verification-before-completion` obrigatório |
| **Doc drift** | Código ≠ docs | `documentation-reconciliation` em CI |