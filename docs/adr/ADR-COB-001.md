# ADR-COB-001: Sistema de cobrança mensal de locação assistido por IA

## Status
Proposto

## Contexto

### Diagnóstico

| Capacidade | Status | Evidence |
|------------|--------|----------|
| Fichas centralizadas de locador/imóvel/locatário | Inexistente | Dados de contrato dispersos; cálculo mensal refeito manualmente a cada boletim |
| Classificação de rubricas do boleto de condomínio (ordinária x extraordinária) | Manual, não documentada | Critério aplicado de memória a cada boleto, sem registro auditável do porquê |
| Emissão de cards de cobrança | Manual | Redigido do zero todo mês, por imóvel |
| Memória histórica de cálculo | Inexistente | Nenhum rastro do que foi cobrado nem de como, em meses anteriores |
| Referência jurídica associada ao cálculo | Implícita | Lei 8.245/91 e Código Civil aplicados de memória, sem link ao cálculo feito |

### Consequências da Lacuna

- Risco de erro em valores enviados a terceiros (locador/locatário) sem trilha de auditoria
- Retrabalho manual mensal: mesmo raciocínio de classificação de rubrica refeito do zero todo mês
- Sem memória, uma contestação de cobrança ("por que esse valor em março?") não tem como ser respondida com precisão
- Não escala: cada contrato novo aumenta o trabalho manual linearmente

## Decisão

Adotar um Project no Claude — workspace com conhecimento persistente e os conectores de Google Drive e Notion já autenticados na conta — separando explicitamente três camadas: (1) dados estruturados de origem, em planilha no Drive, lida ao vivo; (2) conhecimento de apoio estático (legislação e templates de card), mantido como Project Knowledge; (3) motor de execução mensal, acionado por Luciano colando o boleto de condomínio, que classifica rubricas contra uma tabela de regras versionada, calcula em código — não por geração de texto — o valor devido por locador e por locatário, preenche os templates de card e grava o resultado na memória de cálculo no Notion.

O princípio não negociável da decisão: **nenhum valor monetário em um card sai de texto gerado livremente pela IA.** Todo número é resultado de uma função determinística e auditável. A IA decide classificação de rubrica ambígua e redige o card — nunca soma dinheiro "no olho".

### Arquitetura da Solução

```
Fichas (Drive) + Base de conhecimento (legislação/templates)
   → Motor (Claude: classifica rubricas + calcula em código)
   → Cards de cobrança + Memória de cálculo (Notion)
```

### Detalhes da Implementação

**Trigger:** Mensal, sob demanda — Luciano cola o(s) boleto(s) de condomínio no chat do Project (um a um, em lote, ou como CSV/XLSX).

**Jobs:**
1. Ler as fichas atualizadas na planilha do Drive (contratos ativos, o que é cobrado e como, por imóvel)
2. Interpretar o boleto colado e extrair rubricas com seus valores
3. Classificar cada rubrica como ordinária, extraordinária ou chamada extra, contra a tabela de regras (base: Lei 8.245/91 arts. 22-23 + exceções contratuais registradas na ficha)
4. Calcular, em código, o valor de responsabilidade do locador e do locatário por contrato
5. Preencher os templates de card (um para locador, um para locatário, por contrato)
6. Gravar o registro do mês na memória de cálculo no Notion, com rubricas, valores, classificação aplicada e a base usada para cada decisão
7. Sinalizar explicitamente qualquer rubrica sem classificação clara — nunca decidir e enviar em silêncio

## Alternativas Consideradas

### Alternativa A: Gemini Gem
- **Prós**: nativo ao ecossistema Google; grounding rápido em Drive/Docs; simples de configurar
- **Contras**: sem execução de código confiável para o cálculo; write-back em Sheets/Notion exigiria um backend em Apps Script à parte; risco real de cálculo feito "no olho" pela LLM

### Alternativa B: NotebookLM
- **Prós**: excelente para consulta e Q&A sobre fontes longas (legislação, contratos); grounding forte em texto estático
- **Contras**: não é um agente executor — não gera output estruturado nem escreve de volta em lugar nenhum; sem memória de execução mês a mês

### Alternativa C: Claude Project + conectores Google Drive/Notion (Escolhida)
- **Prós**: conectores já autenticados na conta; execução de código nativa para cálculo determinístico e auditável; Project Knowledge para legislação e templates; escreve a memória direto no Notion sem infraestrutura adicional; zero app, zero painel — como pedido
- **Contras**: depende da disponibilidade dos conectores; leitura de Google Sheets (em oposição a Docs) pelo conector do Drive precisa ser validada empiricamente antes de confiar no fluxo

## Consequências

### Positivas
- Cálculo rastreável e reproduzível — código, não geração de texto
- Zero retrabalho de classificação manual de rubrica mês a mês, uma vez a tabela de regras madura
- Memória consultável para disputas e auditoria retroativa
- Nenhuma infraestrutura nova a manter

### Negativas
- Depende de disciplina para manter a planilha de fichas como fonte única da verdade, sempre atualizada
- Regras de rubrica exigem manutenção quando surgir condomínio ou cláusula contratual atípica
- Primeiro mês exige revisão manual de 100% dos cards até a tabela de regras estar madura

### Riscos
- **Risco**: conector do Drive não ler Google Sheets nativamente, apenas Google Docs
  - **Mitigação**: validar isso na Fase A do blueprint, antes de qualquer outra etapa; se não ler nativamente, publicar a planilha como CSV e ler por URL
- **Risco**: rubrica ambígua classificada errado, gerando cobrança indevida
  - **Mitigação**: toda classificação incerta é sinalizada no card e na memória para revisão humana; motor nunca marca um card como "pronto pra enviar" com pendência aberta naquele contrato

## Referências
- Lei nº 8.245/1991 (Lei do Inquilinato), arts. 22 e 23 — despesas de responsabilidade do locador e do locatário
- Código Civil, dispositivos aplicáveis subsidiariamente a locação
- ADR-LOC-001 — app de gestão de locação (projeto correlato, escopo distinto: aplicação com banco local; este ADR cobre um fluxo assistido por chat, sem app)
