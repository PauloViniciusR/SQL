# Dicionario SQL

Consulta rapida dos comandos mais usados nos notebooks de SQL.

## Comandos basicos

| Comando | Para que serve | Exemplo |
|---|---|---|
| `SELECT` | Escolher colunas ou calculos. | `SELECT nome, preco` |
| `FROM` | Indicar a tabela. | `FROM pedidos` |
| `WHERE` | Filtrar linhas. | `WHERE status = "delivered"` |
| `AND` | Combinar filtros. | `WHERE status = "delivered" AND valor > 100` |
| `ORDER BY` | Ordenar resultado. | `ORDER BY valor DESC` |
| `LIMIT` | Limitar quantidade de linhas. | `LIMIT 10` |

## Agregacoes

| Funcao | Para que serve | Exemplo |
|---|---|---|
| `COUNT(*)` | Contar registros. | `COUNT(*) AS qtd_pedidos` |
| `SUM()` | Somar valores. | `SUM(valor)` |
| `AVG()` | Calcular media. | `AVG(review_score)` |
| `MIN()` | Menor valor. | `MIN(preco)` |
| `MAX()` | Maior valor. | `MAX(preco)` |
| `ROUND()` | Arredondar numero. | `ROUND(AVG(valor), 2)` |

## Agrupamento

```sql
SELECT
    status,
    COUNT(*) AS qtd_pedidos
FROM pedidos
GROUP BY status;
```

| Comando | Para que serve |
|---|---|
| `GROUP BY` | Agrupa linhas para calcular indicadores. |
| `HAVING` | Filtra depois do agrupamento. |

Exemplo com `HAVING`:

```sql
SELECT
    estado,
    COUNT(*) AS qtd_pedidos
FROM clientes
GROUP BY estado
HAVING COUNT(*) >= 500;
```

## Regras com CASE

Use `CASE` para criar classificacoes.

```sql
SELECT
    CASE
        WHEN review_score <= 2 THEN "Ruim"
        WHEN review_score >= 4 THEN "Boa"
        ELSE "Neutra"
    END AS tipo_avaliacao
FROM order_reviews;
```

## Joins

Use `JOIN` para cruzar tabelas.

```sql
SELECT
    o.order_id,
    r.review_score
FROM orders o
JOIN order_reviews r
    ON o.order_id = r.order_id;
```

| Tipo | Uso |
|---|---|
| `JOIN` | Retorna apenas registros com correspondencia nas duas tabelas. |
| `LEFT JOIN` | Mantem todos os registros da primeira tabela. |

## Datas no SQLite

No SQLite, `julianday()` ajuda a calcular diferenca entre datas.

```sql
julianday(order_delivered_customer_date) - julianday(order_estimated_delivery_date)
```

Na analise de atraso:

| Resultado | Significado |
|---:|---|
| `> 0` | Entregou depois do prazo. |
| `= 0` | Entregou na data prometida. |
| `< 0` | Entregou antes do prazo. |

## Exemplo da analise de atraso

```sql
SELECT
    CASE
        WHEN order_delivered_customer_date > order_estimated_delivery_date
            THEN "Atrasado"
        ELSE "No prazo ou adiantado"
    END AS status_entrega,
    COUNT(*) AS qtd_pedidos
FROM orders
WHERE order_status = "delivered"
  AND order_delivered_customer_date IS NOT NULL
  AND order_estimated_delivery_date IS NOT NULL
GROUP BY status_entrega;
```

Regra principal: se a data entregue e maior que a data estimada, o pedido atrasou.
