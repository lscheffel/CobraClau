# Schema - Aba Contratos (Fichas-Locacao)

> Schema definido para a aba Contratos da planilha "Fichas-Locacao" no Google Drive.

---

## Visão Geral

Esta planilha armazena os dados de todos os contratos de locação ativos. É a fonte única de verdade para o motor de cálculo de cobrança mensal.

---

## Colunas da Aba Contratos

| # | Coluna | Tipo | Obrigatória | Descrição |
|---|--------|------|-------------|-----------|
| 1 | `id_contrato` | Texto | ✅ | Identificador único do contrato (ex: "CONTR-001") |
| 2 | `status` | Texto | ✅ | Status do contrato: "Ativo", "Suspenso", "Encerrado" |
| 3 | `locador_nome` | Texto | ✅ | Nome completo do locador (proprietário) |
| 4 | `locador_cpf_cnpj` | Texto | ✅ | CPF ou CNPJ do locador |
| 5 | `locador_email` | Texto | ✅ | Email do locador para contato |
| 6 | `locador_telefone` | Texto | ☐ | Telefone do locador |
| 7 | `locador_chave_pix` | Texto | ☐ | Chave PIX do locador para repasse |
| 8 | `imovel_endereco` | Texto | ✅ | Endereço completo do imóvel |
| 9 | `imovel_numero` | Texto | ✅ | Número do imóvel |
| 10 | `imovel_complemento` | Texto | ☐ | Complemento (apt, sala, etc.) |
| 11 | `imovel_bairro` | Texto | ✅ | Bairro do imóvel |
| 12 | `imovel_cidade` | Texto | ✅ | Cidade do imóvel |
| 13 | `imovel_uf` | Texto | ✅ | UF do imóvel (2 caracteres) |
| 14 | `imovel_cep` | Texto | ✅ | CEP do imóvel |
| 15 | `imovel_tipo` | Texto | ✅ | Tipo do imóvel: "Residencial", "Comercial", "Misto" |
| 16 | `imovel_area_m2` | Número | ☐ | Área do imóvel em m² |
| 17 | `imovel_vagas` | Número | ☐ | Número de vagas de garagem |
| 18 | `locatario_nome` | Texto | ✅ | Nome completo do locatário (inquilino) |
| 19 | `locatario_cpf` | Texto | ✅ | CPF do locatário |
| 20 | `locatario_email` | Texto | ✅ | Email do locatário para contato |
| 21 | `locatario_telefone` | Texto | ☐ | Telefone do locatário |
| 22 | `locatario_data_nascimento` | Data | ☐ | Data de nascimento do locatário |
| 23 | `contrato_data_inicio` | Data | ✅ | Data de início do contrato |
| 24 | `contrato_data_fim` | Data | ☐ | Data de término do contrato (vazio = prazo indeterminado) |
| 25 | `contrato_prazo_meses` | Número | ☐ | Prazo do contrato em meses |
| 26 | `contrato_reajuste_anual` | Texto | ✅ | Critério de reajuste: "IGPM", "IPCA", "Fixo", "Nenhum" |
| 27 | `contrato_indice_reajuste` | Texto | ☐ | Índice específico (se "Fixo") |
| 28 | `contrato_percentual_reajuste` | Número | ☐ | Percentual fixo de reajuste (se aplicável) |
| 29 | `valor_aluguel` | Moeda | ✅ | Valor do aluguel base (R$) |
| 30 | `valor_condominio` | Moeda | ☐ | Valor do condomínio (se não vier no boleto) |
| 31 | `valor_iptu` | Moeda | ☐ | Valor do IPTU (se não vier no boleto) |
| 32 | `responsabilidade_iptu` | Texto | ✅ | Quem paga IPTU: "Locador", "Locatário", "Dividido" |
| 33 | `responsabilidade_internet` | Texto | ✅ | Quem paga internet: "Locador", "Locatário", "N/A" |
| 34 | `responsabilidade_luz` | Texto | ✅ | Quem paga luz: "Locador", "Locatário", "N/A" |
| 35 | `responsabilidade_agua` | Texto | ✅ | Quem paga água: "Locador", "Locatário", "N/A" |
| 36 | `responsabilidade_gas` | Texto | ✅ | Quem paga gás: "Locador", "Locatário", "N/A" |
| 37 | `forma_cobranca_aluguel` | Texto | ✅ | Como cobra aluguel: "Boleto", "PIX", "Débito Automático" |
| 38 | `forma_cobranca_condominio` | Texto | ✅ | Como cobra condomínio: "Boleto", "PIX", "Embutido no Aluguel" |
| 39 | `vencimento_aluguel` | Número | ✅ | Dia do vencimento do aluguel (1-31) |
| 40 | `vencimento_condominio` | Número | ☐ | Dia do vencimento do condomínio (1-31) |
| 41 | `excecoes_calculo` | Texto longo | ☐ | Exceções específicas ao cálculo deste contrato |
| 42 | `observacoes` | Texto longo | ☐ | Observações gerais sobre o contrato |
| 43 | `data_criacao` | Data | ✅ | Data de criação do registro |
| 44 | `data_atualizacao` | Data | ✅ | Data da última atualização |

