# Volume 1 — Mental Model

> **Objetivo:** Construir modelo mental compartilhado entre humanos e agentes sobre os blocos fundamentais da stack.

---

## Capítulo 1 — O que é um Agente

### Definição

Um **agente** é uma persona operacional especializada que combina:

- **Modelo LLM** específico (ex: `kilo/nvidia/nemotron-3-ultra-550b-a55b:free`)
- **Permissões granulares** (read/edit/bash/mcp/skill por padrão glob)
- **Prompt de sistema** que define papel, restrições e contratos de saída
- **Modo de operação** (`primary` | `subagent` | `all`)
- **Contexto de herança** — carrega AGENTS.md, skills, MCPs do escopo global/projeto

### Responsabilidades

| Responsabilidade | Descrição |
|---|---|
| **Execução contratual** | Segue Execution Contract, valida a cada step |
| **Delegação explícita** | Usa `task` tool para subagentes com prompt detalhado |
| **Rastreabilidade** | Cita arquivos, linhas, decisões; não infere |
| **Verificação obrigatória** | Build/lint/test antes de marcar "Concluído" |
| **Documentação sincronizada** | Atualiza ADR/Blueprint/TODO/README após alterações |

### Autonomia

- **Alta** dentro do domínio de expertise definido no prompt
- **Zero** fora de `Forbidden Domains` (ex: architect não edita código)
- **Condicional** em `Escalation Conditions` (pede humano/architect)

### Limites

```yaml
# Exemplo: architect agent
permission:
  read: allow
  edit:
    ".kilo/plans/*.md": allow
    "*": deny
  bash: deny
  mcp: deny
```

### Ciclo de Vida

```
INSTANTIATED → CONTEXT_LOADED → TASK_RECEIVED → 
EXECUTION_LOOP (select → execute → validate → document) → 
TASK_COMPLETE → CONTEXT_PRESERVED
```

### Ownership

- **Humano** define: prompt, permissões, modelo, modo
- **Stack** provisiona: skills, MCPs, contexto global
- **Agente** executa: tarefas, validações, documentação
- **Governance** audita: compliance, drift, quality gates

---

## Capítulo 2 — O que é uma Skill

### Abstração Funcional

Uma **skill** é uma unidade de conhecimento operacional reutilizável, versionável e invocável que encapsula:

- **Problema resolvido** (Mission)
- **Quando usar / não usar** (Scope / Non-Scope)
- **Padrão de raciocínio** (Internal Reasoning Pattern)
- **Critérios de sucesso/falha** (Success Criteria / Failure Modes)
- **Recuperação** (Recovery Strategies)
- **Sinergias** com outras skills

### Granularidade Ideal

| Nível | Exemplo | Quando usar |
|---|---|---|
| **Atômica** | `git-commit-helper` | Uma operação específica |
| **Composta** | `writing-plans` | Workflow multi-step |
| **Meta** | `implementation` | Orquestra ADR→Blueprint→TODO→Execution |

**Regra:** Uma skill = um arquivo `SKILL.md` + templates/scripts opcionais.

### Escopo

- **Global** — `~/.config/kilo/skills/` (carregada sempre)
- **Projeto** — `.kilo/skills/` (override/specific)
- **Remota** — `skills.urls` no `kilo.json` (ex: GitHub raw)

### Reutilização

```markdown
# Frontmatter do SKILL.md
name: writing-plans
description: Cria planos de implementação estruturados
version: 1.2.0
author: lscheffel
maturity: stable
related_skills: [adr-generator, task-decomposition, planning]
```

### Acoplamento Aceitável

| Tipo | Aceitável? | Exemplo |
|---|---|---|
| **Composição** | ✅ | `implementation` usa `writing-plans` + `testing` + `git` |
| **Dependência de dado** | ✅ | `adr-generator` lê ADRs existentes |
| **Estado compartilhado** | ❌ | Duas skills escrevendo no mesmo arquivo sem coordenação |
| **Ordem implícita** | ❌ | Skill A "deve" rodar antes de B sem declarar dependência |

---

## Capítulo 3 — O que é um Workflow

### Coordenação

Um **workflow** é uma sequência declarativa de passos que orquestra:

- **Agents** (primários e subagentes)
- **Skills** (invocadas via `skill` tool)
- **MCPs** (ferramentas externas)
- **Decisão humana** (via `question` tool)
- **Validação** (gates automáticos)

### Orquestração

```
WORKFLOW: implement-adr
├── Phase 1: Artifact Resolution (skill: implementation)
├── Phase 2: Execution Contract (skill: implementation)  
├── Phase 3: Dependency Analysis (skill: implementation)
├── Phase 4: Incremental Execution (agent: code + skills)
│   ├── Task A1 → validate → document
│   ├── Task A2 → validate → document
│   └── Task A3 → validate → document
├── Phase 5: Documentation Sync (skill: documentation)
└── Phase 6: Execution Report (skill: implementation)
```

### Sincronização

- **Sequencial** por default (DAG topological sort)
- **Paralelo** quando tasks sem dependência mútua
- **Barreira** em `Validation Gates` (todos passam antes de prosseguir)

