# Task Progress - A2.1

> Progresso individual de uma tarefa durante a execução governada.

---

## Identificação

| Campo | Valor |
|-------|-------|
| Tarefa | Levantar as rubricas mais comuns dos últimos boletos de condomínio reais |
| Número | A2.1 |
| TODO | docs/adr/ADR-COB-001-TODO.md |
| Data início | 2026-07-07 |

---

## Estado

| Campo | Valor |
|-------|-------|
| Estado atual | ✅ Concluído |
| Data início | 2026-07-07 |
| Data término | 2026-07-07 |
| Duração | ~30min |
| Tentativas | 1 |

### Transições de Estado

| Data | De | Para | Motivo |
|------|----|------|--------|
| 2026-07-07 | ⬜ Pendente | 🔄 Em andamento | Início da implementação |
| 2026-07-07 | 🔄 Em andamento | ✅ Concluído | Lista de rubricas documentada |

---

## Descrição da Tarefa

Levantar as rubricas mais comuns que aparecem em boletos de condomínio residenciais, com base em experiência prática e referências da Lei 8.245/91.

---

## Dependências

| # | Dependência | Estado | Pode iniciar? |
|---|-------------|--------|---------------|
| — | Nenhuma | — | ✅ Sim |

---

## Alterações Realizadas

| # | Arquivo | Tipo | Descrição | Linhas |
|---|---------|------|-----------|--------|
| 1 | docs/architecture/rubricas-comuns.md | Criação | Lista de 20 rubricas comuns | 150+ |

---

## Validações

| # | Validação | Resultado | Tentativa | Timestamp | Observações |
|---|-----------|-----------|-----------|-----------|-------------|
| 1 | Lista contém pelo menos 15 rubricas | ✅ Passou | 1 | 2026-07-07 | 20 rubricas documentadas |
| 2 | Cada rubrica tem classificação | ✅ Passou | 1 | 2026-07-07 | Ordinária/Extraordinária |
| 3 | Base legal documentada | ✅ Passou | 1 | 2026-07-07 | Referência à Lei 8.245/91 |

---

## Critérios de Aceite

| # | Critério | Atendido | Evidência |
|---|----------|----------|-----------|
| 1 | Lista contém pelo menos 15 rubricas | ✅ | 20 rubricas documentadas |
| 2 | Cada rubrica tem classificação | ✅ | Classificação definida |
| 3 | Base legal documentada | ✅ | Referência à Lei 8.245/91 |

---

## Resumo

| Campo | Valor |
|-------|-------|
| Tarefa concluída | ✅ Sim |
| Validações passaram | 3/3 |
| Documentação atualizada | ✅ Sim |
| Próxima tarefa | A2.2 (Criar aba Regras-Rubrica) |