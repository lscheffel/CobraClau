# Volume 11 — Case Studies

> **Objetivo:** Exemplos reais de uso da stack — contexto, objetivo, estratégia, agentes, skills, erros, correções, resultado.

---

## Case Study 1: Implementing ADR-0142 (Definitive Stack Guide)

### Contexto
Stack cresceu para 97 skills, 5 agents, 9 MCPs. Onboarding novo agente levava horas; conhecimento tribal predominava.

### Objetivo
Criar DSG completo (Volumes 0-14) em uma sessão, demonstrando stack capabilities.

### Estratégia
```
1. writing-plans → Breakdown em 14 volumes + tasks
2. implementation → Execution Contract + DAG
3. Parallel execution: Vol 3 (skills) + Vol 4 (agents) + Vol 6 (MCPs)
4. Sequential: Vol 0,1,2,5,7,8,9,10,11,12,13,14
5. documentation-reconciliation → Sync check
```

### Agentes Envolvidos
| Agente | Papel |
|---|---|
| Architect (planning) | writing-plans, task-decomposition |
| Code (execution) | implementation, writing-plans |
| Docs Specialist | documentation, tech-docs-generator |

### Skills Utilizadas
`writing-plans`, `implementation`, `documentation`, `tech-docs-generator`, `documentation-reconciliation`, `find-skills`

### Erros
1. **Big Bang attempt:** Tentou criar todos volumes de uma vez → context explosion
2. **Missing validation:** Primeiro volume sem `verification-before-completion`
3. **Skill shopping:** Carregou 8 skills para planning phase

### Correções
1. `/compact` + reiniciou com `implementation` skill (1 task por vez)
2. Adicionou `verification-before-completion` a cada volume
3. `find-skills` → apenas `writing-plans` + `task-decomposition`

### Resultado
- **14 volumes** criados em ~3 horas
- **Zero validation failures** após correções
- **100% doc coverage** (documentation-reconciliation pass)
- **Template established** para futuros ADRs

---

## Case Study 2: MCP Google Workspace Integration

### Contexto
Precisava ler fichas de locação no Google Drive, planilhas de controle no Sheets, enviar notificações Gmail.

### Objetivo
Configurar `google-workspace` MCP com OAuth, testar tools, documentar no DSG.

### Estratégia
```
1. mcp-builder → Entender workspace-mcp schema
2. Configure kilo.json com OAuth credentials
3. Test each tool: search_drive_files, read_sheet_values, send_gmail_message
4. Document em Volume 6 (MCP Handbook)
5. Create fallback para quando MCP down
```

### Agentes Envolvidos
| Agente | Papel |
|---|---|
| Code | Configuração + testing |
| Architect | Degradação graciosa design |

### Skills Utilizadas
`mcp-builder`, `context7-mcp` (para workspace-mcp docs), `implementation`

### Erros
1. **Wrong env var:** Usou `NOTION_API_KEY` pattern → `NOTION_TOKEN` era correto para Notion; para Google é `GOOGLE_OAUTH_CLIENT_ID/SECRET`
2. **Token location:** Procurou em `~/.config/` → token em `~/.google_workspace_mcp/credentials/`
3. **No fallback:** Primeira versão travava se MCP down

### Correções
1. Lê docs do `@notionhq/notion-mcp-server` → confirmou `NOTION_TOKEN`
2. `search_drive_files` encontrou `adc.json` no VSCode extension storage
3. Implementou `try/catch` com fallback manual (BP-005)

### Resultado
- **MCP functional** com 7 tools testadas
- **Fallback documented** no Volume 6
- **OAuth flow** automatizado para renovação

---

## Case Study 3: Legacy Refactor (CobraClau Billing Engine)

### Contexto
Motor de cálculo de cobrança com 2000+ linhas, zero testes, lógica de negócio misturada com I/O.

### Objetivo
Refatorar para: testável, determinístico, auditável (ADR-COB-001).

### Estratégia
```
1. reverse-engineering-specs → Behavioral specs (implementation-free)
2. test-driven-development → Characterization tests (capture current behavior)
3. refactoring → Strangler Fig: nova impl lado a lado
4. implementation → ADR-COB-001 execution
```

### Agentes Envolvidos
| Agente | Papel |
|---|---|
| Architect | ADR-COB-001 + Blueprint + TODO |
| Code | Characterization tests + Strangler Fig migration |
| Test Engineer | Coverage targets + property-based tests |

### Skills Utilizadas
`reverse-engineering-specs`, `test-driven-development`, `refactoring`, `implementation`, `architecture-review-kilo`

### Erros
1. **Skipped characterization tests:** Assumiu "entendo o código" → bugs sutis
2. **Big Bang migration:** Tentou swap completo → rollback 3x
3. **No rollback procedure:** Blueprint não tinha rollback steps

