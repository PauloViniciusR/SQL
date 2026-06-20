# SQL para Analise de Dados: do Fundamento ao Storytelling

Este repositorio reune estudos e projetos praticos de SQL aplicados a analise de dados com Python. A ideia central e construir uma trilha completa: primeiro dominar a sintaxe essencial de consultas, filtros, agregacoes e joins; depois aplicar esse conhecimento em uma base real de e-commerce para transformar tabelas em uma narrativa analitica.

## A historia por tras do projeto

Uma base de dados so ganha valor quando responde perguntas reais.

Neste repositorio, o caminho comeca em bases controladas, criadas para estudar comandos SQL de forma progressiva. A partir delas, cada notebook resolve uma parte do raciocinio analitico: selecionar dados, filtrar registros, agrupar informacoes, criar regras de negocio, cruzar tabelas e comparar conjuntos.

Depois, essa base tecnica e levada para um contexto mais proximo do mercado: o dataset publico da Olist, com pedidos reais anonimizados de marketplace entre 2016 e 2018. Nesse segundo momento, a analise deixa de ser apenas sintaxe e passa a contar uma historia sobre pedidos, produtos, vendedores, pagamentos e avaliacoes de clientes.

O objetivo nao e apenas executar queries, mas construir uma linha de investigacao:

- Quais tabelas existem e como elas se conectam?
- O que representa um pedido dentro do modelo de dados?
- Um pedido pode ter mais de um item?
- Um item pode ter mais de um pagamento?
- Um mesmo produto pode aparecer associado a mais de um vendedor?
- Como as avaliacoes dos clientes ajudam a interpretar a experiencia de compra?

Essa abordagem aproxima SQL, Pandas e pensamento analitico: antes de buscar respostas prontas, o projeto explora a estrutura dos dados, valida hipoteses e transforma observacoes em contexto.

## Projetos do repositorio

```text
.
├── Sql_sintx
│   ├── Base
│   │   ├── 1. sql.ipynb
│   │   ├── 2. where.ipynb
│   │   ├── 3. group by.ipynb
│   │   ├── 4. having.ipynb
│   │   ├── 5. case.ipynb
│   │   ├── 6. subquery.ipynb
│   │   ├── 7. in and like.ipynb
│   │   ├── 8. join.ipynb
│   │   ├── 9. join2.ipynb
│   │   └── 10. union e fulljoin.ipynb
│   ├── data
│   └── README.md
│
└── Storytelling_sql
    ├── bases
    │   ├── olist_customers_dataset.csv
    │   ├── olist_geolocation_dataset.csv
    │   ├── olist_order_items_dataset.csv
    │   ├── olist_order_payments_dataset.csv
    │   ├── olist_order_reviews_dataset.csv
    │   ├── olist_orders_dataset.csv
    │   ├── olist_products_dataset.csv
    │   ├── olist_sellers_dataset.csv
    │   └── product_category_name_translation.csv
    ├── data
    │   └── vendas_db.db
    ├── png
    │   ├── 01_ato1_preparacao_status_entrega.png
    │   ├── 02_ato2_conflito_queda_nota.png
    │   ├── 03_ato2_conflito_notas_ruins.png
    │   ├── 04_ato3_solucao_faixas_atraso.png
    │   └── 05_ato3_solucao_estados_prioritarios.png
    ├── dicionario_sql.md
    └── notebook
        ├── 1. analise.ipynb
        ├── 2. create_db.ipynb
        ├── 3. base.ipynb
        ├── 4. base2.ipynb
        ├── analise_base_Sql.ipynb
        ├── atraso.ipynb
        └── avaliacao.ipynb
```

## 1. Fundamentos de SQL com Python

Pasta: `Sql_sintx`

Este modulo funciona como uma trilha de aprendizagem de SQL usando SQLite, Pandas e Jupyter Notebook. Os notebooks foram organizados em ordem crescente de complexidade:

| Notebook | Tema | Objetivo |
|---|---|---|
| `1. sql.ipynb` | `SELECT`, aliases e `DISTINCT` | Conectar ao banco e executar as primeiras consultas |
| `2. where.ipynb` | `WHERE` | Filtrar registros com condicoes logicas |
| `3. group by.ipynb` | Agregacoes | Criar indicadores com `COUNT`, `MIN`, `MAX` e `AVG` |
| `4. having.ipynb` | `HAVING` | Filtrar resultados agregados |
| `5. case.ipynb` | `CASE` | Criar regras condicionais dentro da query |
| `6. subquery.ipynb` | Subqueries | Resolver consultas em etapas |
| `7. in and like.ipynb` | `IN` e `LIKE` | Trabalhar com listas e padroes de texto |
| `8. join.ipynb` | `JOIN` | Relacionar tabelas por chave |
| `9. join2.ipynb` | Joins multiplos | Consolidar dados de tabelas diferentes |
| `10. union e fulljoin.ipynb` | `UNION` e `FULL JOIN` | Combinar linhas e simular full join no SQLite |

As bases usadas nesse modulo sao ficticias e servem para praticar conceitos com baixo risco, antes de aplicar a logica em dados maiores.

## 2. Storytelling com dados de e-commerce

Pasta: `Storytelling_sql`

Este projeto utiliza dados reais anonimizados da Olist, marketplace brasileiro com cerca de 100 mil pedidos entre 2016 e 2018. A analise percorre diferentes dimensoes do negocio:

- pedidos e status de compra;
- itens vendidos por pedido;
- pagamentos associados aos pedidos;
- produtos e categorias;
- vendedores;
- clientes;
- avaliacoes dos consumidores.

O notebook `1. analise.ipynb` conduz uma exploracao guiada, partindo da leitura dos arquivos CSV, passando pela inspecao das estruturas e chegando a investigacoes mais especificas, como pedidos com multiplos itens, itens com mais de uma forma de pagamento e distribuicao das notas de avaliacao.

O notebook `2. create_db.ipynb` registra praticas de criacao, insercao, atualizacao e remocao de dados em SQLite usando Python, reforcando a ponte entre DataFrames e bancos relacionais.

O notebook principal para apresentacao executiva e o `analise_base_Sql.ipynb`. Ele consolida a analise de atraso de entrega e impacto na avaliacao do cliente usando SQL, gerando tabelas e graficos prontos para uma apresentacao em PPT.

### Pergunta de negocio

A logistica afirma que atraso nao parece ser um problema relevante porque, em media, os pedidos chegam cerca de 11 dias antes do prazo. A analise testa essa afirmacao com uma pergunta mais precisa:

> Se a media geral e boa, ainda assim existem clientes impactados por atraso? E esse atraso muda a experiencia do cliente?

### Estrutura do storytelling

A apresentacao foi organizada em 3 atos:

| Ato | Papel na historia | Mensagem principal |
|---|---|---|
| Preparacao | Mostrar o contexto e reconhecer o ponto da logistica. | A operacao parece boa na media: 91,89% dos pedidos chegam no prazo ou antes. |
| Conflito | Mostrar o que a media esconde. | Existem 7.826 pedidos atrasados, e a nota media cai de 4,29 para 2,57 quando o atraso acontece. |
| Solucao | Transformar evidencia em recomendacao. | O objetivo nao e prometer atraso zero, mas criar comunicacao e tratamento para pedidos com atraso ou risco de atraso. |

### Principais achados

| Indicador | Resultado | Interpretacao |
|---|---:|---|
| Pedidos entregues analisados | 96.470 | Base usada para avaliar prazo de entrega. |
| Media geral vs prazo prometido | -11,18 dias | Na media, a entrega acontece antes do prazo. |
| Pedidos atrasados | 7.826 | Existe um grupo relevante de clientes impactados. |
| Percentual de pedidos atrasados | 8,11% | O problema nao e maioria, mas tem volume suficiente para acao. |
| Nota media sem atraso | 4,29 | Entregas no prazo ou adiantadas preservam satisfacao. |
| Nota media com atraso | 2,57 | O atraso derruba a experiencia percebida. |
| Notas 1 ou 2 sem atraso | 9,23% | Baixo risco relativo de insatisfacao. |
| Notas 1 ou 2 com atraso | 54,03% | Mais da metade dos clientes atrasados avalia mal. |

### Graficos para PPT

O notebook `analise_base_Sql.ipynb` gera imagens na pasta `Storytelling_sql/png`, ja com titulo narrativo e indicacao do ato da historia:

| Arquivo | Uso no PPT |
|---|---|
| `01_ato1_preparacao_status_entrega.png` | Mostrar que a operacao entrega bem para a maioria. |
| `02_ato2_conflito_queda_nota.png` | Mostrar a queda da nota media quando ha atraso. |
| `03_ato2_conflito_notas_ruins.png` | Mostrar aumento de notas ruins em pedidos atrasados. |
| `04_ato3_solucao_faixas_atraso.png` | Defender acao antes que o atraso entre em zona critica. |
| `05_ato3_solucao_estados_prioritarios.png` | Sugerir foco inicial por regioes com maior percentual de atraso. |

### Convencao importante

Para evitar leitura invertida, a analise usa a seguinte regra:

```sql
julianday(order_delivered_customer_date) - julianday(order_estimated_delivery_date)
```

Nesse calculo:

| Resultado | Significado |
|---:|---|
| Maior que 0 | Pedido atrasado. |
| Igual a 0 | Pedido entregue na data prometida. |
| Menor que 0 | Pedido entregue antes do prazo. |

O arquivo `Storytelling_sql/dicionario_sql.md` documenta as principais funcoes SQL, regras de negocio, consultas e metricas validadas para essa narrativa.

## Pipeline analitico

O repositorio segue uma estrutura simples de pipeline:

1. **Extracao**: leitura dos arquivos CSV e conexao com bancos SQLite.
2. **Inspecao**: visualizacao inicial, tipos de dados, colunas e volume das tabelas.
3. **Tratamento**: selecao de colunas, remocao de duplicidades e reorganizacao de tabelas.
4. **Modelagem relacional**: uso de chaves para cruzar pedidos, itens, pagamentos, produtos e vendedores.
5. **Analise**: consultas e DataFrames para responder perguntas de negocio.
6. **Storytelling**: organizacao das descobertas em uma narrativa compreensivel para tomada de decisao.

## Tecnologias utilizadas

- Python
- Pandas
- SQLite
- SQL

## Como executar localmente

1. Clone o repositorio:

```bash
git clone <url-do-repositorio>
cd SQL
```

2. Crie e ative um ambiente virtual:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

3. Instale as dependencias principais:

```bash
pip install pandas matplotlib jupyter openpyxl
```

4. Abra o Jupyter Notebook:

```bash
jupyter notebook
```

5. Execute os notebooks na ordem recomendada:

```text
Sql_sintx/Base/1. sql.ipynb
Sql_sintx/Base/2. where.ipynb
Sql_sintx/Base/3. group by.ipynb
Sql_sintx/Base/4. having.ipynb
Sql_sintx/Base/5. case.ipynb
Sql_sintx/Base/6. subquery.ipynb
Sql_sintx/Base/7. in and like.ipynb
Sql_sintx/Base/8. join.ipynb
Sql_sintx/Base/9. join2.ipynb
Sql_sintx/Base/10. union e fulljoin.ipynb
Storytelling_sql/notebook/2. create_db.ipynb
Storytelling_sql/notebook/1. analise.ipynb
Storytelling_sql/notebook/analise_base_Sql.ipynb
```

Para gerar os graficos do storytelling, execute o notebook `Storytelling_sql/notebook/analise_base_Sql.ipynb` ate a secao `Graficos para o PPT`.

## Dados

Os dados do modulo `Sql_sintx` sao bases ficticias de estudo.

Os dados do modulo `Storytelling_sql` sao arquivos CSV da Olist, anonimizados e usados para fins educacionais e analiticos. Como boa pratica de versionamento, os arquivos originais ficam separados em `Storytelling_sql/bases`, enquanto bases derivadas e bancos SQLite ficam em `Storytelling_sql/data`.

## Principais aprendizados

- SQL e mais forte quando usado com entendimento do modelo de dados.
- Joins precisam ser validados antes da analise para evitar duplicidade ou perda de registros.
- Consultas agregadas devem separar bem filtros de linha (`WHERE`) e filtros de grupo (`HAVING`).
- Pandas complementa SQL ao facilitar exploracao, validacao e exibicao dos resultados.
- Storytelling em dados depende de sequencia: contexto, pergunta, evidencia e conclusao.
- Medias gerais podem esconder grupos pequenos, mas com alto impacto na experiencia do cliente.
- Para analise de datas, a convencao do sinal precisa ser explicita para evitar conclusoes invertidas.
