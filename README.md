# Teste Técnico: Pessoa Engenheira de Dados Involves

## 1) Descreva com suas palavras os principais conceitos abaixo:

### a) O que é um Data Warehouse?

**Resposta**:

Um Data Warehouse é um repositório centraliado responsável pelo armazenamento de dados de diversas fontes, visando a permitir a organização e a consulta de grandes volumes de dados históricos confiáveis. Para tais fins, o Data Wharehouse é caracterizado por não permitir a alteração dos dados e e não trabalhar com operações transacionais.

### b) Quais características possuem as tabelas do tipo Fato e Dimensão?

**Resposta**:

As Tabelas Fato se caracterizam por conter registros referentes a ocorrências que envolvam valores, métricas e quantidades. Já as Tabelas Dimensão são constituídas de registros referentes a informações descritivas que permitem agregar contexto aos fatos. Essas tabelas são utilizadas em modelagem dimensional de bancos de dados de forma que as chaves extrangeiras das Tabelas Fato se relacionam com as chaves primárias das Tabelas dimensão.

### c) O que é ETL?

**Resposta**:

ETL é o nome dado ao processo utilizado para trabalhar com dados de diversas fontes e carregá-los em sistemas analíticos, como Datawarehouses. O nome ETL é uma sigla em inglês para as 3 etapas do processo: Extract (Extração), Transform (Transfomação) e Load (Carga). Nesse processo os dados são: 

1. extraídos de suas respectivas fontes;
2. transformados por meio de processos de limpeza, padronização, filtragem, aplicação de regras de negócio e validação;
3. carregados em seu destino definido.

### d) Quais são as principais atribuições de um Engenheiro de Dados?

**Resposta**:

As principais atribuições de um Engenheiro de Dados são:
- Construção e manutenção dos ambientes de dados;
- Desenvolvimento e manutenção de pipelines de dados responsáveis pela transferência e transformação dos dados;
- Assegurar a qualidade e a governança dos dados disponibilizados na camada analítica.

### e) O que é Trade Marketing?

**Resposta**:

É uma área de aplicação do Marketing voltada para a relação das indústrias com os seus canais de distribuição (atacadistas e varejistas). O enfoque dessa área consiste em atuar em parceria com os pontos de venda para maximizar os volumes de vendas e garantir a exposição desejada dos produtos. Orientando-se por dados, o trading marketing pode analisar o desempenho obtido e obter insights para a aplicação de novas estratégias.

## 2) Crie uma query, considerando o SGBD MySQL, para exibir todos os dados de uma tabela de Pontos de Venda (tabela origem PONTO_VENDA_UNIDADE) e restringir apenas os pontos de venda que possuem sell in maior que 20.000 (campo SELLIN) e ainda ordená-los por nome do ponto de venda (campo NOME_PDV).

**Resposta**:

```SQL
SELECT * FROM PONTO_VENDA_UNIDADE -- Seleciona todas as colunas
WHERE SELLIN > 20000 -- Filtra pelos valores de SELLIN 
ORDER BY NOME_PDV -- Ordena por NOME_PDV
```

## 3) Considerando a tabela de origem da questão anterior, crie uma query que some o valor de sell in de acordo com cada ponto de venda e agrupe os resultados por mês (campo MES) e ano (campo ANO). Ordene os registros por um período cronológico de forma crescente e por nome do ponto de venda.

**Resposta**:

```SQL
SELECT ANO, MES, NOME_PDV, SUM(SELLIN) FROM PONTO_VENDA_UNIDADE -- Seleciona ANO, MES, NOME_PDV e a agregação em sOma de SELLIN
GROUP BY ANO, MES, NOME_PDV -- Agrupa por ANO, MES e NOME_PDV
ORDER BY ANO ASC, MES ASC, NOME_PDV ASC -- Orderna por ANO, MES e NOME_PDV
```

## 4) Considerando a tabela de origem da questão 2 e uma segunda tabela VISITAS_PONTO_VENDA, crie uma query que calcule a quantidade de visitas do ponto de venda de nome INVOLVES, sabendo-se que a tabela de visitas possui um campo que identifica se o ponto de venda foi visitado ou não chamado FL_VISITADO (Se 1 = Ponto de venda visitado / Se 0 = Ponto de venda não visitado). O campo chave que liga as duas tabelas é ID_PDV (na tabela PONTO_VENDA_UNIDADE) e FK_PDV(na tabela VISITAS_PONTO_VENDA). A query deve mostrar apenas as informações de nome do ponto de venda e quantidade de visitas realizadas.

