# SQL para Analise de Dados

Este repositorio reune estudos de SQL com Python e uma analise pratica de storytelling usando dados reais anonimizados da Olist.

A trilha tem duas partes:

- `Sql_sintx`: fundamentos de SQL com bases de estudo.
- `Storytelling_sql`: analise de pedidos, entregas e avaliacoes de clientes.

## Estrutura

```text
.
├── Sql_sintx
│   ├── Base
│   ├── data
│   ├── dicionario_sql.md
│   └── README.md
│
└── Storytelling_sql
    ├── bases
    ├── data
    │   └── vendas_db.db
    ├── notebook
    │   ├── 1. analise.ipynb
    │   ├── 2. create_db.ipynb
    │   ├── analise_base_Sql.ipynb
    │   ├── atraso.ipynb
    │   └── avaliacao.ipynb
    └── png
```

## 1. Fundamentos de SQL

Pasta: `Sql_sintx`

Notebooks em ordem de estudo:

| Notebook | Tema |
|---|---|
| `1. sql.ipynb` | `SELECT`, aliases e `DISTINCT` |
| `2. where.ipynb` | Filtros com `WHERE` |
| `3. group by.ipynb` | Agregacoes |
| `4. having.ipynb` | Filtros em grupos |
| `5. case.ipynb` | Regras com `CASE` |
| `6. subquery.ipynb` | Subqueries |
| `7. in and like.ipynb` | `IN` e `LIKE` |
| `8. join.ipynb` | `JOIN` |
| `9. join2.ipynb` | Joins multiplos |
| `10. union e fulljoin.ipynb` | `UNION` e simulacao de `FULL JOIN` |

O arquivo `Sql_sintx/dicionario_sql.md` funciona como consulta rapida dos comandos principais.

## 2. Storytelling com SQL

Pasta: `Storytelling_sql`

O foco da analise e responder:

> Se a logistica entrega bem na media, ainda existem clientes impactados por atraso?

Notebook principal:

```text
Storytelling_sql/notebook/analise_base_Sql.ipynb
```

## Principais achados

| Indicador | Resultado |
|---|---:|
| Pedidos entregues analisados | 96.470 |
| Media geral em relacao ao prazo | -11,18 dias |
| Pedidos atrasados | 7.826 |
| Percentual de pedidos atrasados | 8,11% |
| Nota media sem atraso | 4,29 |
| Nota media com atraso | 2,57 |
| Notas ruins sem atraso | 9,23% |
| Notas ruins com atraso | 54,03% |

Leitura principal:

> A operacao parece boa na media, mas a experiencia muda quando a promessa de entrega e quebrada.

## Storytelling da apresentacao

| Ato | Mensagem |
|---|---|
| Preparacao | A maioria dos pedidos chega no prazo ou antes. |
| Conflito | A media esconde clientes atrasados com pior avaliacao. |
| Solucao | Melhorar comunicacao e tratamento quando houver atraso ou risco de atraso. |

Os graficos para PPT sao gerados pelo notebook e salvos em:

```text
Storytelling_sql/png
```

## Regra de atraso

A convencao usada na analise:

```sql
julianday(order_delivered_customer_date) - julianday(order_estimated_delivery_date)
```

| Resultado | Significado |
|---:|---|
| `> 0` | Pedido atrasado |
| `= 0` | Pedido entregue na data prometida |
| `< 0` | Pedido entregue antes do prazo |

## Como executar

Crie o ambiente:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

Instale as dependencias:

```bash
pip install pandas matplotlib jupyter openpyxl
```

Abra o Jupyter:

```bash
jupyter notebook
```

Execute:

```text
Sql_sintx/Base/1. sql.ipynb
...
Sql_sintx/Base/10. union e fulljoin.ipynb
Storytelling_sql/notebook/analise_base_Sql.ipynb
```

## Tecnologias

- SQL
- SQLite
- Python
- Pandas
- Matplotlib
- Jupyter Notebook
