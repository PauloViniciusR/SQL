# Dicionario SQL da Analise

Este dicionario resume os principais comandos e regras usados na analise de atrasos de entrega e experiencia do cliente.

## Comandos e funcoes SQL

| Comando / Funcao | O que faz | Exemplo |
|---|---|---|
| `SELECT` | Escolhe quais colunas ou calculos serao exibidos. | `SELECT COUNT(*) AS total_pedidos` |
| `FROM` | Indica de qual tabela os dados serao buscados. | `FROM orders` |
| `WHERE` | Filtra os registros da analise. | `WHERE order_status = "delivered"` |
| `AND` | Adiciona mais uma condicao ao filtro. | `AND order_delivered_customer_date IS NOT NULL` |
| `IS NOT NULL` | Garante que o campo nao esta vazio. | `order_estimated_delivery_date IS NOT NULL` |
| `COUNT(*)` | Conta a quantidade de linhas ou registros. | `COUNT(*) AS qtd_pedidos` |
| `AVG()` | Calcula a media de uma coluna ou expressao. | `AVG(review_score)` |
| `ROUND()` | Arredonda um numero. | `ROUND(AVG(review_score), 2)` |
| `julianday()` | Transforma uma data em numero para calcular diferenca entre datas. | `julianday(order_delivered_customer_date) - julianday(order_estimated_delivery_date)` |
| `CASE` | Cria uma regra condicional, parecida com `IF/ELSE`. | `CASE WHEN atraso > 0 THEN "Atrasado" ELSE "No prazo" END` |
| `WHEN` | Define a condicao dentro do `CASE`. | `WHEN order_delivered_customer_date > order_estimated_delivery_date` |
| `THEN` | Define o resultado quando a condicao do `WHEN` e verdadeira. | `THEN "Atrasado"` |
| `ELSE` | Define o resultado caso nenhuma condicao anterior seja verdadeira. | `ELSE "No prazo ou adiantado"` |
| `END` | Finaliza o bloco `CASE`. | `END AS status_entrega` |
| `AS` | Da um apelido para uma coluna ou calculo. | `COUNT(*) AS qtd_pedidos` |
| `GROUP BY` | Agrupa os dados por uma ou mais colunas. | `GROUP BY status_entrega` |
| `SUM()` | Soma valores numericos ou flags criadas com `CASE`. | `SUM(CASE WHEN review_score <= 2 THEN 1 ELSE 0 END)` |
| `MAX()` | Retorna o maior valor de um grupo. | `MAX(review_score)` |
| `MIN()` | Retorna o menor valor de um grupo. | `MIN(review_score)` |
| `JOIN` | Junta dados de duas tabelas. | `JOIN order_reviews r` |
| `ON` | Define a regra de ligacao entre tabelas no `JOIN`. | `ON o.order_id = r.order_id` |
| `LEFT JOIN` | Mantem todos os registros da tabela principal, mesmo sem correspondencia na segunda tabela. | `FROM orders o LEFT JOIN order_reviews r ON o.order_id = r.order_id` |
| `HAVING` | Filtra resultados depois do agrupamento. | `HAVING COUNT(*) >= 500` |
| `ORDER BY` | Ordena o resultado. | `ORDER BY qtd_pedidos DESC` |
| `DESC` | Ordena do maior para o menor. | `ORDER BY pct_atraso DESC` |
| `ASC` | Ordena do menor para o maior. | `ORDER BY pct_atraso ASC` |
| Subquery | Consulta dentro de outra consulta. | `(SELECT COUNT(*) FROM orders)` |

## Apelidos de tabela

| Apelido | Tabela | Por que usamos |
|---|---|---|
| `o` | `orders` | Para escrever menos e indicar que a coluna vem da tabela de pedidos. |
| `r` | `order_reviews` | Para indicar que a coluna vem da tabela de avaliacoes. |
| `c` | `customers` | Para indicar que a coluna vem da tabela de clientes. |
| `i` | `order_items` | Para indicar que a coluna vem da tabela de itens do pedido. |
| `p` | `products` | Para indicar que a coluna vem da tabela de produtos. |

