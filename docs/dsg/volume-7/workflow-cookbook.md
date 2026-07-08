# Volume 7 — Workflow Cookbook

> **Objetivo:** Workflows oficiais prontos para uso — cada um com goal, preconditions, sequence, gates, rollback.

> **⚠️ Fonte canônica = utilitários individuais.** Conforme **ADR-0142.1** (Implementado, arquivado em `docs/adr/archive/`), cada workflow e fluxo CI abaixo foi extraído para um arquivo utilitário atômico em `docs/kilosettings/ci-workflows/` (veja `INDEX.md`). Este Volume 7 é o **índice narrativo**; para executar, consulte o utilitário linkado em cada seção. Os fluxos CI também têm **pipeline actions reais** em `.github/workflows/` (`ci-validation-gates.yml`, `ci-doc-reconciliation.yml`, `ci-pre-merge-gate.yml`).

---

## Workflow 1: implement-adr

**Utilitário canônico:** [wf-implement-adr.md](../../kilosettings/ci-workflows/wf-implement-adr.md)

### Goal
Implementar uma ADR aprovada via Execution Contract + DAG + validação contínua.

### Preconditions
- [ ] ADR existe em `docs/adr/ADR-XXX.md` com status `Accepted`
- [ ] Blueprint existe em `docs/adr/ADR-XXX-BP.md`
- [ ] TODO existe em `docs/adr/ADR-XXX-TODO.md` com tasks e dependências
- [ ] Branch limpa (não main/master sem PR)
- [ ] Workspace limpo (sem uncommitted changes)

### Inputs
- ADR ID (ex: `ADR-0142`)

### Participants
| Papel | Agent/Skill |
|---|---|
| Orchestrator | `implementation` skill |
| Planner | `writing-plans` skill |
| Executor | `code` agent |
| Validator | `testing` + `code-review` skills |
| Documenter | `documentation` skill |

### Sequence
```
Phase 1: Artifact Resolution
├── Load ADR, Blueprint, TODO
├── Extract related_skills from ADR frontmatter
├── Map impacted files from Blueprint
└── Checkpoint: All artifacts exist & coherent

Phase 2: Execution Contract
├── Validate ADR status = Accepted
├── Validate Blueprint has tasks
├── Validate TODO has tasks with states
├── Validate branch & workspace
├── Extract acceptance criteria from TODO
├── Extract rollback criteria from Blueprint
└── Checkpoint: Contract signed (all fields valid)

Phase 3: Dependency Analysis & Execution Plan
├── Build DAG from TODO dependencies
├── Detect cycles (fail if exists)
├── Topological sort → execution order
├── Identify parallelizable tasks
├── Estimate total time
└── Checkpoint: DAG valid, order defined

Phase 4: Incremental Execution (per task in DAG order)
├── Verify dependencies = Concluído
├── Mark task = Em andamento
├── Generate task-progress.md
├── Execute changes (code + skill invocations)
├── Continuous Validation:
│   ├── Build
│   ├── Lint
│   ├── Typecheck
│   ├── Unit tests
│   ├── Integration tests (if exist)
│   ├── Architectural validation (vs ADR)
│   └── Documentation validation
├── If PASS:
│   ├── Update affected documentation
│   ├── Mark task = Concluído
│   └── Update task-progress.md
├── If FAIL (max 3 retries):
│   ├── Analyze root cause
│   ├── Fix & re-validate
│   └── If still fail → Mark = Bloqueado
└── Re-evaluate dependent tasks

Phase 5: Documentation Synchronization
├── Check ADR updates needed
├── Check Blueprint updates needed
├── Check README updates needed
├── Check related_skills updates needed
└── Checkpoint: No doc drift

Phase 6: Execution Report
├── Generate execution-report.md
├── Include: summary, tasks, validations, risks, debt, recommendations
└── Checkpoint: Report complete
```

### Decision Points
| Point | Condition | Action |
|---|---|---|
| Contract validation | Any field invalid | STOP; fix artifacts first |
| Cycle detection | Cycle in DAG | STOP; fix TODO dependencies |
| Validation gate | Any check fails | BLOCK task; 3 retries max |
| All tasks done | All = Concluído | PROCEED to Phase 5 |

