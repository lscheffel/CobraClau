# Volume 9 — Anti-pattern Encyclopedia

> **Objetivo:** Catálogo de anti-patterns operacionais — sintoma, causa raiz, exemplo, consequências, detecção, prevenção, recuperação.

---

## Formato Padrão

```
## [Nome do Anti-pattern]

### Symptom
[O que você observa]

### Root Cause
[Por que acontece]

### Example
[Cenário real]

### Consequences
[Impacto se não corrigido]

### Detection
[Como identificar]

### Prevention
[Como evitar]

### Recovery
[Como corrigir se já ocorreu]
```

---

## Agent Anti-patterns

---

### Agent Overlap

**Symptom:** Múltiplos agentes trabalhando no mesmo arquivo/domínio simultaneamente; conflitos de merge; trabalho duplicado.

**Root Cause:** Delegation strategy não definida; `Forbidden Domains` não respeitados; handoff impreciso.

**Example:**
```
Architect cria plano → Code agent implementa
→ Test Engineer escreve testes no mesmo arquivo
→ Frontend Specialist ajusta componente
→ 3 PRs conflitando no mesmo componente
```

**Consequences:**
- Merge conflicts constantes
- Context explosion (cada agente carrega contexto do outro)
- Quality gates bypassados (cada um assume que o outro validou)

**Detection:**
- `git log --oneline --all -- <file>` mostra >2 autores recentes
- Task progress mostra múltiplos "Em andamento" no mesmo domínio

**Prevention:**
- Architect define `Delegation Strategy` explícita no plan
- Agents consultam `Forbidden Domains` antes de aceitar task
- `dispatching-parallel-agents` só para tasks verdadeiramente independentes

**Recovery:**
```
1. Identificar owner único por arquivo/domínio
2. Reverter commits conflitantes (git reset)
3. Re-atribuir tasks com owner claro
4. Documentar no plan: "Owner: [agent] para [files]"
```

---

### Responsibility Ambiguity

**Symptom:** "Achei que o outro agente ia fazer"; tasks caem no vazio; validações não acontecem.

**Root Cause:** `Decision Authority` não definida; `Escalation Conditions` ausentes; handoff sem contrato.

**Example:**
```
Code agent: "Test Engineer vai validar"
Test Engineer: "Code agent disse que testes passavam"
→ Ninguém rodou integration tests
```

**Consequences:**
- Silent failures em produção
- Blame culture entre agentes
- Validation gates ineficazes

**Detection:**
- Task "Concluída" sem validation gate executado
- Perguntas "Quem faz X?" frequentes no chat

**Prevention:**
- Todo plan: `Validation Gates` explicitos por task
- `Escalation Conditions` definidas no agent profile
- Handoff requer `task-progress.md` preenchido

**Recovery:**
```
1. Root cause: missing gate → adicionar ao plan
2. Re-run validation missing
3. Atualizar agent profile com authority clara
```

---

### Excessive Delegation

**Symptom:** Task principal delega para subagent → subagent delega → sub-subagent → context lost; latency alta; token cost explode.

**Root Cause:** Delegation sem critério; `subtask: true` usado por default; não usar `dispatching-parallel-agents` para fan-out.

**Example:**
```
Main task: "Refactor auth"
├── Subagent 1: "Research JWT libs"
│   └── Sub-subagent: "Compare benchmarks"
├── Subagent 2: "Design new schema"
│   └── Sub-subagent: "Write migration"
└── Subagent 3: "Update tests"
    └── Sub-subagent: "Fix flaky tests"
```

**Consequences:**
- Context fragmentation (cada level perde info)
- Token cost 3-5x maior
- Debugging impossível (qual level falhou?)

**Detection:**
- Task tree depth > 2
- Tokens/session > threshold
- "Qual subagent fez X?" perguntas frequentes

**Prevention:**
- Regra: Max 1 level de delegation (main → subagent)
- Fan-out: `dispatching-parallel-agents` para tasks independentes
- Fan-in: Main agent consolida, não sub-subagents

**Recovery:**
```
1. Consolidar context no main agent
2. Cancelar sub-subagents desnecessários
3. Re-executar como tasks paralelas diretas
```

---

### Context Explosion

**Symptom:** Agent carrega 50+ arquivos no contexto; resposta lenta; tokens esgotados; alucinações aumentam.

**Root Cause:** `read`/`glob` excessivos; não usar `grep`/`search` primeiro; não compactar (`/compact`); subagents herdam contexto pai.

**Example:**
```
Agent lê 30 files para "entender codebase"
→ Context 80% full
→ Próxima task: "Adicionar campo"
→ Agent alucina schema (não leu migration)
```

