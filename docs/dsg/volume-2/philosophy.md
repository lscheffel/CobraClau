# Volume 2 — Philosophy

> **Objetivo:** Explicar *por que* a stack é assim — princípios, tradeoffs e decisões de design.

---

## Design Principles

Cada princípio abaixo é uma **regra de decisão**, não apenas aspiração. Quando em dúvida, aplique o princípio.

---

### 1. Explicit over Implicit

> **Tudo que afeta comportamento deve ser declarável, legível e versionável.**

| Implícito (evitar) | Explícito (preferir) |
|---|---|
| "O agente sabe fazer isso" | Skill com `Mission`, `Scope`, `Success Criteria` |
| "Contexto carrega sozinho" | `instructions` glob patterns no `kilo.json` |
| "Skill carrega se necessária" | `skill` tool invocation no prompt/plan |
| "Permissão herdada" | `permission` object por agent/tool/MCP |
| "Workflow conhecido da equipe" | Blueprint com DAG + TODO + Gates |

**Teste:** `grep -r "assume" .` → deve retornar zero resultados em docs/código.

---

### 2. Governance over Improvisation

> **Processo vence heroísmo. Checklist vence intuição.**

```
Improvisação          → Governance
├─ "Vou fazer rápido"   → Execution Contract checklist
├─ "Pulo validação"     → Validation Gates (build/lint/test)
├─ "Corrijo depois"     → Rollback procedure no Blueprint
├─ "Sei o que faço"     → ADR com Decision + Consequences
└─ "Documento depois"   → Documentation Sync (Workflow 6)
```

**Métrica:** % de tasks com `Execution Contract` válido antes de iniciar → target 100%.

---

### 3. Specialization over Generalism

> **Agentes especializados superam generalistas em qualidade e velocidade.**

| Generalista | Especializado (Stack) |
|---|---|
| Um agente "code" faz tudo | `architect` planeja, `code` implementa, `test-engineer` valida, `docs-specialist` documenta |
| Contexto inchado | Contexto mínimo por papel |
| Permissões amplas | Permissões mínimas necessárias |
| Modelo único | Modelo otimizado por tarefa |

**Regra:** Se um agente faz >3 tipos de tarefa distintos, divida.

---

### 4. Reproducibility over Creativity

> **Mesmo input → mesmo output. Criatividade controlada, não livre.**

```yaml
# Exemplo: architect agent
temperature: 0.5    # Baixa = determinístico
top_p: 0.9
steps: 25

# Exemplo: code agent  
temperature: 0.4    # Muito baixa = reprodutível
top_p: 0.9
steps: 50
```

**Aplicação:**
- Seeds fixos para geração de código
- Templates para ADRs, Blueprints, TODOs
- Execution Contract valida pré-condições
- Validation Gates garantem pós-condições

---

### 5. Determinism over Stochastic Exploration

> **Exploração tem custo; determinismo tem valor. Pague pelo primeiro, ganhe pelo segundo.**

| Estocástico (caro) | Determinístico (barato) |
|---|---|
| Tentar 5 abordagens | Uma abordagem validada por gates |
| "Vamos ver o que acontece" | "Contract diz: se X falha, rollback" |
| Debug por intuição | Systematic-debugging skill (4 fases) |
| Refactor "melhorando" | Strangler Fig + Branch by Abstraction |

**Orçamento:** Máximo 20% do tempo em exploração; 80% em execução validada.

---

### 6. Documentation over Tribal Knowledge

> **Se não está escrito, não existe. Se está escrito mas desatualizado, mente.**

| Tribal (risco) | Documentado (ativo) |
|---|---|
| "Pergunte ao Luciano" | `AGENTS.md` + `docs/dsg/` |
| "Sempre fizemos assim" | ADR com Decision + Consequences |
| "O agente sabe" | Skill com `Failure Modes` + `Recovery` |
| "Configuração padrão" | `kilo.json` + `kilo.jsonc` versionados |
| "Workflow conhecido" | Blueprint + TODO + Workflow Cookbook |

**Enforcement:** `documentation-reconciliation` skill roda em CI; falha se drift > threshold.

---

### 7. Review over Trust

> **Validação automática > confiança no autor. Peer review > auto-review.**