### Parallel Sections
Tasks sem dependência mútua no DAG → executam em paralelo via `dispatching-parallel-agents`

### Validation Gates
| Gate | Checks | Pass Criteria |
|---|---|---|
| Pre-execution | Contract fields | All valid |
| Per-task | Build + Lint + Typecheck + Tests | All pass |
| Architectural | Code matches ADR decision | No drift |
| Documentation | Docs sync with code | `documentation-reconciliation` passes |

### Rollback Procedures
```bash
# Se task falha irrecuperavelmente:
git reset --hard HEAD~1  # Reverte último commit da task
# Se multiple tasks:
git reset --hard <commit-before-first-task>
# Sempre gere:
rollback-report.md com: motivo, tasks revertidas, ações corretivas
```

### Exit Criteria
- [ ] All tasks = Concluído
- [ ] All validation gates pass
- [ ] Documentation synchronized
- [ ] Execution report generated
- [ ] Branch ready for PR/merge

### Metrics
- Tasks completed / total
- Validation pass rate
- Time per task vs estimate
- Rollback count

### Example Run
```
ADR-0142: Definitive Stack Guide
Tasks: 142 (Vol 0-14)
Duration: ~4 hours
Parallel: Vol 3 (skills) + Vol 4 (agents) + Vol 6 (MCPs)
Validation: 100% pass
Rollbacks: 0
```

---

## Workflow 2: new-feature

**Utilitário canônico:** [wf-new-feature.md](../../kilosettings/ci-workflows/wf-new-feature.md)

### Goal
Da ideia à implementação validada: spec → plan → execute → review.

### Preconditions
- [ ] Problema/objetivo claro
- [ ] Stakeholder disponível para decisões

### Sequence
```
1. Spec Writing (spec-writing skill)
   ├── JTBD framework
   ├── Acceptance criteria
   └── SLC release planning

2. ADR Creation (adr-generator skill) — se arquitetural
   ├── Context, Decision, Consequences
   ├── Status: Proposed → Accepted
   └── Blueprint + TODO auto-generated

3. Planning (writing-plans skill)
   ├── Phase breakdown
   ├── Task cards with files, criteria, deps
   └── Complexity estimates

4. Execution (implementation skill OR executing-plans skill)
   ├── Per implement-adr workflow
   └── Validation gates per task

5. Review & Merge
   ├── code-review skill (pre-merge)
   ├── security-review skill (if applicable)
   └── documentation-reconciliation (CI)

6. Release (release skill)
   ├── Changelog generation
   ├── Version bump
   ├── Tag + deploy
   └── Rollback plan documented
```

### Decision Points
| Point | Condition | Action |
|---|---|---|
| Spec review | Stakeholder não alinhado | Iterate spec; não prossiga |
| ADR needed? | Impacta arquitetura/NFRs | Sim → adr-generator |
| Plan approval | Complexidade alta | Human approval gate |

---

## Workflow 3: refactor-legacy

**Utilitário canônico:** [wf-refactor-legacy.md](../../kilosettings/ci-workflows/wf-refactor-legacy.md)

### Goal
Refatorar código legacy com segurança: specs → characterization tests → incremental refactor.

### Preconditions
- [ ] Código legacy identificado
- [ ] Tests atuais passam (baseline)

### Sequence
```
1. Reverse Engineering (reverse-engineering-specs skill)
   ├── Exhaustive code path tracing
   ├── Behavioral specs (implementation-free)
   └── Edge cases documented

2. Characterization Tests (test-driven-development skill)
   ├── Capture current behavior as tests
   ├── Cover: happy path, edges, errors
   └── Baseline: all pass

3. Strangler Fig / Branch by Abstraction (refactoring skill)
   ├── Create abstraction layer
   ├── Incremental migration
   ├── Tests pass at each step
   └── Old code deleted when covered

4. Validation
   ├── All characterization tests pass
   ├── New implementation tests pass
   ├── Performance benchmarks
   └── Documentation updated
```