**Consequences:**
- Qualidade degrada exponencialmente após 70% context
- Alucinações em schemas, APIs, tipos
- `/compact` perde nuance crítica

**Detection:**
- Context usage > 70% (UI indicator)
- Respostas genéricas / "não sei sem olhar"
- Alucinações em detalhes técnicos

**Prevention:**
- Sempre `grep`/`glob` antes de `read`
- `read_multiple_files` para batch
- `/compact` proativo a cada 2-3 tasks
- Subagents: Kilo passa APENAS dados extraídos (não herda MCP/context)

**Recovery:**
```
/compact → resume essentials
OU
/new session → reload AGENTS.md + skills essenciais
```

---

## Skill Anti-patterns

---

### Skill Shopping

**Symptom:** Carrega 5-10 skills para tarefa que precisa de 1; prompt inchado; confusão de qual skill usar.

**Root Cause:** Não usar `find-skills` primeiro; assumir que "mais skills = melhor"; não ler `Non-Scope` das skills.

**Example:**
```
Task: "Debug failing test"
Skills loaded: testing, testing-strategy, test-driven-development, 
               acceptance-testing, systematic-debugging, verification-before-completion,
               code-review, clean-code, refactoring
→ Agent confuso sobre qual pattern seguir
```

**Consequences:**
- Prompt tokens desperdiçados
- Conflitos de pattern entre skills
- Agent indeciso / slow

**Detection:**
- `skill` tool invocado >3 vezes por task simples
- Skills com `Non-Scope` sobreposto carregadas juntas

**Prevention:**
```
1. find-skills "debug test"
2. Lê top 2 results
3. Carrega APENAS a mais relevante
4. Se precisa segunda: carrega explicitamente
```

**Recovery:**
```
/compact → remove skills não usadas
Re-invoke apenas skill necessária
```

---

### Implicit Skill Use

**Symptom:** Agent "sabe fazer X" sem invocar skill; output não segue pattern; validation gates falham.

**Root Cause:** Assumir que training data basta; não ler `Internal Reasoning Pattern` da skill.

**Example:**
```
User: "Crie ADR"
Agent: Escreve ADR sem `adr-generator` skill
→ Falta: Status, Consequences, Blueprint ref, TODO ref
→ ADR incompleto, não executável
```

**Consequences:**
- Artifacts fora do padrão
- Execution Contract falha (missing fields)
- Downstream tasks quebram

**Detection:**
- Output não segue template da skill
- Campos obrigatórios ausentes
- Validation gate falha "inesperadamente"

**Prevention:**
- **Regra:** SE skill existe para X, SEMPRE `skill: X` antes de fazer X
- `using-superpowers` / `using-toolkit` protocol

**Recovery:**
```
1. Invocar skill correta
2. Re-fazer task seguindo pattern
3. Validar com gates da skill
```

---

### Wrong Tool for Job

**Symptom:** Skill usada fora do `Scope`; resultado ruim; tempo desperdiçado.

**Root Cause:** Não ler `Scope`/`Non-Scope`; assumir funcionalidade por nome.

**Example:**
```
"Planeje esta feature" → `writing-plans` (correto)
"Planeje esta feature" → `planning` (strategic, não tactical)
"Planeje esta feature" → `roadmap-planning` (alto nível, não implementation)
```

**Consequences:**
- Output no nível errado (stratégico vs tático)
- Rework necessário
- Frustração do user

**Detection:**
- Skill `Non-Scope` menciona exatamente o que foi pedido
- Output não atende `Success Criteria` da skill

**Prevention:**
- Ler `Scope` + `Non-Scope` ANTES de invocar
- `find-skills` retorna múltiplas → ler cada `Mission`

---

## Workflow Anti-patterns

---

### Big Bang Implementation

**Symptom:** Todas as tasks implementadas de uma vez; single commit gigante; validação só no final.

**Root Cause:** Pressa; não seguir `implementation` skill Execution Loop; pular `Execution Contract`.

**Example:**
```
Plan: 10 tasks
Agent: Edita 50 arquivos → 1 commit → "Done"
→ Build falha → 3h debug → Rollback parcial → Rework
```

**Consequences:**
- Debugging exponencial
- Rollback complexo (o que reverter?)
- Validation gates ineficazes (late detection)

**Detection:**
- Commits com >10 arquivos modificados
- Tasks marcadas "Concluída" sem validation gate
- Branch sem commits intermediários

**Prevention:**
- `implementation` skill: 1 task → validate → commit → next
- `Execution Contract` obrigatório antes de iniciar
- `verification-before-completion` skill a cada task

