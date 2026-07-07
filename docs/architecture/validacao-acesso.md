# Validação de Acesso ao Google Drive

> Documentação da validação de leitura da planilha "Fichas-Locacao" via conector.

---

## Resumo da Validação

| Campo | Valor |
|-------|-------|
| Data da validação | 2026-07-07 |
| Planilha testada | Fichas-Locacao |
| URL | https://docs.google.com/spreadsheets/d/1r1-EzmIl1LVgLi0NMq2uxOAjBenWcX--2C5TbhLEeK4/ |
| Método de acesso | webfetch com export CSV |
| Status | ✅ Aprovado |

---

## Métodos de Acesso Testados

### 1. Export CSV via URL

**URL:**
```
https://docs.google.com/spreadsheets/d/{SHEET_ID}/export?format=csv
```

**Resultado:** ✅ Funcional para aba principal (Contratos)

**Limitação:** Não retorna abas secundárias (Regras-Rubrica)

### 2. Google Visualization API (gviz)

**URL:**
```
https://docs.google.com/spreadsheets/d/{SHEET_ID}/gviz/tq?tqx=out:csv&sheet={SHEET_NAME}
```

**Resultado:** ✅ Funcional para todas as abas

**Vantagem:** Permite especificar a aba pelo nome

---

## Abas Validadas

### Aba Contratos

| Campo | Resultado |
|-------|-----------|
| Total de linhas | 10 contratos |
| Colunas | 44 colunas |
| Dados obrigatórios | ✅ Todos preenchidos |
| Formato de dados | ✅ Válido |

### Aba Regras-Rubrica

| Campo | Resultado |
|-------|-----------|
| Total de linhas | 20 rubricas |
| Colunas | 6 colunas |
| Classificações | ✅ Válidas |
| Base legal | ✅ Documentada |

---

## Método Recomendado para o Motor

Para o motor de cálculo, recomenda-se usar o método **Google Visualization API (gviz)** porque:

1. Permite acessar qualquer aba pelo nome
2. Retorna dados em formato CSV padronizado
3. Não requer autenticação para planilhas públicas
4. É mais confiável que o export padrão

**Exemplo de uso:**
```python
import requests

SHEET_ID = "1r1-EzmIl1LVgLi0NMq2uxOAjBenWcX--2C5TbhLEeK4"

# Ler aba Contratos
url_contratos = f"https://docs.google.com/spreadsheets/d/{SHEET_ID}/gviz/tq?tqx=out:csv&sheet=Contratos"
response = requests.get(url_contratos)
contratos = response.text

# Ler aba Regras-Rubrica
url_regras = f"https://docs.google.com/spreadsheets/d/{SHEET_ID}/gviz/tq?tqx=out:csv&sheet=Regras-Rubrica"
response = requests.get(url_regras)
regras = response.text
```

---

## Restrições

1. **Planilha deve ser pública** ou compartilhada com link de visualização
2. **Limite de taxa**: Google pode limitar requisições excessivas
3. **Formato CSV**: Dados retornados como texto, não como objetos estruturados

---

## Próximos Passos

1. ✅ A3.1: Testar leitura via Drive (concluído)
2. ⬜ A3.2: Confirmar se conector lê Sheets nativamente
3. ⬜ A3.3: Testar escrita no Notion

---

## Conclusão

O acesso à planilha via Google Visualization API é **funcional e confiável**. O método pode ser utilizado pelo motor de cálculo para ler dados de contratos e regras de rubrica.