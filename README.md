# Teste Técnico: Pessoa Engenheira de Dados Involves

## 1) Descreva com suas palavras os principais conceitos abaixo:

### a) O que é um Data Warehouse?

Um Data Warehouse é um repositório centraliado responsável pelo armazenamento de dados de diversas fontes, visando a permitir a organização e a consulta de grandes volumes de dados históricos confiáveis. Para tais fins, o Data Wharehouse é caracterizado por não permitir a alteração dos dados e e não trabalhar com operações transacionais.

### b) Quais características possuem as tabelas do tipo Fato e Dimensão?

As Tabelas Fato se caracterizam por conter registros referentes a ocorrências que envolvam valores, métricas e quantidades. Já as Tabelas Dimensão são constituídas de registros referentes a informações descritivas que permitem agregar contexto aos fatos. Essas tabelas são utilizadas em modelagem dimensional de bancos de dados de forma que as chaves extrangeiras das Tabelas Fato se relacionam com as chaves primárias das Tabelas dimensão.

### c) O que é ETL?

ETL é o nome dado ao processo utilizado para trabalhar com dados de diversas fontes e carregá-los em sistemas analíticos, como Datawarehouses. O nome ETL é uma sigla em inglês para as 3 etapas do processo: Extract (Extração), Transform (Transfomação) e Load (Carga). Nesse processo os dados são: 

1. extraídos de suas respectivas fontes;
2. transformados por meio de processos de limpeza, padronização, filtragem, aplicação de regras de negócio e validação;
3. carregados em seu destino definido.

### d) Quais são as principais atribuições de um Engenheiro de Dados?

As principais atribuições de um Engenheiro de Dados são:
- Construção e manutenção dos ambientes de dados;
- Desenvolvimento e manutenção de pipelines de dados responsáveis pela transferência e transformação dos dados;
- Assegurar a qualidade e a governança dos dados disponibilizados na camada analítica.

### e) O que é Trade Marketing?

É uma área de aplicação do Marketing voltada para a relação das indústrias com os seus canais de distribuição (atacadistas e varejistas). O enfoque dessa área consiste em atuar em parceria com os pontos de venda para maximizar os volumes de vendas e garantir a exposição desejada dos produtos. Orientando-se por dados, o trading marketing pode analisar o desempenho obtido e obter insights para a aplicação de novas estratégias.

## 2) Crie uma query, considerando o SGBD MySQL, para exibir todos os dados de uma tabela de Pontos de Venda (tabela origem PONTO_VENDA_UNIDADE) e restringir apenas os pontos de venda que possuem sell in maior que 20.000 (campo SELLIN) e ainda ordená-los por nome do ponto de venda (campo NOME_PDV).

```SQL
SELECT * FROM PONTO_VENDA_UNIDADE -- Seleciona todas as colunas
WHERE SELLIN > 20000 -- Filtra pelos valores de SELLIN 
ORDER BY NOME_PDV -- Ordena por NOME_PDV
```

## 3) Considerando a tabela de origem da questão anterior, crie uma query que some o valor de sell in de acordo com cada ponto de venda e agrupe os resultados por mês (campo MES) e ano (campo ANO). Ordene os registros por um período cronológico de forma crescente e por nome do ponto de venda.

```SQL
SELECT ANO, MES, NOME_PDV, SUM(SELLIN) FROM PONTO_VENDA_UNIDADE -- Seleciona ANO, MES, NOME_PDV e a agregação em sOma de SELLIN
GROUP BY ANO, MES, NOME_PDV -- Agrupa por ANO, MES e NOME_PDV
ORDER BY ANO ASC, MES ASC, NOME_PDV ASC -- Orderna por ANO, MES e NOME_PDV
```

## 4) Considerando a tabela de origem da questão 2 e uma segunda tabela VISITAS_PONTO_VENDA, crie uma query que calcule a quantidade de visitas do ponto de venda de nome INVOLVES, sabendo-se que a tabela de visitas possui um campo que identifica se o ponto de venda foi visitado ou não chamado FL_VISITADO (Se 1 = Ponto de venda visitado / Se 0 = Ponto de venda não visitado). O campo chave que liga as duas tabelas é ID_PDV (na tabela PONTO_VENDA_UNIDADE) e FK_PDV(na tabela VISITAS_PONTO_VENDA). A query deve mostrar apenas as informações de nome do ponto de venda e quantidade de visitas realizadas.

```SQL
SELECT pvu.NOME_PDV, SUM(vpv.FL_VISITADO) FROM PONTO_VENDA_UNIDADE pvu -- Seleciona o NOME_PDV e a agragação da soma de PONTO_VENDA_UNIDADE
INNER JOIN VISITAS_PONTO_VENDA vpv -- Inner join entre as tabelas
ON pvu.ID_PFV = vpv.FK_PDV -- Relaciona as chaves ID_PFV E FK_PDV
WHERE pvu.NOME_PDV = 'INVOLVES' -- Filtra dados para manter quando o NOME_PDV for INVOLVES
GROUP BY pvu.NOME_PDV -- Agrupa por NOME_PDV
```
