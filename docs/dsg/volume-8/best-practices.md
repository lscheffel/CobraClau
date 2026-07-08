# Volume 8 — Best Practices

> **Objetivo:** Práticas recomendadas organizadas por categoria — aplicáveis a toda a stack.

---

## BP-001: Capability Contract

**Toda capability (skill, agent, command, MCP, workflow) deve responder:**

| Pergunta | Onde Documentar |
|---|---|
| O que faz? | `Mission` / `description` |
| Quando usar? | `Scope` / `Quando usar` |
| Quando **não** usar? | `Non-Scope` / `Quando não usar` |
| Qual alternativa existe? | `Synergies` / `Alternativas` |

**Exemplo (Skill):**
```markdown
## Mission
Cria planos de implementação estruturados a partir de specs/ADRs.

## Scope
- Transição ADR→execução
- Breakdown de features
- Roadmap técnico

## Non-Scope
- Execução do plano (use `implementation`)
- Criação da ADR (use `adr-generator`)

## Alternativas
- `executing-plans` — para execução de plano já aprovado
- `planning` — para planejamento estratégico alto nível
```

---

## BP-002: Explicit Agent Boundaries

**Todo agente deve possuir limites explícitos no prompt e permissions.**

```yaml
# Exemplo: architect
permission:
  read: allow
  edit:
    ".kilo/plans/*.md": allow
    "*": deny        # NÃO edita código
  bash: deny         # NÃO roda comandos
  mcp: deny          # NÃO usa MCPs
```

**Regra:** Se agente faz >3 tipos de tarefa → divida em agentes especializados.

---

## BP-003: Skill Anti-Patterns Required

**Toda skill deve documentar anti-patterns na seção `Anti-Patterns`.**

```markdown
## Anti-Patterns
| Anti-pattern | Sintoma | Prevenção |
|---|---|---|
| Skill shopping | Carrega 10 skills para tarefa simples | `find-skills` → 1-2 skills max |
| Implicit skill use | "O agente sabe" sem invocar | Sempre `skill` tool explícita |
| Wrong tool for job | `writing-plans` para debug | Mental Model → escolha correta |
```

---

## BP-004: Workflow Rollback Required

**Todo workflow deve possuir procedimento de rollback documentado.**

```markdown
## Rollback Procedures
```bash
# Se task falha irrecuperavelmente:
git reset --hard HEAD~1
# Se multiple tasks:
git reset --hard <commit-before-first-task>
# Sempre gere:
rollback-report.md
```
```

---

## BP-005: External Integration Degradation

**Toda integração externa (MCP) deve ter estratégia de degradação graciosa.**

```python
try:
    result = await mcp_tool.call(args)
except MCPUnavailable:
    result = fallback()  # cache, manual, skip
except AuthError:
    result = None        # continua sem MCP
except RateLimit:
    await sleep(backoff)
    result = await mcp_tool.call(args)  # retry once
```

**Nunca** derrube a sessão por falha de MCP.

---

## BP-006: Promote Emergent Behavior

**Todo comportamento emergente recorrente deve virar documentação explícita.**

| Emergente | Promovido Para |
|---|---|
| "Sempre fazemos X antes de Y" | Workflow step / Skill pattern |
| "Agente A sempre chama B" | Agent delegation strategy |
| "Erro Z acontece sempre" | Troubleshooting entry / Anti-pattern |
| "Convenção não escrita" | ADR / Best Practice / Rule |

---

## BP-007: Zero Tribal Knowledge

**Nenhum conhecimento operacional crítico pode permanecer exclusivamente implícito.**

| Implícito | Explícito (Obrigatório) |
|---|---|
| "Pergunte ao X" | `AGENTS.md` + DSG |
| "Sempre fizemos assim" | ADR com Decision |
| "O agente sabe" | Skill com Failure Modes |
| "Config padrão" | `kilo.json` versionado |
| "Workflow conhecido" | Workflow Cookbook |

---

## Architecture Best Practices

| Prática | Descrição |
|---|---|
| **ADR First** | Nenhuma implementação sem ADR aprovado |
| **Blueprint Before Code** | Tasks + deps + rollback antes de escrever |
| **Strangler Fig** | Legacy migration: new side-by-side, gradual cutover |
| **Branch by Abstraction** | Interface comum, swap implementation |
| **Bounded Contexts** | Skills/agents com responsabilidade única |
| **Dependency Inversion** | Skills dependem de abstrações, não concretos |

---

## Refactoring Best Practices

| Prática | Descrição |
|---|---|
| **Tests First** | Characterization tests antes de qualquer mudança |
| **Small Steps** | Um refactor por commit, validate cada step |
| **Parallel Run** | Old + new simultâneos até cutover |
| **Feature Flags** | Toggle new behavior sem deploy |
| **Rollback Ready** | `git reset --hard` sempre possível |

---

## Prompting Best Practices