### Correções
1. `test-driven-development` skill: RED-GREEN-REFACTOR obrigatório
2. `refactoring` skill: Strangler Fig com feature flag
3. `implementation` skill: Blueprint agora exige rollback procedure

### Resultado
- **Cobertura:** 0% → 94% (characterization + new tests)
- **Determinismo:** Todos valores monetários de função pura
- **Auditoria:** Execution Report + git history rastreável
- **Tempo:** 2 semanas (vs 2 dias estimado otimista)

---

## Case Study 4: Multi-Agent Feature Development (Auth System)

### Contexto
Nova feature: Authentication com JWT, refresh tokens, RBAC.

### Objetivo
Demonstrar `agent-orchestration` + `dispatching-parallel-agents` workflow.

### Estratégia
```
Architect → ADR-AUTH-001 + Blueprint (6 tasks)
    │
    ├─→ Task 1: DB Schema (Code agent + database-schema-design skill)
    ├─→ Task 2: JWT Library (Code agent + security-review skill)  
    ├─→ Task 3: API Endpoints (Code agent + api-design skill)
    ├─→ Task 4: Frontend Login (Frontend Specialist + frontend-design skill)
    ├─→ Task 5: E2E Tests (Test Engineer + webapp-testing skill)
    └─→ Task 6: Docs (Docs Specialist + tech-docs-generator skill)
    
Parallel: Tasks 1,2,3 independent → dispatching-parallel-agents
Sequential: 4 depends on 3; 5 depends on 1-4; 6 depends on 5
```

### Agentes Envolvidos
| Agente | Tasks |
|---|---|
| Architect | Planning only |
| Code (3x parallel) | Tasks 1,2,3 |
| Frontend Specialist | Task 4 |
| Test Engineer | Task 5 |
| Docs Specialist | Task 6 |

### Skills Utilizadas
`agent-orchestration`, `dispatching-parallel-agents`, `database-schema-design`, `security-review`, `api-design`, `frontend-design`, `webapp-testing`, `tech-docs-generator`

### Erros
1. **Parallel too early:** Task 3 (API) dependia de Task 1 (Schema) — não identificada no Blueprint
2. **Context sharing:** Subagents não tinham schema context → API contracts mismatch
3. **Validation gate missing:** Task 3 "Concluída" sem contract test

### Correções
1. `writing-plans` melhorado: dependency analysis obrigatório
2. Kilo passa schema context explicitamente no prompt do subagent
3. `acceptance-testing` skill: contract test gate para API tasks

### Resultado
- **Parallel efficiency:** 3 tasks em 40% do tempo sequencial
- **Zero integration bugs** após corrections
- **Template** para future multi-agent features

---

## Case Study 5: Incident Response (Production API Timeout)

### Contexto
API endpoint `/billing/calculate` timeout >30s em 15% requests.

### Objetivo
Diagnosticar, mitigar, resolver, postmortem.

### Estratégia (incident-response workflow)
```
1. Detect: Alert → systematic-debugging skill
2. Hypothesize: DB query? External MCP? Memory leak?
3. Test: Minimal repro → pg_stat_statements → query plan
4. Root cause: Missing index + N+1 query em loop
5. Mitigate: Scale read replica + query hint
6. Fix: Add index + batch query (refactoring skill)
7. Verify: Load test + monitoring
8. Postmortem: Blameless + action items
```

### Agentes Envolvidos
| Agente | Papel |
|---|---|
| Code | Debug + fix |
| Test Engineer | Load test + regression |
| Architect | Schema review + index strategy |

### Skills Utilizadas
`systematic-debugging`, `performance-optimization`, `refactoring`, `webapp-testing`, `observability`

### Erros
1. **Assumed DB:** Primeiro checkou application code → lost 30min
2. **No minimal repro:** Tentou fixar sem isolar → fix não resolveu
3. **Skipped load test:** "Fix óbvio" → regressão em outro endpoint

### Correções
1. `systematic-debugging` skill: Phase 1 = Reproduce (obrigatório)
2. Minimal repro script obrigatório antes de qualquer fix
3. `webapp-testing` skill: load test gate para perf fixes

### Resultado
- **MTTR:** 45min (detect → mitigate)
- **Root cause fixed:** Index + batch query
- **Regression:** 0 (load test gate)
- **Action items:** 3 (monitoring, query review process, index policy)

---

## Case Study Template

```markdown
# Case Study: [Título]

## Contexto
[Situação inicial, constraints, stack state]

## Objetivo
[O que se queria alcançar, métricas de sucesso]

## Estratégia
[Workflow usado, phases, decision points]

## Agentes Envolvidos
| Agente | Papel | Tasks |

## Skills Utilizadas
[Lista skills + por que cada uma]

## Erros
| # | Erro | Causa | Impacto |

## Correções
| # | Correção | Prevenção Futura |

## Resultado
- Métricas quantitativas
- Qualitativo
- Templates/processos criados

## Lições para DSG
[O que este case ensina sobre uso da stack]
```