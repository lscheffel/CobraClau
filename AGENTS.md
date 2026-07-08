# AGENTS.md - CobraClau

## Visão Geral

Este projeto é um sistema de cobrança mensal de locação assistido por IA. O motor processa boletos de condomínio e gera cards de cobrança com cálculo determinístico e auditável.

## Princípio Fundamental

**REGRA INEGOCIÁVEL:** Nenhum valor monetário em um card sai de texto gerado livremente pela IA. Todo número é resultado de cálculo determinístico (soma/rateio das rubricas do boleto conforme a tabela de regras).

## Estrutura do Projeto

```
CobraClau/
├── docs/
│   ├── adr/                 # Architecture Decision Records
│   │   ├── ADR-COB-001.md   # Decisão principal
│   │   ├── ADR-COB-001-BP.md # Blueprint de implementação
│   │   └── ADR-COB-001-TODO.md # Tarefas
│   └── architecture/        # Documentação de arquitetura
├── templates/               # Templates de card
├── src/                     # Código fonte (se aplicável)
└── .github/workflows/       # CI/CD
```

## Fluxo de Trabalho

1. **Trigger:** Luciano cola boleto(s) de condomínio no chat
2. **Leitura:** Motor lê fichas atualizadas no Google Drive
3. **Extração:** Extrai rubricas do boleto com valores
4. **Classificação:** Classifica rubricas pela tabela de regras
5. **Cálculo:** Calcula valores para locador e locatário em código
6. **Geração:** Preenche templates de card
7. **Memória:** Grava registro no Notion
8. **Entrega:** Retorna cards + pendências (se houver)

## Integração com MCP (Google Workspace)

### Disponibilidade

O MCP `google-workspace` está configurado no `~/.config/kilo/kilo.json` com credenciais OAuth do usuário `lscheffel@gmail.com`. Ele expõe ferramentas para Google Drive, Docs, Sheets, Gmail, Calendar, etc.

### Como agents consomem o MCP

**Kilo (sessão principal):** Tem acesso direto às ferramentas MCP. Quando o contexto envolve Google Drive, Kilo usa automaticamente as ferramentas `search_drive_files`, `get_drive_file_content`, `read_sheet_values`, etc.

**Subagentes (`task` tool):** Não herdam MCPs automaticamente. Para usar dados do Google Drive, o fluxo correto é:

1. **Kilo extrai** os dados necessários via MCP (ex: lê planilha de fichas)
2. **Kilo delega** ao subagente com os dados já em contexto
3. **Subagente processa** (classificação, cálculo, geração de cards)

### Ferramentas MCP relevantes para CobraClau

| Ferramenta | Uso no projeto |
|---|---|
| `search_drive_files` | Buscar fichas, boletos, planilhas no Drive |
| `get_drive_file_content` | Ler conteúdo de arquivos (PDFs, Docs, planilhas) |
| `read_sheet_values` | Ler dados de planilhas Google Sheets |
| `modify_sheet_values` | Atualizar valores em planilhas |
| `create_spreadsheet` | Criar novas planilhas de controle |
| `create_drive_folder` | Organizar pastas de cobrança |

### Regra de uso

> **Sempre que o trabalho envolver dados do Google Drive, Kilo deve acessar via MCP primeiro e repassar os dados já extraídos aos subagentes.** Nunca assumir que subagentes têm acesso direto ao Drive.

## Regras de Classificação

### Decision Tree
```
Rubrica extraída do boleto
├── Está na tabela de regras?
│   ├── Sim → Aplica classificação da tabela
│   └── Não → Compara com Lei 8.245/91 arts 22-23
│       ├── Critério claro → Classifica e sugere adicionar à tabela
│       └── Ambíguo → Marca como PENDENTE DE REVISÃO
└── Soma no lado correto: locador ou locatário
```

### Responsabilidades
- **Locador (Lei 8.245/91 art. 22):** Despesas extraordinárias, estruturais
- **Locatário (Lei 8.245/91 art. 23):** Despesas ordinárias, de rotina

## Padrões de Código

- Use Conventional Commits
- Todo cálculo monetário deve ser determinístico e auditável
- Testes em `test/`
- Documentação em `docs/`

## Comandos Importantes

```bash
# Não há comandos de build/execução neste projeto
# O motor roda como Claude Project com conectores
```

## Restrições e Regras

### 🔴 Crítico
- **Cálculo determinístico:** Todo valor numérico sai de função testável
- **Pendências explícitas:** Rubrica sem regra clara = pendência, nunca decisão silenciosa
- **Revisão humana:** Cards com pendência nunca são marcados como "prontos"

### 🟡 Médio
- **Tabela de regras:** Toda rubrica nova deve passar pela tabela antes de classificação
- **Memória de cálculo:** Todo fechamento deve ser registrado no Notion

### 🟢 Baixo
- **Templates:** Seguir padrão definido nos templates de card
- **Legislação:** Referenciar base legal sempre que possível

## Skills Recomendadas

- `adr-generator` — para gerenciar Architecture Decision Records
- `writing-plans` — para planejamento de implementação
- `data-modeling` — para modelagem de dados da planilha
- `documentation` — para manutenção de documentação

## Contato

- **Responsável:** Luciano
- **Projeto correlato:** ADR-LOC-001 (app de gestão de locação)