| Prática | Descrição |
|---|---|
| **Explicit Role** | "You are a [role] who [does X]" |
| **Constraints First** | "Do NOT [X]. Only [Y]." |
| **Output Contract** | "End with: 1. What changed 2. How verified 3. Unhandled" |
| **Template Variables** | Use `$1`, `$ARGUMENTS`, `@file`, `` !`cmd` `` |
| **Few-Shot** | 2-3 examples no prompt para patterns complexos |

---

## Context Management Best Practices

| Prática | Descrição |
|---|---|
| **Minimal Context** | Load apenas o necessário (breadth > depth para planning) |
| **Preserve Across Sessions** | `kilo_local_recall` + AGENTS.md + skills |
| **Subagent Handoff** | Kilo extrai dados → passa no prompt (subagents não herdam MCP) |
| **Task Progress** | `task-progress.md` per task para continuidade |
| **Execution Contract** | Snapshot de pré-condições antes de executar |

---

## Token Economy Best Practices

| Prática | Descrição |
|---|---|
| **Right Model** | `temperature: 0.4` para code, `0.5` para planning, `1.0` para brainstorming |
| **Subtask for Long Ops** | `subtask: true` isola contexto caro |
| **Batch Reads** | `read_multiple_files` > multiple `read` |
| **Search Before Read** | `grep`/`glob` → target files only |
| **Compact Early** | `/compact` quando contexto > 70% |

---

## Review Best Practices

| Camada | Tool/Skill | Quando |
|---|---|---|
| 1. Self-validation | Build + lint + test | Every task |
| 2. Spec compliance | `acceptance-testing` | Feature completion |
| 3. Code quality | `code-review` skill | Pre-commit/merge |
| 4. Architecture | `architecture-review-kilo` | Structural changes |
| 5. Security | `security-review` | Auth/crypto/deps |
| 6. Documentation | `documentation-reconciliation` | CI / post-task |
| 7. Human approval | `question` tool / PR | High risk / architectural |

---

## Testing Best Practices

| Prática | Descrição |
|---|---|
| **RED-GREEN-REFACTOR** | Strict TDD (`test-driven-development` skill) |
| **Pyramid** | Unit > Integration > E2E (70/20/10) |
| **Edge Cases First** | Happy path por último |
| **Clear Assertions** | `expect(x).toBe(y)` não `expect(x).toBeTruthy()` |
| **Deterministic** | No flaky tests; fix or delete |
| **Coverage Threshold** | Definido em `testing-strategy`; CI fail se abaixo |

---

## Governance Best Practices

| Prática | Descrição |
|---|---|
| **Conventional Commits** | `feat:`, `fix:`, `refactor:`, `docs:`, `chore:` |
| **Semantic Versioning** | MAJOR.MINOR.PATCH + changelog |
| **Branch Strategy** | `main` protected; feature branches; PR required |
| **ADR Lifecycle** | Proposed → Accepted → Superseded (nunca delete) |
| **Execution Contract** | Obrigatório antes de qualquer implementação |

---

## Security Best Practices

| Prática | Descrição |
|---|---|
| **Least Privilege** | Agent permissions mínimas necessárias |
| **MCP Permissions** | `{server}_{tool}` patterns; deny dangerous |
| **Secrets Management** | Env vars no `kilo.json`; nunca hardcode |
| **Token Rotation** | PATs/OAuth tokens rotacionados periodicamente |
| **Audit Dependencies** | `security-review` skill em CI |

---

## MCP Usage Best Practices

| Prática | Descrição |
|---|---|
| **Right MCP** | Selection Guide (Vol 6) |
| **Batch Operations** | `read_multiple_files`, `push_files` |
| **Cache Results** | Evite re-reads; use memory MCP |
| **Degrade Gracefully** | BP-005 sempre |
| **Monitor Latency** | Log slow calls; alert se > threshold |

---

## Human-in-the-Loop Best Practices

| Prática | Descrição |
|---|---|
| **Question Tool** | Para decisões ambiguas: uma pergunta + recommended answer |
| **Escalation Conditions** | Definidas por agente no prompt |
| **Approval Gates** | Arquitetura, segurança, breaking changes |
| **Postmortem** | Após incidentes: root cause + action items |

---

## Quick Reference: Best Practice Checklist

```
☐ Capability Contract (BP-001) documentado
☐ Agent Boundaries (BP-002) explícitos
☐ Skill Anti-Patterns (BP-003) listados
☐ Workflow Rollback (BP-004) definido
☐ MCP Degradation (BP-005) implementado
☐ Emergent Behavior (BP-006) promovido
☐ Zero Tribal Knowledge (BP-007) enforced
☐ ADR First para mudanças arquiteturais
☐ Tests First para refactors
☐ Execution Contract válido antes de implementar
☐ Validation Gates passando em cada task
☐ Documentation sincronizada com código
☐ Least privilege em permissions
☐ Secrets em env vars, não hardcoded
```