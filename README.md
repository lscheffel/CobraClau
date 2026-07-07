# CobraClau

Sistema de cobrança mensal de locação assistido por IA para gerenciamento de contratos de locação residencial.

## Visão Geral

O CobraClau automatiza o fechamento mensal de cobrança de locação, processando boletos de condomínio e gerando cards de cobrança para locadores e locatários com cálculo determinístico e auditável.

### Princípio Fundamental

**Nenhum valor monetário em um card sai de texto gerado livremente pela IA.** Todo número é resultado de uma função determinística e auditável. A IA decide classificação de rubrica ambígua e redige o card — nunca soma dinheiro "no olho".

## Funcionalidades

- Processamento de boletos de condomínio (texto, PDF ou CSV)
- Classificação automática de rubricas conforme tabela de regras
- Cálculo determinístico de valores para locador e locatário
- Geração de cards de cobrança padronizados
- Memória de cálculo auditável no Notion
- Sinalização explícita de pendências para revisão humana

## Estrutura do Projeto

```
CobraClau/
├── docs/                    # Documentação
│   ├── adr/                 # Architecture Decision Records
│   └── architecture/        # Documentação de arquitetura
├── templates/               # Templates de card
├── src/                     # Código fonte (se aplicável)
├── .github/workflows/       # CI/CD
├── README.md
├── AGENTS.md
├── CONTRIBUTING.md
├── CHANGELOG.md
├── CODE_OF_CONDUCT.md
└── SECURITY.md
```

## Pré-requisitos

- Claude Project com conectores Google Drive e Notion autenticados
- Google Drive com planilha de fichas de locação
- Notion com database de memória de cálculo configurada

## Configuração

### 1. Google Drive
- Criar planilha "Fichas-Locacao" com abas:
  - **Contratos**: locador, imóvel, locatário, aluguel, IPTU, internet, luz, forma de cobrança, exceções
  - **Regras-Rubrica**: rubrica, classe, responsável padrão, base legal

### 2. Notion
- Criar database "Memória de Cobrança" com propriedades:
  - Mês, Contrato, Imóvel, Rubricas classificadas
  - Valor Locador, Valor Locatário
  - Status (revisado/enviado), Pendências

### 3. Claude Project
- Configurar Project Knowledge com:
  - Legislação relevante (Lei 8.245/91, Código Civil)
  - Templates de card (locador e locatário)
  - Instruções do motor de cálculo

## Uso

1. Luciano cola o boleto de condomínio no chat do Claude Project
2. O motor:
   - Lê as fichas atualizadas no Google Drive
   - Extrai e classifica as rubricas do boleto
   - Calcula valores para locador e locatário
   - Preenche os templates de card
   - Grava o registro na memória do Notion
3. Retorna os cards prontos + lista de pendências (se houver)

## Documentação

- [ADR-COB-001](docs/adr/ADR-COB-001.md) - Sistema de cobrança mensal
- [Blueprint](docs/adr/ADR-COB-001-BP.md) - Plano de implementação
- [TODO](docs/adr/ADR-COB-001-TODO.md) - Tarefas de implementação

## Contribuição

Consulte [CONTRIBUTING.md](CONTRIBUTING.md) para detalhes sobre como contribuir.

## Licença

Este projeto é proprietário e não está licenciado para uso público.