### Rollback
```bash
# Abstraction layer permite instant rollback:
git revert <migration-commits>
# Old code still works behind abstraction
```

---

## Workflow 4: incident-response

**Utilitário canônico:** [wf-incident-response.md](../../kilosettings/ci-workflows/wf-incident-response.md)

### Goal
Responder a incidente em produção: detect → diagnose → mitigate → resolve → postmortem.

### Preconditions
- [ ] Alert firing ou report recebido
- [ ] On-call disponível

### Sequence
```
1. Detect & Triage
   ├── Confirm incident (não false positive)
   ├── Severity: SEV-1/2/3
   ├── Start incident channel/log
   └── Assign incident commander

2. Diagnose (systematic-debugging skill)
   ├── Phase 1: Reproduce / gather evidence
   ├── Phase 2: Form hypothesis
   ├── Phase 3: Test hypothesis (minimal check)
   ├── Phase 4: Confirm root cause
   └── Document findings

3. Mitigate (immediate)
   ├── Apply workaround / rollback / scale
   ├── Verify mitigation works
   └── Communicate status

4. Resolve (permanent fix)
   ├── Root cause fix
   ├── Test fix in staging
   ├── Deploy with monitoring
   └── Verify resolution

5. Postmortem (blameless)
   ├── Timeline
   ├── Root cause (5 whys)
   ├── Impact
   ├── Action items (prevent recurrence)
   └── Share learnings
```

### Validation Gates
| Gate | Check |
|---|---|
| Mitigation | Service restored to SLA |
| Fix | Tests pass + no regression |
| Postmortem | Action items assigned + tracked |

---

## Workflow 5: agent-onboarding

**Utilitário canônico:** [wf-agent-onboarding.md](../../kilosettings/ci-workflows/wf-agent-onboarding.md)

### Goal
Novo agente (humano ou IA) produtivo em < 30 min.

### Sequence
```
1. Read Volume 0 (Executive Overview) — 5 min
2. Read Volume 14 (Quick Reference) — 5 min
3. Read Volume 1 (Mental Model) — 10 min
4. Practice: Load skill → Execute simple task — 10 min
   ├── `skill find-skills`
   ├── `skill writing-plans`
   └── Create mini plan
5. Read Volume 3/4 (Handbook do seu papel) — as needed
```

### Success Criteria
- [ ] Consegue invocar skill corretamente
- [ ] Entende agent/skill/MCP/command/ADR
- [ ] Executa tarefa simples end-to-end

---

## Workflow 6: skill-development

**Utilitário canônico:** [wf-skill-development.md](../../kilosettings/ci-workflows/wf-skill-development.md)

### Goal
Criar nova skill seguindo padrões da stack.

### Sequence
```
1. Discovery (skill-creator skill)
   ├── Problema resolvido?
   ├── Granularidade ideal?
   ├── Reutilização existente?

2. Draft SKILL.md
   ├── Frontmatter: name, description, version, author, maturity
   ├── Mission, Scope, Non-Scope
   ├── Inputs, Outputs
   ├── Internal Reasoning Pattern
   ├── Success Criteria, Failure Modes, Recovery
   ├── Typical Consumers, Synergies
   ├── Anti-Patterns, Cost Profile
   └── Example Sessions

3. Templates/Scripts (se aplicável)
   ├── templates/ na skill dir
   ├── checklists/ para validação

4. Test
   ├── Load skill em nova sessão
   ├── Execute example sessions
   ├── Verify success criteria

5. Register
   ├── Add to skills registry (se global)
   ├── Update related_skills em skills sinérgicas
   └── Document em Volume 3
```

---

## Workflow 7: mcp-integration

**Utilitário canônico:** [wf-mcp-integration.md](../../kilosettings/ci-workflows/wf-mcp-integration.md)

### Goal
Adicionar novo MCP à stack global.

### Preconditions
- [ ] MCP server testado localmente
- [ ] Auth/credentials disponíveis
- [ ] Schema das tools conhecido

