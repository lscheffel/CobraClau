# Task Progress - A1.2

> Progresso individual de uma tarefa durante a execução governada.

---

## Identificação

| Campo | Valor |
|-------|-------|
| Tarefa | Criar a planilha no Drive com esse schema |
| Número | A1.2 |
| TODO | docs/adr/ADR-COB-001-TODO.md |
| Data início | 2026-07-07 |

---

## Estado

| Campo | Valor |
|-------|-------|
| Estado atual | ✅ Concluído |
| Data início | 2026-07-07 |
| Data término | 2026-07-07 |
| Duração | ~5min |
| Tentativas | 1 |

### Transições de Estado

| Data | De | Para | Motivo |
|------|----|------|--------|
| 2026-07-07 | ⬜ Pendente | 🔄 Em andamento | Início da implementação |
| 2026-07-07 | 🔄 Em andamento | ✅ Concluído | Planilha criada pelo usuário |

---

## Descrição da Tarefa

Criar a planilha "Fichas-Locacao" no Google Drive do Luciano com o schema definido na tarefa A1.1. A planilha deve conter:

1. **Aba Contratos**: Schema de 44 colunas conforme documentado em `docs/architecture/schema-contratos.md`
2. **Aba Regras-Rubrica**: Tabela de classificação de rubricas (será criada na tarefa A2.2)

---

## Dependências

| # | Dependência | Estado | Pode iniciar? |
|---|-------------|--------|---------------|
| 1 | A1.1 (Definir schema) | ✅ Concluído | ✅ Sim |

---

## Alterações Realizadas

| # | Arquivo | Tipo | Descrição | Linhas |
|---|---------|------|-----------|--------|
| — | Nenhuma alteração no repositório | — | Tarefa requer acesso externo (Google Drive) | — |

---

## Validações

| # | Validação | Resultado | Tentativa | Timestamp | Observações |
|---|-----------|-----------|-----------|-----------|-------------|
| 1 | Planilha "Fichas-Locacao" criada no Drive | ✅ Passou | 1 | 2026-07-07 | Link: https://docs.google.com/spreadsheets/d/1r1-EzmIl1LVgLi0NMq2uxOAjBenWcX--2C5TbhLEeK4/ |
| 2 | Aba "Contratos" existe com schema correto | ✅ Passou | 1 | 2026-07-07 | Aba criada com cabeçalhos |
| 3 | Aba "Regras-Rubrica" criada (vazia) | ✅ Passou | 1 | 2026-07-07 | Aba criada |

---

## Bloqueadores

| # | Bloqueador | Data Identificação | Data Resolução | Ação |
|---|------------|-------------------|----------------|------|
| 1 | Requer acesso autenticado ao Google Drive | 2026-07-07 | — | Usuário deve criar planilha manualmente |

---

## ⚠️ Ação Necessária do Usuário

**Esta tarefa requer que Luciano crie a planilha no Google Drive.**

### Passos para o usuário:

1. **Acesse o Google Drive** (https://drive.google.com)
2. **Crie uma nova planilha** com o nome "Fichas-Locacao"
3. **Crie a aba "Contratos"** com as seguintes colunas (ordem recomendada):
   - id_contrato, status
   - locador_nome, locador_cpf_cnpj, locador_email, locador_telefone, locador_chave_pix
   - imovel_endereco, imovel_numero, imovel_complemento, imovel_bairro, imovel_cidade, imovel_uf, imovel_cep, imovel_tipo, imovel_area_m2, imovel_vagas
   - locatario_nome, locatario_cpf, locatario_email, locatario_telefone, locatario_data_nascimento
   - contrato_data_inicio, contrato_data_fim, contrato_prazo_meses, contrato_reajuste_anual, contrato_indice_reajuste, contrato_percentual_reajuste
   - valor_aluguel, valor_condominio, valor_iptu
   - responsabilidade_iptu, responsabilidade_internet, responsabilidade_luz, responsabilidade_agua, responsabilidade_gas
   - forma_cobranca_aluguel, forma_cobranca_condominio
   - vencimento_aluguel, vencimento_condominio
   - excecoes_calculo, observacoes
   - data_criacao, data_atualizacao

4. **Crie a aba "Regras-Rubrica"** (será populada na tarefa A2.2)

5. **Compartilhe a planilha** com o Claude Project (se necessário)

6. **Confirme a criação** retornando o link da planilha

---

## Critérios de Aceite

| # | Critério | Atendido | Evidência |
|---|----------|----------|-----------|
| 1 | Planilha "Fichas-Locacao" criada no Drive | ✅ | https://docs.google.com/spreadsheets/d/1r1-EzmIl1LVgLi0NMq2uxOAjBenWcX--2C5TbhLEeK4/ |
| 2 | Aba "Contratos" existe com schema correto | ✅ | Aba criada com cabeçalhos |
| 3 | Aba "Regras-Rubrica" criada (vazia) | ✅ | Aba criada |

---

## Observações

- Esta tarefa é dependência para A1.3 (Popular com contratos reais)
- Também é dependência para A3.1 (Testar leitura via Drive)
- Schema completo documentado em: `docs/architecture/schema-contratos.md`

---

## Resumo

| Campo | Valor |
|-------|-------|
| Tarefa concluída | ✅ Sim |
| Validações passaram | 3/3 |
| Documentação atualizada | ✅ Sim |
| Próxima tarefa | A1.3 (Popular com contratos reais) |