### Pontos de Decisão

| Ponto | Tipo | Ação se falhar |
|---|---|---|
| Execution Contract | Automático | Interrompe; pede correção de artefatos |
| Validation Gate | Automático | Bloqueia task; 3 tentativas → "Bloqueado" |
| Human Approval | `question` tool | Pausa; aguarda input |
| Rollback Trigger | Automático | Reverte commits; gera rollback-report |

### Rollback

Todo workflow **deve** definir:
- Quais commits reverter
- Como restaurar estado anterior (files, DB, MCPs)
- Template de `rollback-report.md` preenchido

---

## Capítulo 4 — O que é uma Regra

### Enforcement

Regras na stack operam em **camadas de precedência**:

```
1. Hard Constraints (código)     → Build falha se violado
2. Execution Contract            → Implementação não inicia
3. Validation Gates              → Task não completa
4. Agent Permissions             → Tool negada em runtime
5. Skill Success Criteria        → Skill reporta falha
6. Best Practices (docs)         → Warning / review
7. Conventions (tribal)          → Deprecated
```

### Precedência

| Conflito | Vence |
|---|---|
| Agent permission vs Skill suggestion | Agent permission |
| ADR decision vs Blueprint detail | ADR decision |
| MCP capability vs Skill assumption | MCP capability (real) |
| Hard constraint vs Best practice | Hard constraint |

### Conflito

Quando duas regras colidem:

1. **Identifique** a camada de cada regra
2. **Aplique** precedência (tabela acima)
3. **Documente** exceção no ADR/Blueprint se intencional
4. **Nunca** ignore silenciosamente

### Exceções

Exceções **só** existem se:
- Documentadas no ADR/Blueprint/TODO
- Têm critério de expiração (data ou condição)
- Passam por review (governance skill)
- São auditáveis (grep-able no código/docs)

---

## Capítulo 5 — O que é um MCP

### Papel

Um **MCP (Model Context Protocol)** server expõe ferramentas externas como functions chamáveis pelo agente:

```
Agent → MCP Client → MCP Server → External System
                ↓
         Tool Schema (JSON Schema)
                ↓
         Permission Check (kilo.json)
                ↓
         Execution → Result → Agent
```

### Fronteira

| Dentro do MCP | Fora do MCP |
|---|---|
| Auth, connection, protocol | Lógica de negócio do agente |
| Rate limiting, retry | Orquestração de múltiplos MCPs |
| Schema validation | Decisão de qual tool chamar |
| Error normalization | Interpretação do resultado |

### Segurança

```json
// kilo.json - mcp tool permissions
"permission": {
  "github_*": "ask",
  "github_get_file_contents": "allow",
  "github_delete_file": "deny"
}
```

**Regra:** Permissões MCP usam mesma sintaxe que tools nativas (`{server}_{tool}`).

### Autorização

| MCP | Auth Type | Credencial |
|---|---|---|
| GitHub | PAT | `GITHUB_PERSONAL_ACCESS_TOKEN` |
| Notion | Bearer | `NOTION_TOKEN` |
| Google Workspace | OAuth | `GOOGLE_OAUTH_CLIENT_ID/SECRET` |
| Context7 | API Key | `CONTEXT7_API_KEY` |
| Filesystem | Local | Nenhuma (path-restrito) |
| Memory | Local | Nenhuma |

### Degradação Graciosa

| Falha | Comportamento |
|---|---|
| MCP indisponível | Tool retorna erro; agente continua sem ela |
| Auth expirada | Agent reporta; usa fallback (cache, manual) |
| Rate limit | Retry com backoff; degrada para cached |
| Schema mismatch | Log estruturado; agente adapta prompt |

---

## Capítulo 6 — O que é um Comando

### Ergonomia

Um **comando** é um atalho invocável via `/nome` que encapsula:

- Prompt estruturado (com variáveis `$1..$N`, `$ARGUMENTS`, `@file`, `!`cmd``)
- Agente alvo opcional (`agent: code`)
- Modelo override opcional
- Modo subtask opcional

### Automação

```markdown
---
description: Run tests and fix failures
agent: code
model: anthropic/claude-sonnet
subtask: true
---
Run all tests in $1 and fix failures.
Use $ARGUMENTS for the full arg string.
Reference files with @file and shell output with !`cmd`.
```

### Aceleração Operacional

| Comando | Sem comando | Com comando |
|---|---|---|
| `/test-fix src/auth` | "Rode os testes em src/auth e corrija falhas" | 2 tokens |
| `/adr-new feature-x` | "Crie ADR para feature-x seguindo template" | 5 tokens |
| `/skill-load x` | "Carregue a skill x e explique quando usar" | 3 tokens |

### Descoberta

```
# Onde procurar (ordem de precedência)
1. ~/.config/kilo/command/
2. ~/.config/kilo/commands/
3. ~/.kilo/command/
4. ~/.kilocode/command/
5. .kilo/command/ (project)
6. .kilocode/command/ (project)
```

**Regra:** Project commands override global com mesmo nome.