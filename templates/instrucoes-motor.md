# Instruções do Motor de Cálculo

## Visão Geral

Este documento define as regras para o motor de cálculo do sistema de cobrança mensal de locação.

## Princípio Fundamental

**REGRA INEGOCIÁVEL:** Nenhum valor monetário em um card sai de texto gerado livremente pela IA. Todo número é resultado de cálculo determinístico (soma/rateio das rubricas do boleto conforme a tabela de regras).

## Fluxo de Processamento

### 1. Leitura de Dados
- Ler fichas de contrato no Google Drive
- Ler tabela de regras de rubrica
- Ler boleto de condomínio colado pelo usuário

### 2. Extração de Rubricas
- Extrair cada rubrica do boleto com seu valor
- Formato esperado: `nome_da_rubrica: R$ valor`

### 3. Classificação de Rubricas
Para cada rubrica extraída:
1. Verificar se existe na tabela de regras
2. Se existir, aplicar a classificação indicada
3. Se não existir, comparar com Lei 8.245/91 arts. 22-23
4. Se a classificação não for clara, marcar como pendência

### 4. Cálculo de Valores

#### Para Locatário:
```python
def calcular_locatario(rubricas, regras):
    total = 0
    pendencias = []
    
    for rubrica in rubricas:
        classe = regras.get(rubrica.nome)
        if classe is None:
            pendencias.append(rubrica)
            continue
        
        if classe == "locatário" or classe == "ordinária":
            total += rubrica.valor
    
    return total, pendencias
```

#### Para Locador:
```python
def calcular_locador(rubricas, regras):
    total = 0
    pendencias = []
    
    for rubrica in rubricas:
        classe = regras.get(rubrica.nome)
        if classe is None:
            pendencias.append(rubrica)
            continue
        
        if classe == "locador" or classe == "extraordinária":
            total += rubrica.valor
    
    return total, pendencias
```

### 5. Geração de Cards
- Preencher template de card do locatário
- Preencher template de card do locador
- Incluir pendências em observações

### 6. Gravação em Memória
- Gravar registro na aba Historico do Google Sheets
- Schema: `docs/architecture/schema-historico.md`
- Um registro por contrato por mês

## Regras de Pendência

### Quando Marcar Pendência
- Rubrica não encontrada na tabela de regras
- Classificação ambígua (não clara pela Lei 8.245/91)
- Contrato com exceção não documentada na ficha

### Formato da Pendência
```
PENDÊNCIA: {nome_da_rubrica} - {motivo}
- Valor: R$ {valor}
- Possível classificação: {sugestão}
- Ação necessária: {o que fazer}
```

### Regra Crítica
**NUNCA** marcar um card como "pronto para enviar" se houver pendência aberta naquele contrato.

## Anti-patterns

### ❌ ERRADO
```
"Somando os valores do boleto, o locatário deve pagar aproximadamente R$ 340"
```

### ✅ CORRETO
```python
locatario_total = sum(r.valor for r in rubricas if regras[r.nome] == "locatario")
# card usa {{locatario_total}} calculado, não estimado
```

## Referências

- Lei 8.245/91 arts. 22-23
- Código Civil (dispositivos aplicáveis)
- Tabela de regras de rubrica
- Fichas de contrato no Google Drive