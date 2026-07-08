# Volume 12 — FAQ

> **Objetivo:** Capturar conhecimento recorrente e implícito — respostas diretas para perguntas frequentes.

---

## Getting Started

### Q: Como começo a usar a stack?
**A:** Leia Volume 0 (Executive Overview) → Volume 14 (Quick Reference) → pratique com `skill find-skills` + `skill writing-plans`.

### Q: Preciso ler todos os 14 volumes?
**A:** Não. Use Volume 14 para consulta rápida. Leia volumes específicos quando precisar (ex: Volume 3 para skills, Volume 6 para MCPs).

### Q: Qual a diferença entre Kilo e KiloCode?
**A:** Kilo = produto comercial (kilo.ai). KiloCode = fork open-source. Config global em `~/.config/kilo/` (Kilo) vs `~/.kilocode/` (legacy KiloCode). Use `~/.config/kilo/`.

### Q: Como sei quais skills estão disponíveis?
**A:** `skill find-skills "sua necessidade"` ou veja Volume 3 (Skills Handbook) com 97 skills categorizadas.

---

## Agents

### Q: Qual agente uso para [tarefa]?
**A:** Consulte Volume 4 (Agent Handbook) → "Quick Decision Matrix":
- Planejar/Architecture → `architect`
- Code review → `code-reviewer`
- Documentar → `docs-specialist`
- React/TS/UI → `frontend-specialist`
- Testes/QA → `test-engineer`
- Implementação geral → `code` (padrão)

### Q: Posso fazer um agent fazer tudo?
**A:** Não. BP-002: Specialization over Generalism. Agents têm `Forbidden Domains` e permissões restritas. Ex: Architect NÃO edita código (`edit: "*": deny`).

### Q: Como crio um agent customizado?
**A:** Arquivo `.md` em `.kilo/agent/` ou `~/.config/kilo/agent/` com frontmatter (mode, model, permissions, prompt). Veja `agent-development` skill.

### Q: Subagents herdam MCPs do parent?
**A:** **Não.** Kilo (main session) acessa MCPs → extrai dados → passa no prompt do subagent. Subagent usa tools nativas + skills.

---

## Skills

### Q: Como descubro qual skill usar?
**A:** `skill find-skills "descreva o que precisa"` → retorna skills relevantes com Mission/Scope.

### Q: Posso usar múltiplas skills na mesma task?
**A:** Sim, mas **máximo 2-3**. Carregar 10 skills = context explosion + confusion. Use `find-skills` para escolher a melhor.

### Q: Skill não tem template que preciso?
**A:** Skills podem ter `templates/` dir. Se não tem, crie PR na skill ou use `skill-creator` para criar nova skill.

### Q: Como sei se skill está deprecated?
**A:** `skill-audit-bulletin` skill audita quality/completeness/actionability/risk. Verifica `maturity` no frontmatter: `stable` | `beta` | `experimental` | `deprecated`.

### Q: Skill global vs project?
**A:** Global: `~/.config/kilo/skills/` (carrega sempre). Project: `.kilo/skills/` (override/specific). Remote: `skills.urls` no `kilo.json` (ex: GitHub raw).

---

## MCPs

### Q: MCP não funciona — o que checar?
**A:** Checklist Volume 10 (Troubleshooting → MCP):
1. `enabled: true` no `kilo.json`?
2. Permission permite? (`permission: { "mcp_tool": "allow" }`)
3. Auth válida? (env vars no `kilo.json`)
4. Server roda? (teste tool simples)

### Q: Qual MCP uso para [necessidade]?
**A:** Selection Guide (Volume 6):
- Files locais → `filesystem`
- Browser automation → `playwright` (moderno) ou `puppeteer` (launch options)
- Library docs → `context7`
- GitHub → `github`
- Git local avançado → `git`
- Notion → `notion`
- Knowledge graph → `memory`
- Google Drive/Sheets/Gmail → `google-workspace`

