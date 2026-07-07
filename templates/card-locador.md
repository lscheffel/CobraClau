# Template de Card - Locador

```
Imóvel: {endereço}
Referência: {mês/ano}

Repasse de aluguel: R$ {valor_repasse}
Despesas extraordinárias do condomínio (responsabilidade do locador): R$ {valor_condominio_locador}

Total líquido: R$ {total}

Observações: {pendências, se houver}
```

## Instruções de Preenchimento

1. **{endereço}**: Endereço completo do imóvel conforme ficha de contrato
2. **{mês/ano}**: Mês e ano de referência da cobrança (ex: Julho/2026)
3. **{valor_repasse}**: Valor do aluguel a ser repassado ao locador
4. **{valor_condominio_locador}**: Soma das rubricas extraordinárias do boleto de condomínio que são responsabilidade do locador
5. **{total}**: Valor líquido a ser repassado ao locador (repasse - despesas extraordinárias, se aplicável)
6. **{pendências}**: Lista de rubricas que ficaram pendentes de classificação (opcional)

## Regras de Cálculo

- Todo valor numérico deve ser resultado de cálculo determinístico
- Nunca usar valores estimados ou arredondados
- Rubricas sem classificação clara devem ser listadas em observações
- O total líquido pode ser negativo se as despesas extraordinárias superarem o aluguel