### Sequence
```
1. Add to ~/.config/kilo/kilo.json
   ├── type: local
   ├── command + args
   ├── environment vars (secrets)
   └── enabled: true

2. Configure permissions
   ├── mcp_*: "ask" (default)
   ├── safe_tools: "allow"
   └── dangerous_tools: "deny"

3. Test
   ├── Invoke tool via agent
   ├── Verify auth works
   ├── Verify error handling
   └── Document em Volume 6

4. Document
   ├── Purpose, Auth, Permissions
   ├── Latency expectations
   ├── Failure modes, retry policies
   ├── Caching recommendations
   ├── Security risks
   ├── Operational costs
   ├── Best practices
   └── Common misuses
```

---

## Workflow 8: documentation-sync

**Utilitário canônico:** [wf-documentation-sync.md](../../kilosettings/ci-workflows/wf-documentation-sync.md) · **Fluxo CI:** [ci-doc-reconciliation.md](../../kilosettings/ci-workflows/ci-doc-reconciliation.md)

### Goal
Manter docs sincronizados com código automaticamente.

### Triggers
- Post-task (sempre)
- CI/CD pipeline
- Pre-merge
- Scheduled (daily)

### Sequence
```
1. documentation-reconciliation skill
   ├── Scan: README, CHANGELOG, ADRs, API docs
   ├── Compare: code vs docs
   ├── Report: drift severity (critical/warn/info)
   └── Auto-fix: trivial (links, formatting)

2. If critical drift:
   ├── Block merge (CI fail)
   ├── Assign to Docs Specialist
   └── Require fix before merge

3. Metrics
   ├── Drift rate (per week)
   ├── Time to fix
   └── Coverage (% files with docs)
```

---

## Quick Reference: All Workflows

| Workflow | Trigger | Duration | Key Skills | Utilitário |
|---|---|---|---|---|
| `implement-adr` | ADR Accepted | 2-8h | implementation, writing-plans, testing | [wf-implement-adr.md](../../kilosettings/ci-workflows/wf-implement-adr.md) |
| `new-feature` | Idea/Request | 4-24h | spec-writing, adr-generator, implementation | [wf-new-feature.md](../../kilosettings/ci-workflows/wf-new-feature.md) |
| `refactor-legacy` | Tech debt | 1-5d | reverse-engineering-specs, refactoring, tdd | [wf-refactor-legacy.md](../../kilosettings/ci-workflows/wf-refactor-legacy.md) |
| `incident-response` | Alert | 15min-4h | systematic-debugging | [wf-incident-response.md](../../kilosettings/ci-workflows/wf-incident-response.md) |
| `agent-onboarding` | New agent | 30min | — | [wf-agent-onboarding.md](../../kilosettings/ci-workflows/wf-agent-onboarding.md) |
| `skill-development` | Need new skill | 2-8h | skill-creator, writing-skills | [wf-skill-development.md](../../kilosettings/ci-workflows/wf-skill-development.md) |
| `mcp-integration` | New MCP needed | 30-60min | — | [wf-mcp-integration.md](../../kilosettings/ci-workflows/wf-mcp-integration.md) |
| `documentation-sync` | Continuous | Auto | documentation-reconciliation | [wf-documentation-sync.md](../../kilosettings/ci-workflows/wf-documentation-sync.md) |

### Fluxos CI (pipeline actions reais)

| Fluxo CI | Trigger | Pipeline Action |
|---|---|---|
| `validation-gates` | commit / PR | [ci-validation-gates.yml](../../.github/workflows/ci-validation-gates.yml) |
| `doc-reconciliation` | CI / pre-merge / scheduled | [ci-doc-reconciliation.yml](../../.github/workflows/ci-doc-reconciliation.yml) |
| `pre-merge-gate` | PR aberto | [ci-pre-merge-gate.yml](../../.github/workflows/ci-pre-merge-gate.yml) |

> Fonte canônica de cada fluxo: `docs/kilosettings/ci-workflows/` (veja [INDEX.md](../../kilosettings/ci-workflows/INDEX.md)).