```
Camadas de Review (todas obrigatórias):
├── 1. Self-validation     → Build + lint + test (auto)
├── 2. Spec compliance     → acceptance-testing skill (auto)
├── 3. Code quality        → code-review skill (agent)
├── 4. Architecture        → architecture-review-kilo (agent)
├── 5. Security            → security-review (agent)
├── 6. Documentation       → documentation-reconciliation (auto)
└── 7. Human approval      → question tool / PR review
```

**Regra:** Nenhuma task "Concluída" sem passar camadas 1-6.

---

### 8. Context Preservation over Speed

> **Contexto perdido = retrabalho. Velocidade sem contexto = ilusão.**

| Perda de contexto | Preservação |
|---|---|
| Nova sessão = zero contexto | `kilo_local_recall` + AGENTS.md + skills |
| Subagente sem herança MCP | Kilo extrai dados → passa no prompt |
| Agent troca sem handoff | `task-progress.md` + `execution-contract.md` |
| Refactor sem spec | `reverse-engineering-specs` skill primeiro |
| Decisão não registrada | ADR criado *antes* da implementação |

**Custo:** ~5 min/session para preservar; economiza horas de re-onboarding.

---

## Tradeoffs

A stack **deliberadamente** aceita custos para ganhar qualidade. Entenda o que você paga e o que ganha.

---

### Onde a Stack Perde Velocidade

| Atividade | Overhead | Justificativa |
|---|---|---|
| **Setup inicial** | ~30 min | Configurar agents, skills, MCPs, ADRs |
| **Execution Contract** | ~5 min/task | Valida pré-condições; evita rework |
| **Validation Gates** | ~2-10 min/task | Build + lint + test a cada step |
| **Documentation Sync** | ~3 min/task | Mantém docs = código; evita drift |
| **ADR/Blueprint/TODO** | ~15 min/feature | Rastreabilidade; onboarding; audit |
| **Rollback procedure** | ~2 min/task | Seguro; permite experimentação |

**Total estimado:** +25-40% tempo bruto por feature.

---

### Onde a Stack Ganha Qualidade

| Métrica | Antes | Com Stack | Ganho |
|---|---|---|---|
| **Bugs em produção** | ~15% | ~2% | -87% |
| **Retrabalho** | ~30% | ~5% | -83% |
| **Onboarding humano** | 2-4 semanas | 2-4 horas | -95% |
| **Onboarding agente** | N/A | < 5 min | ∞ |
| **Auditoria decisão** | Dias | Segundos | -99% |
| **Rollback confiança** | Baixa | 100% | Total |
| **Drift docs/código** | Constante | Zero (CI) | Eliminação |

---

### Onde Aumenta Custo

| Custo | Valor |
|---|---|
| **Tokens** | +15-20% (prompts detalhados, validações, docs) |
| **Setup MCPs** | Tempo inicial (OAuth, PATs, config) |
| **Manutenção skills** | Versionamento, audit, update |
| **Governance overhead** | Reviews, ADRs, checklists |

**ROI:** Break-even em ~3 features médias; positivo a partir da 4ª.

---

### Onde Reduz Risco

| Risco | Mitigação da Stack |
|---|---|
| **Key person dependency** | DSG + skills versionadas + agents especializados |
| **Decisão arquitetural perdida** | ADR obrigatório antes de implementar |
| **Integração externa quebra** | MCP degradation + cache + retry policies |
| **Agente alucina** | Validation gates + deterministic models + review |
| **Scope creep** | Blueprint + TODO + Execution Contract |
| **Security incident** | Permission granular + MCP audit + security-review |

---

## Decision Matrix

Use esta matriz quando precisar escolher entre opções:

| Critério | Peso | Pergunta |
|---|---|---|
| **Reversibilidade** | Alto | "Posso desfazer em < 5 min?" |
| **Rastreabilidade** | Alto | "Daqui 6 meses saberei *por que*?" |
| **Onboarding** | Médio | "Novo agente/humano entende sozinho?" |
| **Custo marginal** | Médio | "Tokens/tempo adicionais justificam?" |
| **Composabilidade** | Baixo | "Funciona com skills/agents/MCPs existentes?" |

**Regra:** Se ≥3 critérios "Não" → não faça / refatore abordagem.