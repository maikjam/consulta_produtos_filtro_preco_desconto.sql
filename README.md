 Consulta SQL — Produtos com Alto Desconto e Preço Acima da Média 
 
-- Liste os produtos (ID, marca, modelo, preço e desconto médio) 
-- cujo desconto médio recebido seja maior que 5% e cujo preço esteja 
-- acima da média dos preços dos produtos da mesma marca.

select
	fun.id_produto,
	pro.marca,
	pro.modelo,
	pro.preco,
	fun.desconto,
	pro2.media_preco

from (
	select 
		product_id as id_produto, 
		round(avg(discount) * -100, 2) as desconto 
	from sales.funnel 
	group by product_id
	having avg(discount) * -100 > 5
	) as fun
	
inner join (
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
	) as pro 
		on fun.id_produto = pro.product_id
		
inner join (
	select
		brand, 
		round(avg(price), 2) as media_preco
	from sales.products 
	group by brand
	) as pro2 
		on pro.marca = pro2.brand
where pro.preco > pro2.media_preco



---

Autor

Consulta criada por **Maikon Lucas Soares Viana** como parte de estudos e projetos com foco em análise de dados SQL.


