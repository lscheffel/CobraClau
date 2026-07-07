# Template de Card - Locatário

```
Imóvel: {endereço}
Referência: {mês/ano}

Aluguel: R$ {valor_aluguel}
Condomínio (rubricas ordinárias, responsabilidade do locatário): R$ {valor_condominio_locatario}
IPTU: R$ {valor_iptu, se aplicável ao contrato}

Total do mês: R$ {total}
Vencimento: {data}

Observações: {pendências, se houver}
```

## Instruções de Preenchimento

1. **{endereço}**: Endereço completo do imóvel conforme ficha de contrato
2. **{mês/ano}**: Mês e ano de referência da cobrança (ex: Julho/2026)
3. **{valor_aluguel}**: Valor do aluguel base conforme contrato
4. **{valor_condominio_locatario}**: Soma das rubricas ordinárias do boleto de condomínio que são responsabilidade do locatário
5. **{valor_iptu}**: Valor do IPTU, se o contrato estabelecer que é responsabilidade do locatário
6. **{total}**: Soma de todos os valores acima
7. **{data}**: Data de vencimento do pagamento
8. **{pendências}**: Lista de rubricas que ficaram pendentes de classificação (opcional)

## Regras de Cálculo

- Todo valor numérico deve ser resultado de cálculo determinístico
- Nunca usar valores estimados ou arredondados
- Rubricas sem classificação clara devem ser listadas em observações