# TODO: Sistema de cobrança mensal de locação assistido por IA

> ADR-COB-001 | Início: 2026-07-07 | Status: ⬜ PENDENTE

---

## Legenda

- ✅ Concluído
- ⬜ Pendente
- 🔄 Em Andamento
- ❌ Bloqueado
- ⏸️ Pausado

**Prioridade:** 🔴 Alta | 🟡 Média | 🟢 Baixa

---

## Fase A: Fundação de dados

### A1: Planilha de fichas

| # | Tarefa | Status | Prioridade | Dependências | Estimativa |
|---|--------|--------|------------|--------------|------------|
| A1.1 | Definir schema da aba Contratos (locador, imóvel, locatário, aluguel, IPTU, internet, luz, forma de cobrança de cada item, exceções) | ✅ | 🔴 | — | 1h |
| A1.2 | Criar a planilha no Drive com esse schema | ✅ | 🔴 | A1.1 | 30min |
| A1.3 | Popular com os contratos reais atualmente ativos | ✅ | 🔴 | A1.2 | 2h |

**Checkpoint A1:**
- [ ] Planilha existe no Drive com todos os contratos ativos de Luciano
- [ ] Schema aprovado — nenhuma coluna essencial faltando

---

### A2: Tabela de regras de rubrica

| # | Tarefa | Status | Prioridade | Dependências | Estimativa |
|---|--------|--------|------------|--------------|------------|
| A2.1 | Levantar as rubricas mais comuns dos últimos boletos de condomínio reais | ⬜ | 🔴 | — | 1h |
| A2.2 | Criar a aba/tabela Regras-Rubrica com classe e base legal por linha | ⬜ | 🔴 | A2.1 | 1h |
| A2.3 | Validar a tabela contra a Lei 8.245/91, arts. 22-23 (revisão cruzada) | ⬜ | 🔴 | A2.2 | 1h |

**Checkpoint A2:**
- [ ] Tabela cobre pelo menos as rubricas dos últimos 3 boletos recebidos
- [ ] Toda linha tem base legal ou justificativa contratual associada

---

### A3: Validação de acesso

| # | Tarefa | Status | Prioridade | Dependências | Estimativa |
|---|--------|--------|------------|--------------|------------|
| A3.1 | Testar leitura da planilha via conector Google Drive no Claude | ⬜ | 🔴 | A1.2 | 30min |
| A3.2 | Confirmar se o conector lê Google Sheets nativamente; se não, definir fallback CSV publicado | ⬜ | 🔴 | A3.1 | 1h |
| A3.3 | Testar escrita de um registro de teste no Notion | ⬜ | 🔴 | — | 30min |

**Checkpoint A3:**
- [ ] Leitura real da planilha confirmada a partir do chat
- [ ] Escrita real no Notion confirmada a partir do chat

---

**Checkpoint Geral Fase A:**
- [ ] Fichas e regras carregadas com dados reais
- [ ] Os dois conectores (Drive e Notion) validados ponta a ponta

---

## Fase B: Motor e templates

### B1: Base de conhecimento

| # | Tarefa | Status | Prioridade | Dependências | Estimativa |
|---|--------|--------|------------|--------------|------------|
| B1.1 | Extrair trechos relevantes da Lei 8.245/91 (arts. 22-23) num arquivo de referência | ⬜ | 🔴 | — | 1h |
| B1.2 | Extrair trechos relevantes do Código Civil aplicáveis subsidiariamente | ⬜ | 🟡 | — | 1h |
| B1.3 | Redigir as instruções do motor: regras de classificação, formato de cálculo, critério de quando marcar pendência | ⬜ | 🔴 | A2 | 2h |
| B1.4 | Subir os arquivos como Project Knowledge no Claude | ⬜ | 🔴 | B1.1, B1.2, B1.3 | 30min |

**Checkpoint B1:**
- [ ] Legislação relevante disponível como referência no Project
- [ ] Instruções do motor cobrem o critério de pendência explicitamente

---

### B2: Templates de card e memória

| # | Tarefa | Status | Prioridade | Dependências | Estimativa |
|---|--------|--------|------------|--------------|------------|
| B2.1 | Definir template de card do locatário | ⬜ | 🔴 | — | 1h |
| B2.2 | Definir template de card do locador | ⬜ | 🔴 | — | 1h |
| B2.3 | Validar os dois templates com um exemplo real preenchido manualmente | ⬜ | 🔴 | B2.1, B2.2 | 1h |
| B2.4 | Ajustar redação conforme feedback de Luciano | ⬜ | 🟡 | B2.3 | 1h |
| B2.5 | Subir templates ao Project Knowledge | ⬜ | 🔴 | B2.4 | 30min |
| B2.6 | Definir schema da memória de cálculo no Notion (propriedades da database) | ⬜ | 🔴 | — | 1h |
| B2.7 | Criar a database no Notion | ⬜ | 🔴 | B2.6 | 30min |
| B2.8 | Testar escrita ponta a ponta do motor com um contrato de teste | ⬜ | 🔴 | B1.4, B2.5, B2.7 | 2h |
| B2.9 | Revisar o registro gravado no Notion contra o card gerado | ⬜ | 🔴 | B2.8 | 30min |

**Checkpoint B2:**
- [ ] Motor gera 1 card de teste completo a partir de um boleto fake
- [ ] Registro correspondente aparece corretamente na memória do Notion
- [ ] Templates aprovados por Luciano

---

**Checkpoint Geral Fase B:**
- [ ] Motor consegue ler fichas + regras, gerar card e gravar memória de ponta a ponta com dados de teste

---

## Fase C: Piloto e ajuste

### C1: Fechamento piloto com dados reais

| # | Tarefa | Status | Prioridade | Dependências | Estimativa |
|---|--------|--------|------------|--------------|------------|
| C1.1 | Rodar o fechamento completo de um mês real, com todos os contratos ativos | ⬜ | 🔴 | B | 2h |
| C1.2 | Revisar manualmente 100% dos cards gerados antes de considerar "pronto pra enviar" | ⬜ | 🔴 | C1.1 | 1h |
| C1.3 | Documentar pendências e ajustes na tabela de regras a partir do piloto | ⬜ | 🟡 | C1.2 | 1h |

**Checkpoint C1:**
- [ ] Primeiro mês fechado e revisado integralmente
- [ ] Tabela de regras atualizada com o aprendizado do piloto

---

**Checkpoint Geral Fase C:**
- [ ] Ciclo mensal completo validado com dados reais, do boleto colado até o card pronto e a memória gravada

---

## Resumo Geral

| Fase | Tarefas | Horas Est. | Status |
|------|---------|------------|--------|
| Fase A: Fundação de dados | 9 | ~8h | ⬜ |
| Fase B: Motor e templates | 13 | ~11h | ⬜ |
| Fase C: Piloto e ajuste | 3 | ~4h | ⬜ |
| **Total** | **25** | **~23h** | |

---

## Dependências entre Fases

```
Fase A (Fundação de dados)
  │
  ├─── A1: Planilha de fichas ─────┐
  ├─── A2: Regras de rubrica ──────┤
  └─── A3: Validação de acesso ────┘
                                    │
Fase B (Motor e templates) ◄───────┘
  │
  ├─── B1: Base de conhecimento ───┐
  └─── B2: Templates e memória ────┘
                                    │
Fase C (Piloto e ajuste) ◄─────────┘
  │
  └─── C1: Fechamento piloto ──────┘
```

---

*Documento gerado em 2026-07-07. Referência: ADR-COB-001.*
