# Contribuindo para o CobraClau

## Como Contribuir

1. Fork o repositório
2. Crie uma branch para sua feature: `git checkout -b feature/nome-da-feature`
3. Faça suas mudanças seguindo os padrões do projeto
4. Commite suas mudanças: `git commit -m "feat: descrição da mudança"`
5. Push para a branch: `git push origin feature/nome-da-feature`
6. Abra um Pull Request

## Padrões de Commit

Use Conventional Commits:

- `feat:` nova funcionalidade
- `fix:` correção de bug
- `docs:` documentação
- `style:` formatação (não afeta o código)
- `refactor:` refatoração de código
- `test:` adição ou correção de testes
- `chore:` tarefas de manutenção

## Regras Específicas do Projeto

### 🔴 Crítico

1. **Cálculo Determinístico**
   - Todo valor monetário deve ser resultado de cálculo em código
   - Nunca usar valores estimados ou arredondados "de cabeça"
   - Todo cálculo deve ser auditável e reproduzível

2. **Pendências Explícitas**
   - Rubricas sem classificação clara devem ser marcadas como pendência
   - Nunca decidir classificação silenciosamente
   - Cards com pendência nunca devem ser marcados como "prontos"

### 🟡 Médio

1. **Tabela de Regras**
   - Toda rubrica nova deve passar pela tabela antes de classificação
   - Atualizar a tabela sempre que uma nova rubrica for validada

2. **Memória de Cálculo**
   - Todo fechamento deve ser registrado no Notion
   - Manter histórico consultável para auditoria

## Estrutura de Código

```
src/
├── domain/        # Regras de negócio e entidades
├── application/   # Casos de uso
├── infrastructure/# Adaptadores externos (Drive, Notion)
└── interfaces/    # Templates e apresentação
```

## Testes

- Mantenha cobertura ≥ 80% para código de negócio
- Testes em `test/`
- Execute antes de submeter: `npm test` (quando aplicável)

## Code Review

- Pelo menos 1 aprovação antes de merge
- CI deve estar verde
- Sem conflitos com a branch principal

## Documentação

- Atualize o README se mudar a estrutura ou uso
- Atualize o CHANGELOG com suas mudanças
- Documente decisões arquiteturais em ADRs

## Perguntas?

Consulte a documentação em `docs/` ou abra uma issue.