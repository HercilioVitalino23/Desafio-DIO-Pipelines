# README

## Explorando IA Generativa em um Pipeline de ETL com Python

Para o desafio da DIO - Explorando IA Generativa em um Pipeline de ETL com Python, desenvolvi esse Pipeline que basicamente extrai dados de uma planilha com ações (do setror de Petróleo e Gás) e suas respectivas informações (obtidas no site [Fundamentus](https://www.fundamentus.com.br)), e filtrar essas informações. No caso, optei por filtar ações que tinham um ROE, ROIC e Crescimento de receita em 5 anos, acima de valores pré-determinados. E salvei essas informações em um arquivo .csv e imagens .png de gráficos comparativos entre os ativos filtrados.

### Extract

Extrai as informações das colunas Papel, ROE, ROIC e Cresc. Rec.5a. Utilzando a biblioteca *Pandas*.

### Transform

Filtrei os dados utilizando parêmetros pré-determinados por mim (ROIC e ROE acima de 3,24% e Cresc. Rec.5a acima de 49%).

### Load
Salvei o resultado em *.csv*, e e usei a biblioteca matplotlib, para gerar gráficos em barras, comparando os dados obtidos de cada ativo e salvando em *.png*.

Por fim subi os arquivos no drive.

Podemos ainda subistituir por outras planilhas (maiores inclusive) e utilizar outros parâmetros. Mas utilizei esta como modelo para exemplificar.