### Q: MCP caiu — a sessão quebra?
**A:** Não, se implementou BP-005 (Degradação Graciosa). Try/catch + fallback (cache, manual, skip). Agent continua sem o MCP.

### Q: Como configuro OAuth Google Workspace?
**A:** 
1. `workspace-mcp` via uvx
2. Credentials em `~/.google_workspace_mcp/credentials/email.json`
3. Refresh token do VSCode `adc.json`
4. `GOOGLE_OAUTH_CLIENT_ID/SECRET` no `kilo.json`

---

## Commands

### Q: Comando customizado não aparece?
**A:** Verifique localização (precedência):
1. `~/.config/kilo/command/` ou `commands/`
2. `~/.kilo/command/` (legacy)
3. `~/.kilocode/command/` (legacy)
4. `.kilo/command/` (project)
Project override global com mesmo nome.

### Q: Como passo arquivo para comando?
**A:** `@file` no prompt expande conteúdo. Ex: `/meu-comando @src/main.ts`

### Q: Comando roda em subtask?
**A:** Frontmatter `subtask: true` isola contexto. Use para ops longas/carás.

---

## Workflows & Governance

### Q: Preciso de ADR para toda feature?
**A:** Se impacta arquitetura/NFRs/tech selection → **Sim** (`adr-generator`). Se tática/local → `writing-plans` direto.

### Q: O que é Execution Contract?
**A:** Validação pré-implantação (Workflow 2). Checa: ADR Accepted, Blueprint+TODO existem, branch/workspace limpos, acceptance criteria, rollback criteria. **Obrigatório** antes de implementar.

### Q: Posso pular validation gate se "sei que passa"?
**A:** **Não.** Hard rule: Task NÃO é "Concluída" sem Build+Lint+Typecheck+Tests+Arch+Doc Sync passarem. `verification-before-completion` skill enforce isso.

### Q: Como faço rollback?
**A:** `git reset --hard <commit-before-task>` + `rollback-report.md` (motivo, tasks revertidas, ações corretivas). Blueprint deve definir rollback procedure.

### Q: ADR status "Proposed" vs "Accepted"?
**A:** Proposed = em discussão/review. Accepted = aprovado para implementação. **Só implementa se Accepted.** (Exceto prototipação explícita).

---

## Troubleshooting

### Q: Agent alucina API/file que não existe
**A:** `verification-before-completion` skill (5-step HARD-GATE). Sempre cite arquivo:linha. `systematic-debugging` para reproduzir.

### Q: Context window cheio (>80%)
**A:** `/compact` (preserva decisões) ou `/new` (reload AGENTS.md + skills). Proativo: `/compact` a cada 2-3 tasks.

### Q: Tokens caros demais
**A:** Token Economy (Volume 8):
- Right model: Code=0.4 temp, Plan=0.5, Brainstorm=1.0
- `/compact` proativo
- `read_multiple_files` vs múltiplos `read`
- Subtask para ops longas
- Cache MCP results em `memory` MCP

### Q: Drift docs vs código
**A:** `documentation-reconciliation` skill em CI (fail on drift). Fix no MESMO commit da code change. Workflow 6 (Documentation Sync) obrigatório pós-task.

---

## Meta

### Q: Como reporto bug na stack?
**A:** Crie issue no repo da skill/agent/command ou abra PR com fix. Para stack core: ADR propondo mudança.

### Q: Como sugiro nova skill?
**A:** `skill-creator` skill guia criação. Ou `find-skills` → se não existe → crie.

### Q: Onde fica o changelog da stack?
**A:** `changelog-generator` skill gera de git commits. Ou veja git log em `~/.config/kilo/` e `.kilo/`.

### Q: Como atualizo skills globais?
**A:** Skills em `~/.config/kilo/skills/` são cópias locais. Atualize puxando do source (GitHub raw URL no `skills.urls`). `skill-audit-bulletin` detecta outdated.