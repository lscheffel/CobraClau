# Volume 6 — MCP Handbook

> **Objetivo:** Referência operacional completa dos 9 MCPs configurados na stack global.

---

## Visão Geral dos MCPs

| MCP | Tipo | Servidor | Status | Uso Principal |
|---|---|---|---|---|
| `filesystem` | local | `@modelcontextprotocol/server-filesystem` | ✅ | File access (read/write/list) |
| `playwright` | local | `@playwright/mcp@0.0.38` | ✅ | Browser automation |
| `puppeteer` | local | `@modelcontextprotocol/server-puppeteer` | ✅ | Browser automation |
| `context7` | local | `@upstash/context7-mcp` | ✅ | Library docs API |
| `github` | local | `@modelcontextprotocol/server-github` | ✅ | GitHub API (repos, issues, PRs) |
| `git` | local | `mcp-server-git` (uvx) | ✅ | Advanced Git operations |
| `notion` | local | `@notionhq/notion-mcp-server` | ✅ | Notion API (pages, databases) |
| `memory` | local | `@modelcontextprotocol/server-memory` | ✅ | Persistent knowledge graph |
| `google-workspace` | local | `workspace-mcp` (uvx) | ✅ | Google Drive, Docs, Sheets, Gmail |

---

## Permissões MCP (kilo.json)

```json
"permission": {
  "github_*": "ask",
  "github_get_file_contents": "allow",
  "github_delete_file": "deny",
  "notion_*": "ask",
  "notion_API-post-page": "allow",
  "google-workspace_*": "ask",
  "google-workspace_search_drive_files": "allow",
  "google-workspace_read_sheet_values": "allow"
}
```

**Regra:** Permissões MCP usam padrão `{server}_{tool}`. Broad patterns primeiro, specific overrides depois.

---

## filesystem

### Purpose
Acesso completo ao filesystem local (restrito a `~/` por config).

### Tools Principais
| Tool | Descrição |
|---|---|
| `read_text_file` | Ler arquivo (com head/tail) |
| `write_file` | Criar/sobrescrever arquivo |
| `edit_file` | Edits line-based (git-style diff) |
| `list_directory` | Listar diretório |
| `directory_tree` | Tree view recursivo |
| `search_files` | Glob pattern search |
| `get_file_info` | Metadata (size, dates, perms) |
| `create_directory` | Criar diretórios nested |
| `move_file` | Move/rename |
| `read_multiple_files` | Batch read |

### Configuração
```json
"filesystem": {
  "type": "local",
  "command": ["npx", "-y", "@modelcontextprotocol/server-filesystem", "~/"]
}
```

### Best Practices
- Use `directory_tree` para visão geral antes de `search_files`
- `read_multiple_files` para comparar/analisar múltiplos arquivos
- `edit_file` com `dryRun: true` para preview antes de aplicar

### Failure Modes
| Falha | Causa | Recuperação |
|---|---|---|
| Permission denied | Path fora de `~/` | Use path relativo dentro de allowed dirs |
| File not found | Path incorreto | `search_files` primeiro |
| Large file | > 2000 lines | Use `head`/`tail` ou `offset`/`limit` |

---

## playwright

### Purpose
Browser automation com Playwright — screenshots, snapshots, interaction, network.

### Tools Principais
| Tool | Descrição |
|---|---|
| `browser_navigate` | Navegar para URL |
| `browser_snapshot` | Accessibility snapshot (melhor que screenshot) |
| `browser_click` | Click em elemento |
| `browser_fill_form` | Preencher formulários multi-campo |
| `browser_type` | Digitar texto |
| `browser_wait_for` | Wait for text/condition |
| `browser_take_screenshot` | Screenshot (viewport ou full-page) |
| `browser_network_requests` | Network requests log |
| `browser_evaluate` | Execute JS no page/element |
| `browser_tabs` | List/create/close/select tabs |

### Configuração
```json
"playwright": {
  "type": "local",
  "command": ["npx", "-y", "@playwright/mcp@0.0.38"]
}
```

### Best Practices
- **Prefira `browser_snapshot`** sobre `browser_take_screenshot` para ações
- Use `browser_wait_for` com `text` ou `textGone` antes de interagir
- `browser_network_requests` com `static: false` para API calls apenas

