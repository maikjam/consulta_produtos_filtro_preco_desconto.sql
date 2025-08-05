# Consulta SQL — Produtos  Desconto e Preço 

Consulta (ID, marca, modelo, preço e desconto médio),
cujo desconto médio recebido seja maior que 5% e cujo preço esteja 
acima da média dos preços dos produtos da mesma marca.

Objetivo

Esta consulta tem como finalidade **listar produtos** que atendem a dois critérios simultâneos:

1. Possuem **desconto médio superior a 5%**.
2. Têm **preço acima da média** dos preços praticados para produtos da **mesma marca**.

---

Lógica da Consulta

A consulta é composta por três blocos principais (subconsultas) e suas respectivas junções:

### 1. Cálculo do Desconto Médio por Produto

sql
select 
	product_id as id_produto, 
	round(avg(discount) * -100, 2) as desconto 
from sales.funnel 
group by product_id
having avg(discount) * -100 > 5


- Agrupa os produtos por product_id.
- Calcula o desconto médio (em percentual) invertendo o sinal, pois os descontos são negativos na base.
- Filtra para manter apenas produtos com **desconto médio superior a 5%**.

---

2. Informações do Produto

sql
select 
	product_id,
	brand as marca,
	model as modelo,
	price as preco
from sales.products 
group by 
	product_id,
	brand,
	model,
	price


- Extrai os principais atributos do produto: marca, modelo e preço.
- A junção com a subconsulta anterior permite relacionar os descontos com os produtos.

---

3. Preço Médio por Marca

sql
select
	brand, 
	round(avg(price), 2) as media_preco
from sales.products 
group by brand


- Agrupa os produtos por brand (marca).
- Calcula a **média de preço** para cada marca.

---

4. Filtro Final

sql
where pro.preco > pro2.media_preco


- Após as junções, o filtro final garante que o produto só será exibido se o seu preço estiver **acima da média de sua marca**.

---

Resultado Esperado

A consulta retorna as seguintes colunas:

- id_produto: ID único do produto.
- marca: Marca do produto.
- modelo: Modelo do produto.
- preco: Preço atual do produto.
- desconto: Desconto médio aplicado (%).
- media_preco: Média de preços da marca.

---

Observações

- O uso do ROUND em avg(discount) e avg(price) é para melhor visualização dos dados.
- Descontos negativos na tabela sales.funnel são tratados ao multiplicar por -100.

---

Requisitos de Execução

- Banco de dados com os esquemas sales.funnel e sales.products já populados.
- Permissões para realizar joins e agregações.

---

Autor

Consulta criada por **Maikon Lucas Soares Viana** como parte de estudos e projetos com foco em análise de dados SQL.


