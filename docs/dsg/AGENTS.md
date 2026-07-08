# AGENTS.md — DSG Onboarding for Agents

> **Objetivo:** Agente recém-instanciado → utilização eficiente da stack em < 5 min.

---

## Protocolo de Inicialização (Obrigatório)

Ao instanciar, execute **em ordem**:

```bash
# 1. Carregue o guia de referência rápida (30 seg)
skill find-skills "quick reference"  # ou leia volume-14/quick-reference.md

# 2. Entenda o modelo mental (2 min)
skill find-skills "mental model"     # ou leia volume-1/mental-model.md

# 3. Identifique seu papel (30 seg)
skill find-skills "agent handbook"   # ou leia volume-4/agent-handbook.md

# 4. Pratique: skill discovery (1 min)
skill find-skills "sua necessidade"  # retorna skills relevantes
```

---

## Navegação Essencial

| Preciso... | Vá para |
|---|---|
| "O que é X?" | Vol 13 (Glossary) |
| "Como faço Y?" | Vol 14 (Quick Reference) → Vol 3/4/6 |
| "Qual skill/agent/MCP?" | Vol 14 (Decision Matrix) |
| "Erro Z — o que faço?" | Vol 10 (Troubleshooting) |
| "Anti-pattern comum?" | Vol 9 (Encyclopedia) |
| "Exemplo real?" | Vol 11 (Case Studies) |

---

## Regras de Ouro (Non-Negotiable)

1. **SEMPRE** `skill find-skills` antes de assumir capability
2. **SEMPRE** invoque `skill: nome` explicitamente (nunca implícito)
3. **SEMPRE** cite arquivo:linha para claims técnicos
4. **NUNCA** marque task "Concluída" sem validation gates (Vol 7)
4. **NUNCA** assuma MCP disponível — implemente degradação graciosa (Vol 6)
5. **SE** perguntado 2x → documente AGORA (BP-007)

---

## Meu Papel (Auto-Identificação)

Leia **Vol 4 (Agent Handbook)** → encontre seu agent ID → internalize:
- `Decision Authority` — o que você PODE decidir
- `Forbidden Domains` — o que você NÃO toca
- `Escalation Conditions` — quando pedir ajuda
- `Delegation Strategy` — como passar tasks

---

## Fluxo de Trabalho Padrão

```
RECEIVE TASK
    │
    ▼
skill find-skills "task description"  ← 1-2 skills max
    │
    ▼
skill: nome_da_skill                   ← invocação explícita
    │
    ▼
FOLLOW Internal Reasoning Pattern      ← do SKILL.md
    │
    ▼
EXECUTE → VALIDATE (all gates) → DOCUMENT
    │
    ▼
IF blocked: 3 retries → mark Bloqueado → escalate
```

---

## Validação Obrigatória (Por Task)

```
✅ Build pass
✅ Lint pass
✅ Typecheck pass
✅ Tests pass (unit + integration)
✅ Architectural validation (vs ADR)
✅ Documentation sync verified
```

**Falhou qualquer um → Task NÃO é "Concluída"**

---

## Context Management

- `/compact` a cada 2-3 tasks (preserva decisões)
- `/new` se contexto > 80% ou corrupted
- Subagents: Kilo passa dados extraídos (não herda MCP)

---

## Emergency Procedures

| Situação | Ação |
|---|---|
| Session corrupted | `/new` → reload AGENTS.md + Vol 0 + Vol 14 |
| All MCPs down | Disable in kilo.json → native tools only |
| Validation gates failing | Check local → CI → architecture-review |
| Knowledge graph corrupt | `memory_read_graph` → backup → fix |

---

## Referência Rápida de Arquivos

```
DSG Root:     docs/dsg/
Quick Ref:    volume-14/quick-reference.md
Glossary:     volume-13/glossary.md
Mental Model: volume-1/mental-model.md
My Agent:     volume-4/agent-handbook.md
My Skills:    volume-3/skills-handbook.md
My MCPs:      volume-6/mcp-handbook.md
Workflows:    volume-7/workflow-cookbook.md
Troubleshoot: volume-10/troubleshooting-manual.md
FAQ:          volume-12/faq.md
```

---

## Checklist de Pronto

Antes de aceitar primeira task, confirme:

- [ ] Li Vol 14 (Quick Reference)
- [ ] Li Vol 1 (Mental Model) — capítulos relevantes
- [ ] Li Vol 4 (Agent Handbook) — meu perfil
- [ ] Pratiquei] [ ] Test: `skill find-skills "test"` → funciona
- [ ] Entendo: validation gates, degradação graciosa, authority boundaries
- [ ] Sei: onde estão glossary, FAQ, troubleshooting

---

**Status:** Pronto para operação eficiente.  
**Próximo:** Aguardar task → seguir fluxo padrão acima.