### Failure Modes
| Falha | Causa | Recuperação |
|---|---|---|
| Browser not installed | Playwright não instalado | Rode `playwright_browser_install` |
| Element not found | Seletor incorreto | Use `browser_snapshot` para achar target correto |
| Timeout | Page não carrega | Aumente timeout ou check network |

---

## puppeteer

### Purpose
Browser automation alternativa com Puppeteer — screenshots, evaluate, navigate.

### Tools Principais
| Tool | Descrição |
|---|---|
| `puppeteer_navigate` | Navegar (com launchOptions opcional) |
| `puppeteer_screenshot` | Screenshot (element ou viewport) |
| `puppeteer_click` | Click via CSS selector |
| `puppeteer_fill` | Fill input via CSS selector |
| `puppeteer_evaluate` | Execute JS no console |
| `puppeteer_hover` | Hover element |
| `puppeteer_select` | Select option |
| `puppeteer_press_key` | Press key (não exposto, use playwright) |

### Configuração
```json
"puppeteer": {
  "type": "local",
  "command": ["npx", "-y", "@modelcontextprotocol/server-puppeteer"]
}
```

### Quando Usar vs Playwright
| Cenário | Recomendado |
|---|---|
| Precisa `launchOptions` customizados | Puppeteer |
| Accessibility snapshot | Playwright |
| Network analysis | Playwright |
| Simple screenshot/click | Qualquer |

---

## context7

### Purpose
Documentação atualizada de bibliotecas/frameworks via Context7 API.

### Tools Principais
| Tool | Descrição |
|---|---|
| `resolve-library-id` | Resolve nome → Context7 library ID |
| `query-docs` | Query docs/examples para library ID |

### Configuração
```json
"context7": {
  "type": "local",
  "command": ["npx", "-y", "@upstash/context7-mcp"],
  "environment": {
    "CONTEXT7_API_KEY": "ctx7sk-..."
  }
}
```

### Workflow Padrão
```
1. resolve-library-id("Next.js", "routing in Next.js")
   → retorna "/vercel/next.js"
2. query-docs("/vercel/next.js", "App Router layout.tsx")
   → retorna docs + code snippets
```

### Best Practices
- **Sempre** use `resolve-library-id` primeiro (não assuma ID)
- Query específico: "Como configurar X" não "X"
- Use para: setup, code gen, API reference, version-specific

### Failure Modes
| Falha | Causa | Recuperação |
|---|---|---|
| Library not found | Nome incorreto | Tente variações: "Next.js", "nextjs", "vercel/next.js" |
| Rate limited | Muitas queries | Cache local; aguarde |
| No snippets | Library sem exemplos | Use docs oficiais como fallback |

---

## github

### Purpose
GitHub API — repos, issues, PRs, files, reviews, actions.

### Tools Principais
| Tool | Descrição |
|---|---|
| `search_repositories` | Buscar repos |
| `get_file_contents` | Ler arquivo/branch |
| `create_pull_request` | Criar PR |
| `list_pull_requests` | Listar PRs (filtros) |
| `get_pull_request` | PR details |
| `get_pull_request_files` | Files changed |
| `get_pull_request_reviews` | Reviews |
| `create_pull_request_review` | Submit review |
| `create_issue` / `list_issues` / `get_issue` | Issues |
| `merge_pull_request` | Merge (squash/merge/rebase) |
| `push_files` | Push múltiplos arquivos |
| `create_branch` | Criar branch |
| `update_pull_request_branch` | Update branch from base |

### Configuração
```json
"github": {
  "type": "local",
  "command": ["npx", "-y", "@modelcontextprotocol/server-github"],
  "environment": {
    "GITHUB_PERSONAL_ACCESS_TOKEN": "github_pat_..."
  }
}
```

### Permissions (Recomendado)
```json
"github_*": "ask",
"github_get_file_contents": "allow",
"github_delete_file": "deny",
"github_create_pull_request": "ask",
"github_merge_pull_request": "ask"
```

