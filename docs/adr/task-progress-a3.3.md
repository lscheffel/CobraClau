# Task Progress - A3.3

> Progresso individual de uma tarefa durante a execução governada.

---

## Identificação

| Campo | Valor |
|-------|-------|
| Tarefa | Testar escrita de um registro de teste no Notion |
| Número | A3.3 |
| TODO | docs/adr/ADR-COB-001-TODO.md |
| Data início | 2026-07-07 |

---

## Estado

| Campo | Valor |
|-------|-------|
| Estado atual | 🔄 Em andamento |
| Data início | 2026-07-07 |
| Data término | — |
| Duração | — |
| Tentativas | 1 |

### Transições de Estado

| Data | De | Para | Motivo |
|------|----|------|--------|
| 2026-07-07 | ⬜ Pendente | 🔄 Em andamento | Início da implementação |

---

## Descrição da Tarefa

Testar se é possível escrever um registro de teste na database "Memória de Cobrança" no Notion a partir do Claude.

---

## Dependências

| # | Dependência | Estado | Pode iniciar? |
|---|-------------|--------|---------------|
| — | Nenhuma | — | ✅ Sim |

---

## Alterações Realizadas

| # | Arquivo | Tipo | Descrição | Linhas |
|---|---------|------|-----------|--------|
| — | Nenhuma alteração ainda | — | — | — |

---

## Validações

| # | Validação | Resultado | Tentativa | Timestamp | Observações |
|---|-----------|-----------|-----------|-----------|-------------|
| — | Nenhuma validação ainda | — | — | — | — |

---

## ⚠️ Ação Necessária do Usuário

**Esta tarefa requer que Luciano teste a escrita no Notion.**

### Pré-requisitos

1. **Conta Notion** com acesso à API
2. **Database "Memória de Cobrança"** criada no Notion
3. **Integration token** configurado no Claude Project

### Passos para teste

1. **Verificar se a database existe** no Notion:
   - Nome: "Memória de Cobrança"
   - Propriedades esperadas:
     - Mês (Texto)
     - Contrato (Texto)
     - Imóvel (Texto)
     - Rubricas classificadas (Texto longo)
     - Valor Locador (Número)
     - Valor Locatário (Número)
     - Status (Seleção: revisado/enviado)
     - Pendências (Texto longo)

2. **Testar escrita** de um registro de exemplo:
   ```json
   {
     "Mês": "Julho/2026",
     "Contrato": "CONTR-001",
     "Imóvel": "Av. Beira Mar, 101 - Capão da Canoa/RS",
     "Rubricas classificadas": "Água fria: Locatário, IPTU: Locatário",
     "Valor Locador": 0,
     "Valor Locatário": 570,
     "Status": "revisado",
     "Pendências": ""
   }
   ```

3. **Confirmar** que o registro foi criado na database

4. **Retornar** o resultado do teste

---

## Critérios de Aceite

| # | Critério | Atendido | Evidência |
|---|----------|----------|-----------|
| 1 | Database "Memória de Cobrança" existe | ⬜ | Confirmação do usuário |
| 2 | Escrita de registro teste funcionou | ⬜ | Print ou confirmação |
| 3 | Propriedades corretas | ⬜ | Verificação na database |

---

## Observações

- Esta tarefa é dependência para B2.7 (Criar database no Notion)
- Se a database não existir, será necessário criá-la primeiro
- Documentação do schema: `docs/architecture/schema-memoria-notion.md`

---

## Resumo

| Campo | Valor |
|-------|-------|
| Tarefa concluída | ❌ Não (aguardando usuário) |
| Validações passaram | 0/3 |
| Documentação atualizada | ⬜ Não |
| Próxima tarefa | Checkpoint A3 |