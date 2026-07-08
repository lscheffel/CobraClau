---
id: wf-mcp-integration
title: Workflow — MCP Integration
type: workflow
version: 1.0.0
status: active
trigger: "Novo MCP necessário na stack global"
participants: []
related_skills: []
related_bp: [ADR-0142.1-BP]
source: docs/dsg/volume-7/workflow-cookbook.md#workflow-7-mcp-integration
---

# Workflow: mcp-integration

## Goal
Adicionar novo MCP à stack global com segurança e documentação.

## Preconditions
- [ ] MCP server testado localmente
- [ ] Auth/credentials disponíveis
- [ ] Schema das tools conhecido

## Inputs
- MCP server (command/args), credenciais, schema

## Sequence
1. **Add to `~/.config/kilo/kilo.json`** — `type: local`; `command` + `args`; `environment` vars (secrets); `enabled: true`.
2. **Configure permissions** — `mcp_*: "ask"` (default); `safe_tools: "allow"`; `dangerous_tools: "deny"`.
3. **Test** — invocar tool via agent; verificar auth; verificar error handling; documentar no Volume 6 (MCP Handbook).
4. **Document** — Purpose, Auth, Permissions, Latency, Failure Modes, Retry Policies, Caching, Security Risks, Operational Costs, Best Practices, Common Misuses.

## Validation Gates
| Gate | Check |
|------|-------|
| Config | kilo.json válido, server sobe |
| Auth | Tool invocável e autenticado |
| Docs | Volume 6 atualizado com todos os campos |

## Rollback
- Remover bloco do MCP em `kilo.json`; revogar credenciais se necessário; reverter Volume 6.

## Exit Criteria
- [ ] MCP funcional e autenticado
- [ ] Permissões configuradas por perigo
- [ ] Documentado no Volume 6

## Metrics
- Latência observada vs esperada
- Taxa de erro de auth

## Source Reference
DSG Vol 7, Workflow 7 — docs/dsg/volume-7/workflow-cookbook.md