### Best Practices
- Use `get_file_contents` com `branch` para ler de branch específica
- `push_files` para atomic multi-file commits
- `list_pull_requests` com `state: "open"`, `base: "main"` para review queue

### Failure Modes
| Falha | Causa | Recuperação |
|---|---|---|
| 401 Unauthorized | PAT expirado/inválido | Regenerar PAT com scopes corretos |
| 403 Forbidden | Sem permissão no repo | Verificar org/repo access |
| Rate limit | Muitas requests | `gh auth status` → check limit; aguarde reset |

---

## git

### Purpose
Operações Git avançadas via `mcp-server-git` (uvx).

### Tools Principais
| Tool | Descrição |
|---|---|
| `git_status` | Working tree status |
| `git_diff` / `git_diff_staged` / `git_diff_unstaged` | Diffs |
| `git_log` | Commit log (filtros: since, until, max_count) |
| `git_show` | Commit contents |
| `git_branch` | List branches (local/remote/all) |
| `git_create_branch` | Nova branch |
| `git_checkout` | Switch branch |
| `git_add` | Stage files |
| `git_commit` | Commit staged |
| `git_reset` | Unstage all |
| `git_search` | Search commits |

### Configuração
```json
"git": {
  "type": "local",
  "command": ["uvx", "mcp-server-git", "--repository", "/home/loupan/projetosVS/CobraClau"]
}
```

### Best Practices
- Sempre `git_status` antes de operações
- `git_log` com `max_count` para limitar output
- Use `git_diff_staged` para revisar antes de commit

### Failure Modes
| Falha | Causa | Recuperação |
|---|---|---|
| Not a git repo | Path errado | Verifique `--repository` path |
| Merge conflict | Unmerged files | Resolva manualmente ou `git_reset` |
| Detached HEAD | Checkout commit | `git_create_branch` para salvar |

---

## notion

### Purpose
Notion API — pages, databases, blocks, comments, search, markdown.

### Tools Principais
| Tool | Descrição |
|---|---|
| `API-post-search` | Search pages/databases |
| `API-query-data-source` | Query database (filters, sorts) |
| `API-post-page` | Create page |
| `API-retrieve-a-page` | Get page |
| `API-patch-page` | Update page properties |
| `API-retrieve-page-markdown` | Page as Markdown |
| `API-update-page-markdown` | Update page content (replace/update/insert) |
| `API-get-block-children` | Block children |
| `API-patch-block-children` | Append blocks |
| `API-retrieve-a-database` | Database schema |
| `API-create-a-data-source` | Create database |
| `API-get-users` / `API-get-user` | Users |

### Configuração
```json
"notion": {
  "type": "local",
  "command": ["npx", "-y", "@notionhq/notion-mcp-server", "--headers", "{\"Notion-Version\": \"2025-09-03\"}"],
  "environment": {
    "NOTION_TOKEN": "ntn_..."
  }
}
```

**Importante:** Env var é `NOTION_TOKEN` (não `NOTION_API_KEY`).

### Best Practices
- `API-retrieve-page-markdown` para ler; `API-update-page-markdown` com `type: "replace_content"` para escrever
- `API-query-data-source` com `filter` para queries complexas
- Conecte páginas/databases na integration settings (Notion → Settings → Integrations)

### Failure Modes
| Falha | Causa | Recuperação |
|---|---|---|
| 401 | Token inválido | Regenerar `ntn_...` token |
| 403 | Integration não tem access | Share page/database com integration |
| 404 | Page não encontrada | Check page_id; deve ser UUID |
| 409 Conflict | Row limit / concurrent edit | Retry; `allow_deleting_content: true` se needed |

---

## memory

### Purpose
Persistent knowledge graph — entities, relations, observations.

### Tools Principais
| Tool | Descrição |
|---|---|
| `memory_create_entities` | Criar entities (name, type, observations) |
| `memory_create_relations` | Criar relations (from, to, relationType) |
| `memory_add_observations` | Adicionar observations a entity |
| `memory_search_nodes` | Search por query |
| `memory_open_nodes` | Abrir nodes por names |
| `memory_read_graph` | Read entire graph |
| `memory_delete_entities` | Delete entities |
| `memory_delete_observations` | Delete observations |
| `memory_delete_relations` | Delete relations |

