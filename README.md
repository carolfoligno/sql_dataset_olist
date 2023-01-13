
<img src="https://user-images.githubusercontent.com/80589529/212401902-88149d1c-fb7d-487b-9471-79d321a9076d.png" width="500" height="250" />

> Um banco de dados relacional é um banco de dados que modela os dados de uma forma que eles sejam percebidos pelo usuário como tabelas ou mais formalmente relações.
>

Armazenando os dados em tabelas, organizadas em colunas, onde cada coluna armazena um tipo de dados (inteiro, números reais, strings de caracteres, data, entre outros). MySQL, Oracle, PostgreSQL, Maria DB, SQLite são uns exemplos de Banco de Dados Relacionais.

SQLite é uma biblioteca em linguagem C que implementa um banco de dados SQL embutido. Programas que usam a biblioteca SQLite podem ter acesso a banco de dados SQL sem executar um processo SGBD separado.

> SQLite não é uma biblioteca cliente usada para conectar com um grande servidor de banco de dados, mas sim o próprio servidor.
>

Basicamente, é um banco de dados de arquivo, onde se utiliza o SQL para consultar os dados. 

A linguagem de programação padrão para os bancos de dados relacionais é a Structured Query Language (SQL).

## PROBLEMA DE NEGÓCIO

O cadastro das as informações sobre o produto, o cliente, o comerciante, o meio de pagamento, as avaliações e o pedido realizado de dezenas de e-commerce que realiza vendas onlines são armazenados em um banco de dados.

Os gerentes da empresa acreditam que há muitas informações valiosas armazenas nos dados, mas não tem habilidades para explorar e encontrar as respostas para validar ou refutar suas hipóteses de negócio.

## OS DADOS

* Os dados desse projeto estão disponível no [Kaggle](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce).

O MER (Modelo Entidade Relacionamento) das tabelas é um modelo dos dados que pode descreve a relação entre as tabelas num banco de dados relacional. 

<img src="https://user-images.githubusercontent.com/80589529/212404317-cfddfc77-ae15-4990-bf31-533ee2bacdc3.png" width="600" height="400" />

## PERGUNTAS DO CEO

O CEO fez 22 perguntas sobre os dados, que através da Linguagem SQL foi possível responde-las. Alguns exemplos das perguntas:

* Quantos clientes únicos tiveram seu pedidos com status de “processing”, “shipped” e “delivered”, feitos entre os dias 01 e 31 de Outubro de 2016. Mostrar o resultado somente se o número total de clientes for acima de 5.

```
query = """
SELECT 
    o.order_status,
    COUNT(DISTINCT o.order_id) 
FROM orders o 
WHERE o.order_status IN ('processing', 'shipped', 'delivered') AND 
    o.order_purchase_timestamp BETWEEN '2016-10-01' AND '2016-10-31'
GROUP BY o.order_status 
HAVING COUNT(DISTINCT o.order_id) > 5
"""
```
* Quantos produtos estão cadastrados nas categorias: perfumaria, brinquedos, esporte lazer e cama mesa, que possuem entre 5 e 10 fotos, um peso que não está entre 1 e 5 g, um altura maior que 10 cm, uma largura maior que 20 cm. Mostra somente as linhas com mais de 10 produtos únicos.

```
query = """
SELECT
    p.product_category_name,
    COUNT(DISTINCT p.product_id) as produtos
FROM products p
WHERE p.product_category_name IN ('perfumaria', 'brinquedos', 'esporte_lazer', 'cama_mesa_banho') 
    AND p.product_photos_qty BETWEEN 5 AND 10 
    AND p.product_weight_g NOT BETWEEN 1 AND 5
    AND p.product_height_cm > 10
    AND p.product_width_cm > 20
GROUP BY p.product_category_name
"""
```
* Quantos produtos estão cadastrados em qualquer categorias que comece com a letra “a” e termine com a letra “o” e que possuem mais de 5 fotos? Mostrar as linhas com mais de 10 produtos.
```
query = """
SELECT 
    p.product_category_name,
    COUNT(DISTINCT p.product_id) as produtos
FROM products p 
WHERE p.product_category_name LIKE 'a%o'
    AND p.product_photos_qty > 5
GROUP BY p.product_category_name 
HAVING COUNT(DISTINCT p.product_id) > 10
"""
```

> As outras perguntas e as respostas estão disponíveis no arquivo notebook .ipynb .
>
> Esse pequeno projeto foi realizado com o objetivo de desenvolver o conhecimento em SQL com a orientação do professor Meigarom da [Comunidade DS](https://membro.comunidadedatascience.com/).