**Resposta**:

```SQL
SELECT pvu.NOME_PDV, SUM(vpv.FL_VISITADO) FROM PONTO_VENDA_UNIDADE pvu -- Seleciona o NOME_PDV e a agragação da soma de PONTO_VENDA_UNIDADE
INNER JOIN VISITAS_PONTO_VENDA vpv -- Inner join entre as tabelas
ON pvu.ID_PFV = vpv.FK_PDV -- Relaciona as chaves ID_PFV E FK_PDV
WHERE pvu.NOME_PDV = 'INVOLVES' -- Filtra dados para manter quando o NOME_PDV for INVOLVES
GROUP BY pvu.NOME_PDV -- Agrupa por NOME_PDV
```
## 5) Considerando a query abaixo, a pessoa engenheira de dados identificou que a performance da query está muito abaixo do esperado. Imaginando que um dos problemas possa estar relacionado aos índices das tabelas do banco de dados, a pessoa resolveu criar os índices nas tabelas. Liste quais possíveis campos devem ser indexados nas tabelas do banco de dados para que a query criada possa performar melhor. Leve em consideração que nenhum campo no banco de dados está indexado.

```SQL
select
 FT.CICLO,
 FT.ID_DIM_PDV,
 FT.ID_BLOCO_ITEM,
 SUM(FT.QTD_PONTO_EXTRA),
 SUM(FTPI.TOTAL_NOTA_ITEM)
from FT_DOMINANCIA_PONTO_EXTRA_COMPLIANCE FT
inner join TABREF_PAINEL_LOJAS_LP TPLL
 on FT.ID_DIM_PDV = TPLL.ID_DIM_PDV
 and FT.CICLO = TPLL.CICLO
inner join FT_PERFECTSTORE_ITEM FTPI
 on FT.CICLO = FTPI.CICLO
 and FT.ID_DIM_PDV = FTPI.ID_DIM_PDV
 and FT.ID_BLOCO_ITEM = FTPI.ID_BLOCO_ITEM
 and FT.SEMANA_LP = FTPI.SEMANA_LP
where
 FT.CICLO = 202009
 and FT.ID_DIM_PDV = 223459792
group by FT.CICLO,
 FT.ID_DIM_PDV;
```

**Resposta**:

A prioridade maior para a indexação das tabelas deve ser dada às colunas utilizadas como chaves primárias/extrangeiras, bem como às colunas utilizadas em claulas de WHERE e GROUP BY. Assim, os campos que devem ser indexados em cada tabela são os seguintes:

- Tabela `FT_DOMINANCIA_PONTO_EXTRA_COMPLIANCE`:
  - ID_DIM_PDV;
  - CICLO;
  - ID_BLOCO_ITEM
  - SEMANA_LP;
- Tabela `TABREF_PAINEL_LOJAS_LP`:
  - ID_DIM_PDV;
  - CICLO;
- Tabela `FT_PERFECTSTORE_ITEM`:
  - ID_DIM_PDV;
  - CICLO;
  - ID_BLOCO_ITEM
  - SEMANA_LP.

## 6) Considere a instrução Python a seguir: `x = [ print(i) for i in range(10) if i % 2 == 0 ]` Após a execução dessa instrução no Python , a variável “x” conterá qual valor.

**Resposta**:

A variável `x` conterá uma lista de tamanho 5, contendo `None` em todos os seus campos. Isso acontece porque a função print exibe o valor mas retorna None e porque o list comprehension utilizado só realiza o print quando `i%2==0`, i.e., quando `i` é um número par.

## 7) Faça um script em Python que peça dois números e imprima a soma.

**Resposta**:

```python

x = int(input("Digite o primeiro número: "))
y = int(input("Digite o segundo número: "))

print(f"A soma dos números é: {x+y}")
```

---

## Para responder às questões 8, 9 e 10 utilize a ferramenta Pentaho Data Integration (PDI) na versão de sua preferência. A ETL final deve conter um job principal que, por sua vez, deve conter as transformações criadas nas questões 8, 9, 10. Além disso, que tal ganhar um ponto a mais nessas questões ? Para isso, inclua o projeto criado em um repositório do Github (é importante que seja público para termos visibilidade, ok ?). Compartilhe por aqui o link para o repositório.