**Recovery:**
```
1. git reset --hard <last-known-good>
2. Re-executar tasks uma a uma com gates
3. Documentar no Execution Report: "Big bang attempted, reverted"
```

---

### Skip Validation

**Symptom:** "Teste passou local, vou commitar"; "Lint warning não é erro"; "Doc depois".

**Root Cause:** Otimismo bias; pressure to deliver; não internalizou `Review over Trust`.

**Example:**
```
Task: "Add user endpoint"
Agent: Implements → git commit → push
→ CI fails: typecheck error + missing test
→ Rollback → Fix → Re-push
```

**Consequences:**
- CI/CD pipeline blocked
- Wasted compute + time
- False "Concluído" status

**Detection:**
- Tasks "Concluída" mas CI falha
- `verification-before-completion` não invocado

**Prevention:**
- **Hard rule:** Task NÃO é "Concluída" sem:
  - Build pass
  - Lint pass (zero warnings se configured)
  - Tests pass (unit + integration se exist)
  - Typecheck pass
  - Doc sync verified

**Recovery:**
```
1. Reverter commit
2. Rodar full validation local
3. Fix all issues
4. Re-commit + push
```

---

### Drift Documentation

**Symptom:** Code ≠ Docs; ADR diz X, implementação faz Y; README desatualizado.

**Root Cause:** `Documentation Sync` (Workflow 6) não executado; `documentation-reconciliation` não em CI; "doc depois".

**Example:**
```
ADR: "Use Redis for cache"
Code: Usa in-memory Map
README: "Cache: Redis"
→ New agent configura Redis → cache miss → bug
```

**Consequences:**
- Onboarding broken (docs mentem)
- Decisions baseadas em docs errados
- Audit fails

**Detection:**
- `documentation-reconciliation` skill reporta drift
- `grep -r "TODO.*doc"` no code
- ADR status ≠ implementation reality

**Prevention:**
- Doc sync no MESMO commit da code change
- `documentation-reconciliation` em CI (fail on drift)
- `Implementation` skill Workflow 6 obrigatório

**Recovery:**
```
1. Audit completo: code vs todos docs
2. Fix all drift em 1 commit
3. Add CI gate se não existe
```

---

## MCP Anti-patterns

---

### MCP Dependency Without Fallback

**Symptom:** Task falha completamente quando MCP indisponível; session crashes.

**Root Cause:** Não implementar degradação graciosa (BP-005); assumir disponibilidade 100%.

**Example:**
```
Task: "Search GitHub for similar issues"
GitHub MCP down → Agent: "Cannot complete task"
→ User blocked
```

**Consequences:**
- Single point of failure externo
- User experience quebrada
- No offline capability

**Detection:**
- Task failure rate correlaciona com MCP status
- "MCP unavailable" errors no log

**Prevention:**
- SEMPRE `try/catch` com fallback
- Cache local para reads frequentes
- Manual alternative documentada

**Recovery:**
```
1. Implement fallback no agent prompt
2. Test com MCP desligado
3. Document manual alternative
```

---

### Over-permissioned MCP

**Symptom:** MCP tool `delete`/`write` disponível para agent que só precisa `read`; acidente waiting to happen.

**Root Cause:** Permissões default amplas; não revisar `permission` no `kilo.json`.

**Example:**
```
GitHub MCP: `github_*`: "allow"
Agent: "Clean up old branches" → deleta main branch
```

**Consequences:**
- Data loss
- Security incident
- Audit failure

**Detection:**
- `permission` no kilo.json tem `"*"` para MCP
- Tools perigosas (`delete`, `create`, `update`) em `"allow"`

**Prevention:**
```json
"github_*": "ask",
"github_get_file_contents": "allow",
"github_delete_file": "deny",
"github_create_pull_request": "ask"
```

**Recovery:**
```
1. Audit permissions immediately
2. Restrict to minimum needed
3. Add approval gate for dangerous tools
```

---

## Governance Anti-patterns

---

### Authority Inversion

**Symptom:** Agent com menos expertise toma decisão arquitetural; Architect aprova sem review; Code agent define NFRs.

**Root Cause:** `Decision Authority` não respeitado; escalation não seguido; "move fast" culture.

**Example:**
```
Code agent: "Vou usar MongoDB para este feature"
Architect: "OK" (sem questionar)
→ 6 meses depois: consistency issues, no transactions
```

**Consequences:**
- Architectural debt
- Decisions não reversíveis
- Blame quando falha

**Detection:**
- Decisions fora do `Decision Authority` do agent
- ADR criado por agent sem authority
- "Surprise" architectural choices

