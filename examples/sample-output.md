# Sample Output

This fictional example shows the structure expected after the document analysis and Google Sheets append step.

## Extracted JSON

```json
{
  "data_fatura": "2026-05-01",
  "fornecedor": "Example Supplier Ltd.",
  "valor": 150.75,
  "moeda": "BRL",
  "impostos": 12.30,
  "numero_fatura": "INV-EXAMPLE-001",
  "categoria": "Services",
  "tipo": "corporativo",
  "metodo_pagamento": "credit_card",
  "nome_sugerido_arquivo": "2026-05-01_Example_Supplier_150-75.pdf"
}
```

## Google Sheets Row

| Column | Example value |
| --- | --- |
| Data da Fatura | `2026-05-01` |
| Fornecedor | `Example Supplier Ltd.` |
| Valor | `150.75` |
| Moeda | `BRL` |
| Impostos | `12.30` |
| Número da Fatura | `INV-EXAMPLE-001` |
| Categoria | `Services` |
| Corporativo ou Pessoal | `corporativo` |
| Método de Pagamento | `credit_card` |
| URL do Arquivo | `https://drive.google.com/example-placeholder` |
| E-mail de Origem | `supplier@example.com` |

Use fictional values only when creating examples or screenshots.
