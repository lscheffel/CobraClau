# Task Progress - A1.1

> Progresso individual de uma tarefa durante a execução governada.

---

## Identificação

| Campo | Valor |
|-------|-------|
| Tarefa | Definir schema da aba Contratos |
| Número | A1.1 |
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
| 2026-07-07 | 🔄 Em andamento | ✅ Concluído | Schema definido e documentado |

---

## Descrição da Tarefa

Definir o schema da aba Contratos da planilha "Fichas-Locacao" no Google Drive. O schema deve conter:

- Locador (dados do proprietário)
- Imóvel (endereço e características)
- Locatário (dados do inquilino)
- Aluguel (valor base)
- IPTU (responsabilidade)
- Internet (responsabilidade)
- Luz (responsabilidade)
- Forma de cobrança de cada item
- Exceções de contrato

---

## Dependências

| # | Dependência | Estado | Pode iniciar? |
|---|-------------|--------|---------------|
| — | Nenhuma | — | ✅ Sim |

---

## Alterações Realizadas

| # | Arquivo | Tipo | Descrição | Linhas |
|---|---------|------|-----------|--------|
| 1 | docs/architecture/schema-contratos.md | Criação | Schema completo da aba Contratos | 150+ |

---

## Validações

| # | Validação | Resultado | Tentativa | Timestamp | Observações |
|---|-----------|-----------|-----------|-----------|-------------|
| 1 | Schema contém todas as colunas essenciais | ✅ Passou | 1 | 2026-07-07 | 44 colunas definidas |
| 2 | Schema é coerente com o fluxo de cálculo | ✅ Passou | 1 | 2026-07-07 | Cobertura completa |
| 3 | Schema permite exceções de contrato | ✅ Passou | 1 | 2026-07-07 | Campo excecoes_calculo |

---

## Bloqueadores

| # | Bloqueador | Data Identificação | Data Resolução | Ação |
|---|------------|-------------------|----------------|------|
| — | Nenhum bloqueador | — | — | — |

---

## Critérios de Aceite

| # | Critério | Atendido | Evidência |
|---|----------|----------|-----------|
| 1 | Schema contém todas as colunas essenciais | ✅ | 44 colunas documentadas em schema-contratos.md |
| 2 | Schema é coerente com o fluxo de cálculo | ✅ | Cobertura completa do fluxo ADR-COB-001 |
| 3 | Schema permite exceções de contrato | ✅ | Campo excecoes_calculo incluído |

---

## Observações

- Esta é a primeira tarefa do projeto
- Schema deve ser validado contra o fluxo de cálculo definido no Blueprint
- Colunas devem suportar dados reais dos contratos de Luciano

---

## Resumo

| Campo | Valor |
|-------|-------|
| Tarefa concluída | ✅ Sim |
| Validações passaram | 3/3 |
| Documentação atualizada | ✅ Sim |
| Próxima tarefa | A1.2 (Criar planilha no Drive) |