# Execution Contract - ADR-COB-001

> Contrato obrigatório que valida se todos os artefatos necessários estão presentes antes da execução.

---

## Identificação

| Campo | Valor |
|-------|-------|
| ADR | docs/adr/ADR-COB-001.md |
| Blueprint | docs/adr/ADR-COB-001-BP.md |
| TODO | docs/adr/ADR-COB-001-TODO.md |
| Data de geração | 2026-07-07 |
| Responsável | Kilo (Agente de IA) |

---

## Artefatos

| Artefato | Path | Status | Coerente |
|----------|------|--------|----------|
| ADR | docs/adr/ADR-COB-001.md | ✅ Aceito | ✅ Coerente |
| Blueprint | docs/adr/ADR-COB-001-BP.md | ✅ Existe | ✅ Coerente |
| TODO | docs/adr/ADR-COB-001-TODO.md | ✅ Existe | ✅ Coerente |

### Validação de Coerência

- [x] ADR contém seção "Decisão" preenchida
- [x] Blueprint contém tarefas documentadas
- [x] TODO contém tarefas com estados
- [x] Tarefas do Blueprint existem no TODO
- [x] Dependências no TODO são consistentes com Blueprint

---

## Ambiente

| Campo | Valor |
|-------|-------|
| Branch atual | main |
| Workspace limpo | ✅ Sim |
| Commit HEAD | f964ada |
| Diretório de trabalho | /home/loupan/projetosVS/CobraClau |

### Validação do Ambiente

- [x] Sem alterações não commitadas (git status limpo)
- [x] Todos os arquivos impactados existem no workspace
- ⚠️ Branch é main (sem PR aberto) - requer confirmação

---

## Arquivos Impactados

| Arquivo | Tipo de Mudança | Skill Relacionada |
|---------|-----------------|-------------------|
| docs/architecture/lei-inquilinato-8245-91.md | Referência | documentation |
| docs/architecture/codigo-civil-locacao.md | Referência | documentation |
| templates/card-locatario.md | Template | writing-plans |
| templates/card-locador.md | Template | writing-plans |
| templates/regras-rubrica.md | Configuração | data-modeling |
| templates/instrucoes-motor.md | Instruções | implementation |

---

## Critérios de Aceite

| # | Critério | Verificável |
|---|----------|-------------|
| 1 | Planilha de fichas criada no Google Drive | ✅ Arquivo existe no Drive |
| 2 | Tabela de regras populada com rubricas comuns | ✅ Pelo menos 15 rubricas |
| 3 | Templates de card aprovados por Luciano | ✅ Confirmação do usuário |
| 4 | Conector Google Drive testado | ✅ Leitura confirmada |
| 5 | Conector Notion testado | ✅ Escrita confirmada |
| 6 | Motor gera card de teste válido | ✅ Card gerado corretamente |
| 7 | Memória gravada no Notion | ✅ Registro confirmado |

---

## Critérios de Rollback

| # | Critério | Critério de Ativação |
|---|----------|---------------------|
| 1 | Reverter criação de planilha | Se Drive não estiver acessível |
| 2 | Reverter criação de database Notion | Se Notion não estiver acessível |
| 3 | Reverter templates | Se aprovados incorretamente |
| 4 | Reverter configurações | Se conectores falharem |

---

## Validação Final

- [x] Todos os campos obrigatórios estão preenchidos
- [x] Todos os artefatos existem e estão coerentes
- [x] Ambiente está limpo e pronto para execução
- [x] Critérios de aceite estão definidos e verificáveis
- [x] Critérios de rollback estão definidos
- [x] Contrato aprovado pelo agente executor

---

## ✅ Status da ADR

**A ADR-COB-001 foi alterada para status "Aceito".**

A implementação pode prosseguir conforme planejado.

---

## Assinatura

| Campo | Valor |
|-------|-------|
| Contrato validado em | 2026-07-07T19:44:19-03:00 |
| Status | ✅ Aprovado |
| Próximo passo | Iniciar Execution Loop (Workflow 4) |