---

## Exemplo de Preenchimento

| Coluna | Exemplo |
|--------|---------|
| id_contrato | CONTR-001 |
| status | Ativo |
| locador_nome | João da Silva |
| locador_cpf_cnpj | 123.456.789-00 |
| locador_email | joao.silva@email.com |
| imovel_endereco | Rua das Flores |
| imovel_numero | 123 |
| imovel_complemento | Apt 101 |
| imovel_bairro | Centro |
| imovel_cidade | São Paulo |
| imovel_uf | SP |
| imovel_cep | 01234-567 |
| imovel_tipo | Residencial |
| locatario_nome | Maria Santos |
| locatario_cpf | 987.654.321-00 |
| locatario_email | maria.santos@email.com |
| contrato_data_inicio | 01/01/2024 |
| contrato_reajuste_anual | IPCA |
| valor_aluguel | R$ 2.500,00 |
| responsabilidade_iptu | Locatário |
| responsabilidade_internet | Locatário |
| responsabilidade_luz | Locatário |
| responsabilidade_agua | Locatário |
| responsabilidade_gas | Locatário |
| forma_cobranca_aluguel | Boleto |
| forma_cobranca_condominio | Boleto |
| vencimento_aluguel | 10 |
| vencimento_condominio | 15 |
| data_criacao | 07/07/2026 |
| data_atualizacao | 07/07/2026 |

---

## Validações

### Obrigatoriedade
- Todas as colunas marcadas como "Obrigatória" devem ser preenchidas
- Colunas opcionais podem ficar vazias

### Formato
- `id_contrato`: Formato "CONTR-XXX" (XXX = número sequencial)
- `status`: Valores permitidos: "Ativo", "Suspenso", "Encerrado"
- `imovel_uf`: 2 caracteres maiúsculos (SP, RJ, MG, etc.)
- `imovel_cep`: Formato "XXXXX-XXX"
- `valor_*`: Formato numérico (sem formatação de moeda)
- `vencimento_*`: Número entre 1 e 31
- `data_*`: Formato "DD/MM/AAAA"

### Integridade
- `contrato_data_fim` deve ser posterior a `contrato_data_inicio` (se preenchida)
- `responsabilidade_*` deve ser "Locador", "Locatário" ou "N/A"
- `forma_cobranca_*` deve ser "Boleto", "PIX", "Débito Automático" ou "Embutido no Aluguel"

---

## Notas

1. **Exceções de cálculo**: O campo `excecoes_calculo` permite documentar regras específicas para contratos atípicos (ex: "Aluguel já embute condomínio").

2. **Atualização**: O campo `data_atualizacao` deve ser atualizado sempre que houver alteração no contrato.

3. **Histórico**: Valores de aluguel e encargos devem ser atualizados a cada reajuste, mantendo o histórico em versão anterior.

4. **Integração**: Este schema é lido pelo motor de cálculo para determinar:
   - Quais rubricas se aplicam a cada contrato
   - Quem é responsável por cada rubrica
   - Como calcular valores para locador e locatário