# Volume 10 — Troubleshooting Manual

> **Objetivo:** Diagnóstico e resolução rápidos para problemas comuns por categoria.

---

## Categorias

1. [Context](#context)
2. [Memory](#memory)
3. [MCP](#mcp)
4. [Orchestration](#orchestration)
5. [Hallucination](#hallucination)
6. [Governance](#governance)
7. [Performance](#performance)
8. [Cost](#cost)

---

## Context

### Sintoma: "Agent não tem contexto necessário"

| Verificação | Ação |
|---|---|
| AGENTS.md carregado? | `/new` → reload automático |
| Skills invocadas? | `skill find-skills` → carrega skill |
| MCP acessível? | Teste tool simples (ex: `filesystem: list_directory`) |
| Subagent recebeu dados? | Kilo deve extrair + passar no prompt |

**Solução Rápida:**
```
/compact → resume essentials
OU
/new → fresh session com AGENTS.md + skills
```

### Sintoma: "Context window cheio (>80%)"

| Causa | Fix |
|---|---|
| Muitos `read`/`glob` | Use `grep`/`search` primeiro |
| Subagents herdando contexto | Kilo passa apenas dados extraídos |
| Histórico longo | `/compact` a cada 2-3 tasks |
| Skills carregadas desnecessárias | `find-skills` → 1-2 skills max |

**Comandos:**
```
/compact          # Summarize preservando decisões
/summarize        # Mais agressivo
/context          # Ver usage atual
```

### Sintoma: "Contexto inconsistente entre agents"

| Causa | Fix |
|---|---|
| Handoff sem `task-progress.md` | Sempre preencher ao delegar |
| Agent anterior não documentou decisão | `Execution Contract` captura pré-condições |
| Subagent não recebe constraints | Prompt completo com `Forbidden Domains` |

---

## Memory

### Sintoma: "Knowledge graph vazio / não persiste"

| Verificação | Ação |
|---|---|
| `memory_read_graph` retorna vazio? | Normal na primeira execução |
| Entities não criadas? | `memory_create_entities` com observations |
| Relações não criadas? | `memory_create_relations` active voice |

**Exemplo Correto:**
```json
// Entity
{ "name": "ADR-0142", "entityType": "decision", "observations": ["DSG implementation", "Status: Accepted"] }
// Relation  
{ "from": "ADR-0142", "to": "Volume 3", "relationType": "documents" }
```

### Sintoma: "Busca não encontra entities conhecidas"

| Causa | Fix |
|---|---|
| Case-sensitive | Use exact name: "ADR-0142" não "adr-0142" |
| Observations não indexadas | Adicione keywords nas observations |
| Graph muito grande | `memory_open_nodes` com names específicos |

### Sintoma: "Duplicatas no knowledge graph"

| Detecção | Fix |
|---|---|
| `memory_search_nodes` retorna múltiplos | `memory_open_nodes` → merge observations → `memory_delete_entities` duplicados |
| Nomes similares | Padronize naming convention |

---

## MCP

### Sintoma: "MCP tool não aparece / não invoca"

| Verificação | Ação |
|---|---|
| MCP enabled no `kilo.json`? | `"enabled": true` |
| Permission permite? | `permission: { "mcp_tool": "allow" }` |
| Server rodando? | Teste tool simples |
| Auth válida? | Check env vars no `kilo.json` |

### Sintoma: "MCP retorna erro de auth"

| MCP | Credencial | Verificação |
|---|---|---|
| GitHub | `GITHUB_PERSONAL_ACCESS_TOKEN` | `gh auth status` |
| Notion | `NOTION_TOKEN` (não API_KEY) | Token `ntn_...` válido? |
| Google Workspace | OAuth token em `~/.google_workspace_mcp/` | Re-autenticar se expirado |
| Context7 | `CONTEXT7_API_KEY` | Key válida? |

### Sintoma: "MCP rate limited / lento"

| Estratégia | Implementação |
|---|---|
| Cache local | `memory` MCP para results frequentes |
| Batch operations | `read_multiple_files`, `push_files` |
| Retry com backoff | Exponential backoff 1s, 2s, 4s |
| Degrade graciosamente | Fallback para manual/cache |

### Sintoma: "MCP tool schema mismatch"

| Causa | Fix |
|---|---|
| MCP version mudou | Re-check tool schema via `browser_snapshot` |
| Args incorretos | Follow exact JSON schema |
| Required fields missing | Check tool definition |

---

## Orchestration

### Sintoma: "Tasks não executam em paralelo quando deveriam"

| Verificação | Fix |
|---|---|
| DAG tem dependências? | `dispatching-parallel-agents` só para tasks independentes |
| `subtask: true` usado? | Subtasks são sequenciais por default |
| Agent não delegando? | Use `dispatching-parallel-agents` skill explicitamente |

### Sintoma: "Deadlock / circular dependency no DAG"

| Detecção | Fix |
|---|---|
| `implementation` skill reporta cycle | Fix TODO dependencies → remove cycle |
| Tasks travadas "Em andamento" | Check `task-progress.md` → resolve blocker |

### Sintoma: "Validation gate falha mas task marcada Concluída"

| Causa | Fix |
|---|---|
| `verification-before-completion` não invocado | Obrigatório antes de marcar done |
| Agent assumiu que "passa" | Hard rule: evidence required |
| CI passa local mas falha remoto | Run full CI local antes de push |

---

## Hallucination

### Sintoma: "Agent inventa APIs/schemas/files que não existem"

| Detecção | Fix |
|---|---|
| `grep` no codebase não encontra | Agent deve `read`/`grep` antes de assumir |
| Types não matcham | `read` actual file vs assumido |
| Function signatures erradas | `grep` function definition |

**Prevenção:**
- `verification-before-completion` skill: 5-step HARD-GATE
- `systematic-debugging` skill: reproduce → hypothesize → test
- Sempre citar arquivo:linha para claims

### Sintoma: "Agent confunde skills / usa pattern errado"

| Causa | Fix |
|---|---|
| Múltiples skills carregadas | `find-skills` → 1 skill por task |
| Não leu `Non-Scope` | Ler `Scope` + `Non-Scope` antes de invocar |
| Prompt drift | `/compact` periódico + reforçar constraints |

---

## Governance

### Sintoma: "ADR não encontrado / status inconsistente"

| Verificação | Fix |
|---|---|
| `docs/adr/ADR-XXX.md` existe? | `adr-generator` cria template |
| Status = "Accepted"? | Só implementa se Accepted |
| Blueprint + TODO existem? | `writing-plans` gera ambos |

### Sintoma: "Execution Contract falha"

| Campo | Validação |
|---|---|
| ADR existe | Path correto |
| ADR status | "Accepted" |
| Blueprint existe | Tem tasks |
| TODO existe | Tem tasks com estados |
| Branch | Não main/master sem PR |
| Workspace | Clean (no uncommitted) |
| Arquivos impactados | Existem no filesystem |

### Sintoma: "Drift docs vs código detectado"

| Tool | Ação |
|---|---|
| `documentation-reconciliation` | Audit completo |
| CI fail | Fix drift no mesmo commit |
| Links quebrados | Auto-fix ou manual |

---

## Performance

### Sintoma: "Agent lento / timeout"

| Causa | Fix |
|---|---|
| Context > 80% | `/compact` ou `/new` |
| MCP latency alta | Cache + batch + degrade |
| Model lento | Check `/models` → use faster model |
| Tool calls excessivos | Batch reads; search antes de read |

### Sintoma: "Token cost alto"

| Otimize"

| Métrica | Target | Ação se excede |
|---|---|---|
| Tokens/task | < 50k | `/compact`, fewer skills |
| Tokens/session | < 500k | `/new` session |
| MCP calls | < 20/task | Cache + batch |
| Subagent spawns | < 3/session | `dispatching-parallel-agents` |

---

## Cost

### Sintoma: "Custo de execução > orçamento"

| Otimização | Impacto |
|---|---|
| Right model per task | Code: 0.4 temp; Plan: 0.5; Brainstorm: 1.0 |
| Subtask para ops longas | Isola contexto caro |
| `read_multiple_files` | -70% vs multiple `read` |
| `/compact` proativo | -40% tokens/session |
| Cache MCP results | -90% repeat calls |

---

## Quick Diagnostic Flowchart

```
PROBLEMA
    │
    ├─ Context? → /compact → /new → check AGENTS.md + skills
    ├─ Memory? → memory_search_nodes → create entities if missing
    ├─ MCP? → check enabled + permission + auth + latency
    ├─ Orchestration? → check DAG + validation gates + delegation
    ├─ Hallucination? → verification-before-completion + cite sources
    ├─ Governance? → Execution Contract + ADR status + doc sync
    ├─ Performance? → context usage + model + MCP latency
    └─ Cost? → right model + subtask + batch + compact
```

---

## Emergency Procedures

### Session Corrompida (agent loop, nonsense output)
```
1. /new → fresh session
2. Reload: AGENTS.md + Volume 0 + Volume 14
3. Re-invoke skills necessárias
4. Continue from last known good task-progress.md
```

### MCP Total Failure (todos MCPs down)
```
1. Disable MCPs no kilo.json: "enabled": false
2. Continue com tools nativos (filesystem, bash, git)
3. Document manual workarounds
4. Re-enable quando MCP restaurado
```

### Validation Gates Consistente Falhando
```
1. Check: build/lint/test passing locally?
2. Check: CI config matches local?
3. Check: test flakiness?
4. If systemic: architecture-review-kilo → fix root cause
```

### Knowledge Graph Corrompido
```
1. memory_read_graph → backup
2. memory_delete_entities duplicados
3. Re-create critical entities from ADRs/docs
4. Document recovery em Execution Report
```