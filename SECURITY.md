# Política de Segurança

## Versões Suportadas

| Versão | Suportado          |
|--------|--------------------|
| 1.0.x  | :white_check_mark: |
| < 1.0  | :x:                |

## Reportando Vulnerabilidades

**Por favor, NÃO reporte vulnerabilidades de segurança publicamente.**

Em vez disso, entre em contato diretamente com o responsável pelo projeto.

### Contato

- **Responsável:** Luciano
- **Email:** [inserir email de contato]

### O que incluir no relatório

- Descrição do tipo de vulnerabilidade
- Etapas para reproduzir o problema
- Possíveis impactos
- Sugestões de correção (se houver)

### Processo

1. Você reporta a vulnerabilidade de forma privada
2. Eu confirmo o recebimento em até 48 horas
3. Eu avalio a gravidade e impacto
4. Eu trabalho na correção
5. Eu lanço a correção e credito você (se desejar)

## Práticas de Segurança

### Dados Sensíveis

Este projeto lida com dados financeiros de terceiros (locadores e locatários). Por isso:

- **Nunca** armazene dados de pagamento (CPF, chaves PIX, etc.) no repositório
- **Nunca** exponha valores financeiros em logs ou commits
- **Use** variáveis de ambiente para credenciais de API
- **Valide** todos os dados de entrada antes de processar

### Credenciais

- Google Drive API: armazenar em variáveis de ambiente
- Notion API: armazenar em variáveis de ambiente
- Claude API: armazenar em variáveis de ambiente

### Boas Práticas

- Não commitar `.env` ou arquivos com credenciais
- Usar least privilege principle para APIs
- Rotacionar chaves periodicamente
- Monitorar acessos às APIs

## Agradecimentos

Agradecemos a todos que ajudam a manter este projeto seguro.