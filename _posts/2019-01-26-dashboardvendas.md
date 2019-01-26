---
layout: post
title: Dashboard Vendas Automotivas
subtitle: Resultados anuais de vendas
tags: [power bi, BI]
comments: false
---

No capítulo 2 do curso [Microsoft Power BI para Data Science](https://www.datascienceacademy.com.br/pages/curso-microsoft-power-bi-para-data-science) é apresentado um estudo de caso bastante simples:

* Você é um analista de dados de uma revendedora de automóveis de luxo, com sede em São Paulo;

* A empresa atua nas regiões do sudeste e também no Paraná e na Bahia;

* Houve uma mudança de CEO recentemente e seu gerente requisitou que você apresentasse os resultados de 2012 a 2015 à nova liderança;

* Os dados foram extraídos do sistema de CRM da empresa em uma planilha de excel.

A estrutura dos dados recebidos foi da seguinte forma (primeiras 5 linhas):


![tabela](/img/tabelaoriginal.png)
      
      
Os dados originais apresentam 458 linhas.   Trata-se de uma base de dados bastante compacta, mas ,mesmo se houvessem milhões de linhas, nada diferiria em nossa análise.

Importanto os dados no Power BI, o primeiro passso realizado foi a criação de uma nova tabela vaiza, com o propósito de criar uma tabela de calendário, que será nossa primeira tabela de consulta. 

A forma que acho mais prática de criar a tabela é simplesmente copiando o código DAX abaixo (retirei deste [site](https://kohera.be/blog/power-bi/how-to-create-a-date-table-in-power-bi-in-2-simple-steps/)):


~~~~
Date =
ADDCOLUMNS (
CALENDAR (DATE(2000;1;1); DATE(2025;12;31));
"DateAsInteger"; FORMAT ( [Date]; "YYYYMMDD" );
"Year"; YEAR ( [Date] );
"Monthnumber"; FORMAT ( [Date]; "MM" );
"YearMonthnumber"; FORMAT ( [Date]; "YYYY/MM" );
"YearMonthShort"; FORMAT ( [Date]; "YYYY/mmm" );
"MonthNameShort"; FORMAT ( [Date]; "mmm" );
"MonthNameLong"; FORMAT ( [Date]; "mmmm" );
"DayOfWeekNumber"; WEEKDAY ( [Date] );
"DayOfWeek"; FORMAT ( [Date]; "dddd" );
"DayOfWeekShort"; FORMAT ( [Date]; "ddd" );
"Quarter"; "Q" & FORMAT ( [Date]; "Q" );
"YearQuarter"; FORMAT ( [Date]; "YYYY" ) & "/Q" & FORMAT ( [Date]; "Q" )
)
~~~~


Depois da tabela calendário, criei a partir da tabela original as seguintes tabelas de consulta:

* Cliente;

* Estados;

* Fabricantes;

* Modelos (dos veículos);

* Cor.

Ao final, permaneceram 7 tabelas, sendo 6 delas de consulta e 1 tabela de fatos, que é a tabela original.

Nos relacionamentos do Power BI, realizei um _star schema_ conforme abaixo:


![star](/img/starschema.png)

A partir da tabela original (VendaCarros), criei as principais medidas que seriam expostas no _dashboard_:

* **Vendas:** soma da dimensão ValorVenda;

* **Custo total:** soma das dimensões CustoEntrega, CustoMaoDeObra e ValorCusto;

* **Lucro:** diferença entre as medidas Vendas e Custo total;

* **Volume de vendas:** contagem de linhas da tabela VendaCarros;

* **Margem Líquida:** razão entre as medidas Lucro e Vendas;

* **Variação Mês Anterior Vendas:** diferença mensal (em %) no valor de vendas;

* **Variação Mês Anterior Volume:** diferença mensal (em %) no volume de vendas.

Determinadas as medidas, ocultei todas as chaves externas do relatório e campos não utilizados, para garantir uma melhor usabilidade.

No final, o _dashboard_ desenvolvido foi o seguinte:


![Dashboard](/img/Dashboard.png)

Na região superior, encontra-se os principais filtros do relatório, que são os filtros de data e cliente. A partir disso, todos as outras informações são geradas dinamicamente.

À esquerda, constam todos os principais indicadores de resultado. Ao meio, é possível ver a evolução (ou involução) das vendas e custos no período selecionado.

Estão disponíveis, também, informações quanto às vendas regionais e vendas mensais.

Por fim, a matriz de produtos detalha os resultados quanto aos produtos vendidos, classificados por fabricante e modelo de veículo.

Link para download do _dataset_ original e do arquivo Power BI:

[Dashboard](https://github.com/cazulli/Projetos-Power-BI/tree/master/Dashboard%20Vendas%20Automotivas)

### Esta é minha primeira postagem e meu primeiro projeto, então espero grandes evoluções no futuro!



 
