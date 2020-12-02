# Hive

Uma vez que o dataset (dados) estão no hadoop e seus gigabytes espalhados em vários nodes temos que 
usá-los.

Seja para consultas, análises, contagens e etc

Para issos seria necessário construir um programa mapredure (geralmente em java) para fazer isso.

Isso por si só é uma tarefa trabalhosa e exige do desenvolvedor um conhecimento tanto da arquitetura do 
hadoop quando da linguagem java.

O Hive tem início no Facebook, para conseguir suportar a quantidadade de dados gerada e o seu crescimento
o Facebook precisava usar outras formas de armazenamento.

Para fazer uso do Hadoop (armazenamento) e preservar o skill dos desenvolvedores que já usavam a linguagem SQL, foi desenvolvido o Hive, que abstrai as chamadas de baixo nível do java para os programas
map reduce.

Hoje o Hive faz parte da fundação apache.

ex:

Imagine um arquivo simples no hadoop que tenha o seguinte layout

codigo	nome	cidade	estado
1	Paulo	São Paulo	SP
2	Lucas	Santos	SP


Para usá-lo no Hive são feitas essas etapas:

1) criar o database
CREATE DATABASE|SCHEMA [IF NOT EXISTS] dbteste;

2) criar uma tabela (defição)
CREATE TABLE IF NOT EXISTS tbteste ( codigo int, nome String,
cidade String, estado String)
COMMENT ‘tabela exemplo’
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ‘\t’    ("\t" = tab,";" = ponto e vírgula, etc )
LINES TERMINATED BY ‘\n’
STORED AS TEXTFILE;


3) carregar os dados para a tabela
LOAD DATA LOCAL INPATH '/home/user/sample.txt' [OVERWRITE] INTO TABLE tbteste; (arquivo local na máquina)
LOAD DATA INPATH '/hadoop/folder/file.txt' INTO TABLE tbteste; (arquivo presente no hdfs)

4) executar consultas
use dbteste;
select * from tbteste;

5) tambêm é possível executar comandos insert

INSERT INTO TABLE tbteste
VALUES (1,'Paulo', 'São Paulo', 'SP')

ou várias linhas
INSERT INTO TABLE tbteste
VALUES (1,'Paulo', 'São Paulo', 'SP')
, (2,'Lucas', 'Santos', 'SP')

Links

http://myitlearnings.com/hive-queries-inserting-data-into-hive-table/

https://www.edupristine.com/blog/introduction-to-hive

https://www.guru99.com/introduction-hive.html


