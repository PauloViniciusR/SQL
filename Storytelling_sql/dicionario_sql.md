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
| `julianday()` | Transforma uma data em numero para calcular diferenca entre datas. | `julianday(data_entrega) - julianday(data_estimada)` |
| `CASE` | Cria uma regra condicional, parecida com `IF/ELSE`. | `CASE WHEN atraso > 0 THEN "Atrasado" ELSE "No prazo" END` |
| `WHEN` | Define a condicao dentro do `CASE`. | `WHEN order_delivered_customer_date > order_estimated_delivery_date` |
| `THEN` | Define o resultado quando a condicao do `WHEN` e verdadeira. | `THEN "Atrasado"` |
| `ELSE` | Define o resultado caso nenhuma condicao anterior seja verdadeira. | `ELSE "No prazo ou adiantado"` |
| `END` | Finaliza o bloco `CASE`. | `END AS status_entrega` |
| `AS` | Da um apelido para uma coluna ou calculo. | `COUNT(*) AS qtd_pedidos` |
| `GROUP BY` | Agrupa os dados por uma ou mais colunas. | `GROUP BY status_entrega` |
| `JOIN` | Junta dados de duas tabelas. | `JOIN order_reviews r` |
| `ON` | Define a regra de ligacao entre tabelas no `JOIN`. | `ON o.order_id = r.order_id` |
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
| Experiencia ruim | `review_score <= 2` | Notas 1 e 2 indicam possivel insatisfacao. |
| Experiencia positiva | `review_score >= 4` | Notas 4 e 5 indicam possivel satisfacao. |

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