### Configuração
```json
"memory": {
  "type": "local",
  "command": ["npx", "-y", "@modelcontextprotocol/server-memory"]
}
```

### Data Model
```
Entity: { name, entityType, observations[] }
Relation: { from, to, relationType }  // active voice
Observation: string fact about entity
```

### Best Practices
- Use para: project context, decisions, patterns, tribal knowledge
- `entityType`: "decision", "pattern", "component", "person", "convention"
- `relationType`: "depends_on", "implements", "documents", "owns", "uses"
- Search antes de criar para evitar duplicatas

### Failure Modes
| Falha | Causa | Recuperação |
|---|---|---|
| Entity exists | Duplicate name | `memory_open_nodes` primeiro |
| Graph too large | Muitas entities | Search específico; não `read_graph` completo |
| Relation not found | Type/names errados | Case-sensitive; exact match |

---

## google-workspace

### Purpose
Google Drive, Docs, Sheets, Gmail, Calendar via `workspace-mcp` (uvx).

### Tools Principais
| Tool | Descrição |
|---|---|
| `search_drive_files` | Buscar arquivos no Drive |
| `get_drive_file_content` | Ler conteúdo (PDF, Doc, Sheet) |
| `read_sheet_values` | Ler planilha (range A1 notation) |
| `modify_sheet_values` | Atualizar células |
| `create_spreadsheet` | Nova planilha |
| `create_drive_folder` | Criar pasta |
| `send_gmail_message` | Enviar email |
| `list_gmail_messages` | Listar emails |
| `create_calendar_event` | Criar evento |

### Configuração
```json
"google-workspace": {
  "type": "local",
  "command": [
    "uvx", "--from", "workspace-mcp==1.4.7",
    "--hash", "sha256:4e98810f...",
    "workspace-mcp", "--tool-tier", "core"
  ],
  "environment": {
    "GOOGLE_OAUTH_CLIENT_ID": "973695904059-...apps.googleusercontent.com",
    "GOOGLE_OAUTH_CLIENT_SECRET": "GOCSPX-...",
    "OAUTHLIB_INSECURE_TRANSPORT": "1"
  }
}
```

### OAuth Token Location
```
~/.google_workspace_mcp/credentials/lscheffel@gmail.com.json
```
Gerado a partir de refresh_token no VSCode `adc.json`.

### Best Practices
- `search_drive_files` com query específica (`name contains "ficha"`)
- `read_sheet_values` com range exato (`Sheet1!A1:Z100`)
- Use para: ler fichas, boletos, planilhas de controle, enviar notificações

### Failure Modes
| Falha | Causa | Recuperação |
|---|---|---|
| Auth failed | Token expirado | Re-autenticar OAuth; check `credentials/` |
| File not found | Query muito ampla | Refine `search_drive_files` query |
| Rate limit | Muitas requests | Batch operations; aguarde |
| Permission denied | File não shared com service account | Share com integration email |

---

## MCP Selection Guide

| Necessidade | MCP |
|---|---|
| Ler/escrever arquivos locais | `filesystem` |
| Browser automation (moderno) | `playwright` |
| Browser automation (launch options) | `puppeteer` |
| Docs de library/framework | `context7` |
| GitHub ops (PR, issues, files) | `github` |
| Git local avançado | `git` |
| Notion pages/databases | `notion` |
| Knowledge graph persistente | `memory` |
| Google Drive/Sheets/Gmail | `google-workspace` |

---

## Degradação Graciosa (Todos MCPs)

```python
# Padrão recomendado no agente
try:
    result = await mcp_tool.call(args)
except MCPUnavailable:
    # Fallback: cache local, manual, ou skip
    log.warning(f"MCP {server} unavailable, using fallback")
    result = fallback()
except AuthError:
    log.error(f"Auth failed for {server}")
    result = None  # Agent continua sem esse MCP
except RateLimit:
    await sleep(backoff)
    result = await mcp_tool.call(args)  # Retry once
```

**Regra:** Falha de MCP **nunca** derruba a sessão. Agent continua com ferramentas disponíveis.