### Observação importante: Como mencionado na primeira conversa, não twenho muita prática com o Pentaho. Fiz o processo com base no meu entendimento de pipelines ETL, porém com uma visão mais limitada e intuitiva da plataforma. Além disso, como uso Linux em minha máquina pessoal, houveram alguns erros na interface do Pentaho para alguns blocos específicos (ex.: group by) que me fizeram contornar com outra alternativa. Também precisei salvar os dados em um .csv, ao invés de gravar em um banco.

### As soluções se encontram disponíveis em: [https://github.com/jotace8921/teste-involves](https://github.com/jotace8921/teste-involves)

#### O job completo está [AQUI](https://github.com/jotace8921/teste-involves/blob/main/JOB/Involves_Teste_Job.kjb).

--- 

## 8) Construa uma transformação que deve usar como datasource o dataset (DATASET_TESTE_DE.csv) que contém informações de coletas de dados nos ponto de vendas. A ETL deve consultar o dataset e inserir, em uma base de dados (modelo dimensional), as informações coletadas, conforme as tabelas abaixo:

### a) Dimensão Calendário (DIM_CALENDARIO): Deve conter data, mês e ano da coleta;

**Resposta**: Código [AQUI](https://github.com/jotace8921/teste-involves/blob/main/JOB/TRANSFORMATIONS/8a_dim_calendario.ktr).

### b) Dimensão Ponto de Venda (DIM_PDV): Deve conter o id, nome e perfil do ponto de venda;

**Resposta**: Código [AQUI](https://github.com/jotace8921/teste-involves/blob/main/JOB/TRANSFORMATIONS/8b_dim_pdv.ktr).

### c) Dimensão Linha de Produto (DIM_LINHA_PRODUTO): Deve conter o id, nome e perfil da linha de produto.

**Resposta**: Código [AQUI](https://github.com/jotace8921/teste-involves/blob/main/JOB/TRANSFORMATIONS/8c_dim_linha_produto.ktr).

## 9) Construa uma transformação que deve usar como datasource o dataset (DATASET_TESTE_DE.csv) que contém informações de coletas de dados nos ponto de vendas. A transformação deve consultar o dataset e inserir, em uma base de dados (modelo dimensional), as informações coletadas, conforme as tabelas abaixo:

### a) Fato Disponibilidade (FT_DISPONIBILIDADE): Deve conter os ids de ligação das tabelas de dimensões criadas na questão anterior e a quantidade de presenças de cada linha de produto no mês de Setembro/20.

**Resposta**: Código [AQUI](https://github.com/jotace8921/teste-involves/blob/main/JOB/TRANSFORMATIONS/9a_ft_disponibilidade.ktr).

### b) Fato Disponibilidade Agregada (FT_DISPONIBILIDADE_AGREGADA): Deve conter os ids de ligação das tabelas de dimensões (Dimensão Calendário e Ponto de Venda) e a quantidade de presença de linhas de produto agrupadas por ponto de venda no mês de Setembro/20.

**Resposta**: Código [AQUI](https://github.com/jotace8921/teste-involves/blob/main/JOB/TRANSFORMATIONS/9b_ft_disponibilidade_agregada.ktr).

---
### Obs: Os dados de “Disponibilidade” estão categorizados na coluna TIPO_COLETA com o valor “Disponibilidade”. A presença é contada sempre que no campo VALOR aparecer o valor “SIM”
---

## 10) Construa uma transformação que deve usar como datasource o dataset (DATASET_TESTE_DE.csv) que contém informações de coletas de dados nos ponto de vendas. A transformação deve consultar o dataset e inserir, em uma base de dados (modelo dimensional), as informações coletadas, conforme as tabelas abaixo:

### a) Fato Ponto Extra (FT_PONTO_EXTRA): Deve conter os ids de ligação das tabelas de dimensões criadas na questão anterior e a soma de ponto extras de cada linha de produto no mês de Setembro/20.

**Resposta**: Código [AQUI](https://github.com/jotace8921/teste-involves/blob/main/JOB/TRANSFORMATIONS/10A_ft_ponto_extra.ktr).

### b) Fato Ponto Extra Agregada (FT_PONTO_EXTRA_AGREGADA): Deve conter os ids de ligação das tabelas de dimensões (Dimensão Calendário e Ponto de Venda) e a soma de ponto extras de linhas de produto agrupadas por ponto de venda no mês de Setembro/20.

**Resposta**: Código [AQUI](https://github.com/jotace8921/teste-involves/blob/main/JOB/TRANSFORMATIONS/10b_ft_ponto_extra_agregada.ktr).

---
### Obs: Os dados de “Ponto Extra” estão categorizados na coluna TIPO_COLETA com o valor “Ponto Extra”
---