Exemplo:

```sql
FROM orders o
JOIN order_reviews r
    ON o.order_id = r.order_id
```

Nesse caso:

| Expressao | Significado |
|---|---|
| `o.order_id` | Coluna `order_id` da tabela `orders`. |
| `r.order_id` | Coluna `order_id` da tabela `order_reviews`. |
| `r.review_score` | Coluna `review_score` da tabela `order_reviews`. |

## Regras de negocio da analise

| Regra | Codigo SQL | Interpretacao |
|---|---|---|
| Pedido entregue | `order_status = "delivered"` | Consideramos apenas pedidos que foram entregues. |
| Pedido atrasado | `order_delivered_customer_date > order_estimated_delivery_date` | A entrega real aconteceu depois da data prometida. |
| Pedido no prazo ou adiantado | `order_delivered_customer_date <= order_estimated_delivery_date` | A entrega aconteceu ate a data prometida. |
| Dias de atraso | `julianday(order_delivered_customer_date) - julianday(order_estimated_delivery_date)` | Resultado positivo indica atraso; resultado negativo indica entrega antes do prazo. |
| Experiencia ruim | `review_score <= 2` | Notas 1 e 2 indicam possivel insatisfacao. |
| Experiencia positiva | `review_score >= 4` | Notas 4 e 5 indicam possivel satisfacao. |

## Metricas validadas para o storytelling

Esses numeros foram validados no banco `vendas_db.db`, considerando apenas pedidos entregues e com datas validas.

| Metrica | Resultado | Leitura para negocio |
|---|---:|---|
| Total de pedidos entregues analisados | 96.470 | Base principal da analise de prazo. |
| Media geral vs prazo prometido | -11,18 dias | Na media, a operacao entrega antes do prazo. |
| Pedidos atrasados | 7.826 | Existe um grupo relevante de clientes impactados. |
| Percentual de pedidos atrasados | 8,11% | O problema nao e maioria, mas tem volume suficiente para acao. |
| Nota media com atraso | 2,57 | A experiencia cai fortemente quando o prazo e quebrado. |
| Nota media sem atraso | 4,29 | Entregas no prazo ou adiantadas preservam satisfacao. |
| Notas ruins com atraso | 54,03% | Mais da metade dos atrasados avalia com nota 1 ou 2. |
| Notas ruins sem atraso | 9,23% | O risco de insatisfacao e muito menor sem atraso. |

## Consultas principais

### 1. Media geral de dias em relacao ao prazo

```sql
SELECT
    COUNT(*) AS total_pedidos_entregues,
    ROUND(
        AVG(julianday(order_delivered_customer_date)
          - julianday(order_estimated_delivery_date)),
        2
    ) AS media_dias_vs_prazo
FROM orders
WHERE order_status = "delivered"
  AND order_delivered_customer_date IS NOT NULL
  AND order_estimated_delivery_date IS NOT NULL;
```

Interpretacao: valor negativo significa que, na media, os pedidos chegaram antes do prazo.

### 2. Quantidade e percentual de pedidos atrasados

```sql
SELECT
    CASE
        WHEN order_delivered_customer_date > order_estimated_delivery_date
            THEN "Atrasado"
        ELSE "No prazo ou adiantado"
    END AS status_entrega,
    COUNT(*) AS qtd_pedidos,
    ROUND(
        100.0 * COUNT(*) / (
            SELECT COUNT(*)
            FROM orders
            WHERE order_status = "delivered"
              AND order_delivered_customer_date IS NOT NULL
              AND order_estimated_delivery_date IS NOT NULL
        ),
        2
    ) AS percentual_pedidos
FROM orders
WHERE order_status = "delivered"
  AND order_delivered_customer_date IS NOT NULL
  AND order_estimated_delivery_date IS NOT NULL
GROUP BY status_entrega;
```

