# Schema - Aba Histórico (Memória de Cálculo)

> Schema definido para a aba Historico da planilha "Fichas-Locacao" no Google Sheets.

---

## Visão Geral

Esta aba armazena o histórico de cálculos de cobrança mensal. É a memória de cálculo que permite auditoria e consulta retroativa.

---

## Colunas da Aba Histórico

| # | Coluna | Tipo | Obrigatória | Descrição |
|---|--------|------|-------------|-----------|
| 1 | `id_registro` | Texto | ✅ | Identificador único do registro (ex: "HIST-202607-001") |
| 2 | `mes_ano` | Texto | ✅ | Mês e ano de referência (ex: "Julho/2026") |
| 3 | `data_processamento` | Data/Hora | ✅ | Data e hora do processamento |
| 4 | `id_contrato` | Texto | ✅ | Identificador do contrato (referência à aba Contratos) |
| 5 | `imovel_endereco` | Texto | ✅ | Endereço do imóvel |
| 6 | `locador_nome` | Texto | ✅ | Nome do locador |
| 7 | `locatario_nome` | Texto | ✅ | Nome do locatário |
| 8 | `valor_aluguel` | Moeda | ✅ | Valor do aluguel base |
| 9 | `rubricas_processadas` | Texto longo | ✅ | Lista de rubricas processadas com classificação |
| 10 | `valor_condominio_locatario` | Moeda | ✅ | Total de rubricas ordinárias (locatário) |
| 11 | `valor_condominio_locador` | Moeda | ✅ | Total de rubricas extraordinárias (locador) |
| 12 | `valor_iptu` | Moeda | ☐ | Valor do IPTU (se aplicável) |
| 13 | `total_locatario` | Moeda | ✅ | Total a cobrar do locatário |
| 14 | `total_locador` | Moeda | ✅ | Total a repassar ao locador |
| 15 | `pendencias` | Texto longo | ☐ | Lista de pendências de classificação |
| 16 | `status` | Texto | ✅ | Status: "revisado", "enviado", "pendente" |
| 17 | `observacoes` | Texto longo | ☐ | Observações adicionais |
| 18 | `data_revisao` | Data | ☐ | Data da revisão humana |
| 19 | `responsavel_revisao` | Texto | ☐ | Quem revisou o card |

---

## Exemplo de Preenchimento

| Coluna | Exemplo |
|--------|---------|
| id_registro | HIST-202607-001 |
| mes_ano | Julho/2026 |
| data_processamento | 07/07/2026 20:30:00 |
| id_contrato | CONTR-001 |
| imovel_endereco | Av. Beira Mar, 101 - Capão da Canoa/RS |
| locador_nome | Locador Mock 1 |
| locatario_nome | Locatário Mock 1 |
| valor_aluguel | R$ 2.650,00 |
| rubricas_processadas | Água fria: R$ 45 (Locatário), IPTU: R$ 120 (Locatário), Taxa admin: R$ 80 (Locatário) |
| valor_condominio_locatario | R$ 245,00 |
| valor_condominio_locador | R$ 0,00 |
| valor_iptu | R$ 120,00 |
| total_locatario | R$ 3.015,00 |
| total_locador | R$ 2.650,00 |
| pendencias | (vazio) |
| status | revisado |
| observacoes | (vazio) |
| data_revisao | 07/07/2026 |
| responsavel_revisao | Luciano |

---

## Validações

### Obrigatoriedade
- Todas as colunas marcadas como "Obrigatória" devem ser preenchidas
- Colunas opcionais podem ficar vazias

### Formato
- `id_registro`: Formato "HIST-YYYYMM-XXX" (XXX = número sequencial)
- `mes_ano`: Formato "Mês/Ano" (ex: "Julho/2026")
- `data_processamento`: Formato "DD/MM/AAAA HH:MM:SS"
- `id_contrato`: Formato "CONTR-XXX" (referência à aba Contratos)
- `valor_*`: Formato numérico (sem formatação de moeda)
- `status`: Valores permitidos: "revisado", "enviado", "pendente"

### Integridade
- `id_contrato` deve existir na aba Contratos
- `total_locatario` = `valor_aluguel` + `valor_condominio_locatario` + `valor_iptu` (se aplicável)
- `total_locador` = `valor_aluguel` - `valor_condominio_locador` (se houver despesas extraordinárias)
- `status` deve ser "pendente" se houver pendências

---

## Regras de Escrita

1. **Um registro por contrato por mês**: Cada processamento mensal gera um registro para cada contrato ativo

2. **Idempotência**: Se um registro já existe para o mês/contrato, atualizar em vez de criar novo

3. **Backup**: Manter pelo menos 12 meses de histórico (1 ano)

4. **Arquivamento**: Registros com status "enviado" podem ser arquivados após 12 meses

---

## Integração com o Motor

O motor de cálculo deve:

1. **Antes de processar**: Verificar se já existe registro para o mês/contrato
2. **Durante o processamento**: Calcular valores determinísticos
3. **Após o processamento**: Gravar registro na aba Historico
4. **Após a revisão**: Atualizar status e dados de revisão

---

## Consultas Comuns

### Histórico de um contrato
```
Filtrar por id_contrato = "CONTR-001"
Ordenar por mes_ano decrescente
```

### Contratos pendentes de revisão
```
Filtrar por status = "pendente"
```

### Resumo mensal
```
Filtrar por mes_ano = "Julho/2026"
Somar total_locatario e total_locador
```

---

## Referências

- Schema de Contratos: `docs/architecture/schema-contratos.md`
- Regras de Rubrica: `docs/architecture/rubricas-comuns.md`
- Instruções do Motor: `templates/instrucoes-motor.md`