**Prevention:**
- `Architect` agent: única authority para:
  - Technology selection
  - Service boundaries
  - Data model changes
  - NFR targets
- `Escalation Conditions` trigger Architect review

**Recovery:**
```
1. Criar ADR retroativo documentando decisão
2. Architect review + approve/reject
3. Se reject: plan migration (Strangler Fig)
```

---

### Hidden Coupling

**Symptom:** Mudança em A quebra B inesperadamente; tests passam mas behavior muda; "works on my machine".

**Root Cause:** Shared state não documentado; implicit contracts; no interface boundaries; skills/agents acoplados via side effects.

**Example:**
```
Skill A escreve em `.cache/temp.json`
Skill B lê `.cache/temp.json` (assumindo formato)
Skill A muda formato → Skill B quebra silenciosamente
```

**Consequences:**
- Fragile system
- Debugging nightmare
- Fear of change

**Detection:**
- File access patterns: múltiplos agents/skills same file
- `git log --oneline -- <file>` mostra >3 authors
- Integration tests falham mas unit passam

**Prevention:**
- Explicit contracts: interfaces, schemas, APIs
- `Non-Scope` documenta o que NÃO tocar
- Shared state → dedicated MCP/skill com API

**Recovery:**
```
1. Identify coupling point
2. Create explicit contract (interface/schema)
3. Migrate consumers to contract
4. Add integration test for contract
```

---

## Meta Anti-patterns

---

### Prompt Drift

**Symptom:** Agent behavior muda ao longo da sessão; instruções iniciais ignoradas; "esquece" constraints.

**Root Cause:** Context window pressure; não `/compact` periódico; subagents não recebem constraints; contradictory instructions acumulam.

**Example:**
```
Start: "You are Test Engineer. Only edit test files."
3 tasks later: Agent edita implementation file
→ "Mas você disse..."
```

**Consequences:**
- Forbidden domains violados
- Quality gates bypassados
- Trust erosion

**Detection:**
- Agent violates own prompt constraints
- "Anteriormente você disse X, agora Y"
- Permission denied errors frequentes

**Prevention:**
- `/compact` a cada 2-3 tasks
- Reinforce constraints no prompt de cada task
- Subagents: prompt completo com constraints

**Recovery:**
```
/compact → re-establish constraints
OU
/new session → reload AGENTS.md + skills
```

---

### Tribal Knowledge Persistence

**Symptom:** "Só o Luciano sabe"; "Pergunte no chat"; conhecimento crítico não documentado.

**Root Cause:** BP-007 violado; pressa; "documento depois" nunca acontece; assumir que "é óbvio".

**Example:**
```
New agent: "Como configuro OAuth Google?"
Team: "Pergunta pro Luciano"
→ Luciano indisponível → blocked
```

**Consequences:**
- Bus factor = 1
- Onboarding impossível
- Knowledge loss on churn

**Detection:**
- Perguntas recorrentes no chat
- "Não está documentado" frequente
- Onboarding > 4 horas

**Prevention:**
- **Regra:** Se perguntado 2x → documentar AGORA
- `documentation-reconciliation` flagga gaps
- DSG (este guia) = fonte canônica

**Recovery:**
```
1. Capture knowledge em DSG volume apropriado
2. Link no FAQ (Vol 12)
3. Announce: "Agora documentado em Vol X"
```

---

## Quick Reference: Anti-pattern → Skill Mapping

| Anti-pattern | Primary Skill | Secondary Skills |
|---|---|---|
| Agent Overlap | `agent-orchestration` | `dispatching-parallel-agents` |
| Responsibility Ambiguity | `implementation` | `writing-plans` |
| Excessive Delegation | `agent-orchestration` | `subagent-driven-development` |
| Context Explosion | `using-superpowers` | `verification-before-completion` |
| Skill Shopping | `find-skills` | `using-toolkit` |
| Implicit Skill Use | `using-superpowers` | — |
| Wrong Tool for Job | `find-skills` | — |
| Big Bang | `implementation` | `verification-before-completion` |
| Skip Validation | `verification-before-completion` | `acceptance-testing` |
| Drift Documentation | `documentation-reconciliation` | `documentation` |
| MCP No Fallback | `mcp-builder` | — |
| Over-permissioned MCP | `governance` | — |
| Authority Inversion | `agent-orchestration` | `architecture-review-kilo` |
| Hidden Coupling | `architecture-review-kilo` | `clean-code` |
| Prompt Drift | `using-superpowers` | `verification-before-completion` |
| Tribal Knowledge | `documentation` | `self-learning` |