Interpretacao: a media geral e boa, mas ela esconde clientes que receberam depois do prazo prometido.

### 3. Impacto do atraso na avaliacao

```sql
SELECT
    CASE
        WHEN o.order_delivered_customer_date > o.order_estimated_delivery_date
            THEN "Atrasado"
        ELSE "No prazo ou adiantado"
    END AS status_entrega,
    COUNT(*) AS qtd_reviews,
    ROUND(AVG(r.review_score), 2) AS nota_media,
    ROUND(
        100.0 * AVG(CASE WHEN r.review_score <= 2 THEN 1 ELSE 0 END),
        2
    ) AS percentual_notas_ruins
FROM orders o
JOIN order_reviews r
    ON o.order_id = r.order_id
WHERE o.order_status = "delivered"
  AND o.order_delivered_customer_date IS NOT NULL
  AND o.order_estimated_delivery_date IS NOT NULL
GROUP BY status_entrega;
```

Interpretacao: essa e a consulta central para provar que atraso nao e apenas problema operacional; e problema de experiencia do cliente.

### 4. Faixas de atraso

```sql
SELECT
    CASE
        WHEN julianday(o.order_delivered_customer_date)
           - julianday(o.order_estimated_delivery_date) <= 0
            THEN "No prazo ou adiantado"
        WHEN julianday(o.order_delivered_customer_date)
           - julianday(o.order_estimated_delivery_date) BETWEEN 1 AND 3
            THEN "Atraso de 1 a 3 dias"
        WHEN julianday(o.order_delivered_customer_date)
           - julianday(o.order_estimated_delivery_date) BETWEEN 4 AND 7
            THEN "Atraso de 4 a 7 dias"
        ELSE "Atraso acima de 7 dias"
    END AS faixa_atraso,
    COUNT(*) AS qtd_pedidos,
    ROUND(AVG(r.review_score), 2) AS nota_media,
    ROUND(
        100.0 * AVG(CASE WHEN r.review_score <= 2 THEN 1 ELSE 0 END),
        2
    ) AS percentual_notas_ruins
FROM orders o
JOIN order_reviews r
    ON o.order_id = r.order_id
WHERE o.order_status = "delivered"
  AND o.order_delivered_customer_date IS NOT NULL
  AND o.order_estimated_delivery_date IS NOT NULL
GROUP BY faixa_atraso
ORDER BY
    CASE faixa_atraso
        WHEN "No prazo ou adiantado" THEN 1
        WHEN "Atraso de 1 a 3 dias" THEN 2
        WHEN "Atraso de 4 a 7 dias" THEN 3
        ELSE 4
    END;
```

Interpretacao: quanto maior o atraso, maior tende a ser o risco de avaliacao ruim.

## Cuidado importante: sinal do atraso

Use sempre esta convencao:

```sql
julianday(order_delivered_customer_date) - julianday(order_estimated_delivery_date)
```

| Resultado | Significado |
|---:|---|
| Maior que 0 | Pedido atrasado. |
| Igual a 0 | Pedido entregue na data prometida. |
| Menor que 0 | Pedido entregue antes do prazo. |

Evite inverter a regra. Se o calculo for `entrega - estimada`, entao atraso e `> 0`, nao `< 0`.

## Exemplo completo

```sql
SELECT
    CASE
        WHEN o.order_delivered_customer_date > o.order_estimated_delivery_date
            THEN "Atrasado"
        ELSE "No prazo ou adiantado"
    END AS status_entrega,
    COUNT(*) AS qtd_pedidos,
    ROUND(AVG(r.review_score), 2) AS nota_media
FROM orders o
JOIN order_reviews r
    ON o.order_id = r.order_id
WHERE o.order_status = "delivered"
  AND o.order_delivered_customer_date IS NOT NULL
  AND o.order_estimated_delivery_date IS NOT NULL
GROUP BY status_entrega;
```

Esse exemplo junta pedidos com avaliacoes, classifica cada entrega como atrasada ou no prazo, conta os pedidos e calcula a nota media por grupo.
