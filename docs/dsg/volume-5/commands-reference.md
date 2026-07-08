# Volume 5 — Commands Reference

> **Objetivo:** Referência completa de comandos built-in do Kilo e padrão para comandos customizados.

---

## Comandos Built-in do Kilo (Slash Commands)

Estes comandos estão sempre disponíveis via `/comando` ou `Ctrl+P` Command Palette.

---

### Navegação & Sessão

| Comando | Atalho | Descrição |
|---|---|---|
| `/help` | — | Ajuda completa |
| `/status` | — | Status do sistema |
| `/exit` \| `/quit` \| `/q` | — | Sair |
| `/new` \| `/clear` | `<leader>n` | Nova sessão |
| `/sessions` | `<leader>l` | Listar sessões |
| `/share` | — | Compartilhar sessão |
| `/rename` | `ctrl+r` | Renomear sessão |
| `/timeline` | `<leader>g` | Jump to message |
| `/fork` | — | Fork from message |
| `/compact` \| `/summarize` | `<leader>c` | Compactar/summarizar |
| `/undo` | `<leader>u` | Desfazer mensagem |
| `/redo` | `<leader>r` | Refazer |
| `/copy` | `<leader>y` | Copiar última resposta |
| `/copy-session` | — | Copiar transcrição completa |

---

### Agente & Modelo

| Comando | Atalho | Descrição |
|---|---|---|
| `/agents` | `<leader>a` | Trocar agente |
| `/models` | `<leader>m` | Trocar modelo |
| `/mcps` | — | Toggle MCPs |
| `Tab` / `Shift+Tab` | — | Cycle agent |

---

### Aparência

| Comando | Atalho | Descrição |
|---|---|---|
| `/themes` | `<leader>t` | Trocar tema (35+ built-in) |
| — | Ctrl+P → "Toggle appearance" | Dark/Light mode |

---

### Display Toggles (via Ctrl+P)

| Toggle | Atalho | Descrição |
|---|---|---|
| Toggle animations | — | Animações |
| Toggle diff wrapping | — | Quebra de linha no diff |
| Toggle sidebar | `<leader>b` | Sidebar |
| Toggle thinking | `/thinking` | Mostrar reasoning |
| Toggle tool details | — | Detalhes de tools |
| Toggle timestamps | `/timestamps` | Timestamps |
| Toggle scrollbar | — | Scrollbar |
| Toggle header | — | Header |
| Toggle code concealment | `<leader>h` | Ocultar código |

---

### Sistema

| Comando | Descrição |
|---|---|
| `/editor` | Abrir editor |

---

## Comandos Customizados

### Localização (Ordem de Precedência)

```
1. ~/.config/kilo/command/          (Global)
2. ~/.config/kilo/commands/         (Global - plural)
3. ~/.kilo/command/                 (Legacy home)
4. ~/.kilocode/command/             (Legacy home)
5. .kilo/command/                   (Project)
6. .kilo/commands/                  (Project - plural)
7. .kilocode/command/               (Legacy project)
8. .kilocode/commands/              (Legacy project)
```

**Regra:** Project commands override global com mesmo nome.

---

### Formato do Arquivo

Arquivo: `{nome}.md` em qualquer diretório `command/` ou `commands/`

```markdown
---
description: Run tests and fix failures    # Opcional, shown in command list
agent: code                                 # Opcional, route to specific agent
model: anthropic/claude-sonnet              # Opcional, override model
subtask: true                               # Opcional, run as subtask
---
Run all tests in $1 and fix failures.
Use $ARGUMENTS for the full arg string.
Reference files with @file and shell output with !`cmd`.
```

---

### Template Variables

| Variável | Descrição | Exemplo |
|---|---|---|
| `$1` .. `$N` | Positional arguments | `/test-fix src/auth` → `$1` = `src/auth` |
| `$ARGUMENTS` | Full argument string | `/cmd arg1 arg2` → `arg1 arg2` |
| `@file` | File contents | `@src/main.ts` → conteúdo do arquivo |
| `` !`cmd` `` | Shell output | `` !`git status` `` → output do comando |

---

### Exemplos de Comandos Úteis

#### `/test-fix` — Run tests and fix failures
```markdown
---
description: Run tests in path and fix failures
agent: code
subtask: true
---
Run all tests in $1 and fix failures.
Use $ARGUMENTS for the full arg string.
Reference files with @file and shell output with !`cmd`.
```

#### `/adr-new` — Create new ADR
```markdown
---
description: Create new ADR with template
agent: architect
---
Create ADR for: $ARGUMENTS
Follow ADR template in docs/adr/
Include: Context, Decision, Consequences, Status
Save as ADR-XXX.md with next sequential number
```

#### `/skill-load` — Load and explain skill
```markdown
---
description: Load skill and explain usage
---
Load skill: $1
Explain: Mission, Scope, When to use, Anti-patterns, Example sessions
```

#### `/context-save` — Save current context
```markdown
---
description: Save session context for later resume
---
Save current context including:
- Active agents and skills
- Key decisions made
- Files modified
- Next steps
Output as markdown for resume prompt
```

#### `/review-prep` — Prepare for code review
```markdown
---
description: Prepare changes for review
agent: code-reviewer
---
Run pre-merge validation:
1. Build
2. Lint
3. Tests
4. Typecheck
5. Documentation sync
Report any failures with fix suggestions
```

---

## Criando Novos Comandos

### Passo a Passo

1. **Escolha escopo**: Global (`~/.config/kilo/command/`) ou Project (`.kilo/command/`)
2. **Crie arquivo**: `{kebab-case-name}.md`
3. **Adicione frontmatter** com `description` mínimo
4. **Escreva prompt** usando template variables
5. **Teste**: Digite `/nome` no chat

### Boas Práticas

| Prática | Exemplo |
|---|---|
| Nome verboso | `/create-adr` não `/adr` |
| Description clara | "Create ADR with template" |
| Agent apropriado | `architect` para planning, `code` para impl |
| Subtask para operações longas | `subtask: true` |
| Documentar variáveis | Comentários no prompt |

---

## Command Discovery

```bash
# Listar todos comandos disponíveis
ls ~/.config/kilo/command/ ~/.config/kilo/commands/ .kilo/command/ .kilo/commands/ 2>/dev/null

# Ver ajuda de comando específico
/nome-do-comando --help  # Se suportado
# Ou ler o arquivo .md diretamente
```

---

## Troubleshooting Commands

| Sintoma | Causa | Solução |
|---|---|---|
| Comando não aparece | Nome errado / diretório errado | Verifique `command/` vs `commands/` e kebab-case |
| Override não funciona | Project não tem precedência | Project `.kilo/command/` vence global |
| Variáveis não expandem | Sintaxe errada | Use `$1`, `$ARGUMENTS`, `@file`, `` !`cmd` `` |
| Agent não troca | Agent não existe | Verifique `agent: id` corresponde a agent em `agents/` |
| Model override falha | Model ID inválido | Use `provider/model` exato do `/models` |