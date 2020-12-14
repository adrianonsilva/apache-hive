# Hive

- [1. Descrição](#link1)
- [2. Instalação](#link2)
- [3. Executando comandos](#link3)
- [4. Links](#link4)

<a id="link1"></a>
## 1. Descrição

![Screenshot](/images/h00.jpg)

Apache Hive é um data warehouse que está em cima do Hadoop. Usando Hive podemos rodar queries para
análise de dados. Hive permite escrever queries sql em vez de de complexos jobs Map-Reduce. 
Hive converte queries SQL em jobs MapReduce e submete no cluster.

Hive foi desenvolvido pelo Facebook, depois virou um projeto da Apache Software Foundation.

Hive usa comandos Hive Query Language (HQL) que é similar ao SQL padrão.

Dessa forma não é necessário escrever jobs em Java.

<a id="link2"></a>
## 2. Instalação

A versão do Hive precisa estar alinhada com a Versão do Hadoop, para esse exemplo será usada a seguinte versão:

Hive 2.3.7 e Hadoop 2.6.4

![Screenshot](/images/h01.jpg)

Componentes:
- Raspberry P4
- Ubuntu 20.04
- Java

![Screenshot](/images/h02.jpg)

- Hadoop

![Screenshot](/images/h03.jpg)

- Hive

![Screenshot](/images/h04.jpg)

#download</br>
wget https://downloads.apache.org/hive/hive-2.3.7/apache-hive-2.3.7-bin.tar.gz

#extract</br>
tar xvzf apache-hive-2.3.7-bin.tar.gz

#mover para pasta destino</br>
sudo mv apache-hive-2.3.7-bin /usr/local/hive</br>
sudo chown -R hduser /usr/local/hive</br>

![Screenshot](/images/h06.jpg)

#editar bashrc</br>
#para que as variáveis de ambiente sejam carregadas</br>
sudo nano ~/.bashrc</br>

#hive</br>
export HIVE_HOME=/usr/local/hive</br>
export PATH=$PATH:$HIVE_HOME/bin</br>
export CLASSPATH=$CLASSPATH:/usr/local/hive/lib/*</br>
#mysql jdbc driver</br>

export CLASSPATH=$CLASSPATH:/usr/share/java/mysql-connector-java-8.0.22.jar</br>

source ~/.bashrc</br>

#criar diretórios no hdfs, para onde as tabelas do hive serão criadas</br>
hdfs dfs -mkdir -p /bigdata/tmp</br>

#save table and other miscellaneous data</br>
hdfs dfs -mkdir -p /bigdata/hive/warehouse</br>

hdfs dfs -chmod g+w /bigdata/tmp</br>
hdfs dfs -chmod g+w /bigdata/hive/warehouse</br>

![Screenshot](/images/h07.jpg)

#editar os arquivos de configuração do hive</br>

#arquivo hive-env.sh</br>
cd /usr/local/hive/conf</br>
cp hive-env.sh.template hive-env.sh</br>
nano hive-env.sh</br>

#HADOOP_HOME to point to a hadoop install directory</br>
export HADOOP_HOME=/usr/local/hadoop</br>
export HADOOP_HEAPSIZE=512</br>

#Hive Configuration Directory</br>
export HIVE_CONF_DIR=/usr/local/hive/conf</br>
export HIVE_AUX_JARS_PATH=/usr/local/hive/lib</br>

#Java Home</br>
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-arm64</br>

![Screenshot](/images/h08.jpg)

![Screenshot](/images/h09.jpg)

#arquivo hive-site.xml</br>
cd /usr/local/hive/conf</br>
nano hive-site.xml</br>

![Screenshot](/images/h10.jpg)

O arquivo hive-site.xml indica:</br>
- a pasta criada no hdfs</br>
- a conexão para o banco de metadados do hive, nesse exmplo aponta para o mysql</br>

#Metastore Configuration

O Hive armazena seu metadata (schema, paticionamentos, etc.) em um database externo.</br>
Junto com o Hive vem o Derby, mas o Derby não permite multiplas conexões.</br>
Por causa disso, para esse exemplo será usado o mysql para o metastore.

#instalando o mysql</br>
sudo apt-get update</br>
sudo apt-get install mysql-server</br>
sudo mysql_secure_installation utility</br>
sudo systemctl start mysql</br>

#database server launches after a system reboot.</br>
sudo systemctl enable mysql</br>

#copy connector do mysql para a pasta lib do Hive</br>
Faça o download do mysql-connector-java-8.0.22.jar e copie para $HIVE_HOME/lib/</br>

![Screenshot](/images/h11.jpg)

![Screenshot](/images/h12.jpg)

#criar usuário e tabelas para o Hive</br>
mysql -u root -p</br>
mysql -V</br>

CREATE DATABASE METASTORE;</br>
USE METASTORE;</br>
SOURCE /usr/local/hive/scripts/metastore/upgrade/mysql/hive-schema-2.3.0.mysql.sql;</br>

CREATE USER 'hiveuser'@'%' IDENTIFIED BY 'teste';</br>
GRANT all on *.* to 'hiveuser'@'%';</br>
FLUSH PRIVILEGES;</br>

![Screenshot](/images/h13.jpg)

![Screenshot](/images/h14.jpg)

- inicializando o metastore

hive --service metastore</br>

![Screenshot](/images/h14a.jpg)

<a id="link3"></a>
## 3. Executando comandos

Para executar o Hive, vamos executar o comando hive, que permitirá a execução de comandos via linha de comando.

- verificar a versão

![Screenshot](/images/h15.jpg)

- Hive cli

![Screenshot](/images/h16.jpg)

![Screenshot](/images/h16a.jpg)

- Criando uma tabela

CREATE TABLE test (id int, name string)</br>
COMMENT 'paises'</br>
ROW FORMAT  DELIMITED</br>
FIELDS TERMINATED BY '\t'</br>
LINES TERMINATED BY '\n'</br>
STORED AS TEXTFILE;</br>

![Screenshot](/images/h16b.jpg)

- inserindo dados de um arquivo local na tabela criada anteriormente

LOAD DATA LOCAL INPATH '/home/hduser/dataset/pais.txt' OVERWRITE INTO TABLE test;</br>

![Screenshot](/images/h17.jpg)

![Screenshot](/images/h18.jpg)

![Screenshot](/images/h19.jpg)

![Screenshot](/images/h20.jpg)

- verificando o hdfs, usando a interface web do hadoop

![Screenshot](/images/h21.jpg)

Vale destacar, 

/bigdata/hive/warehouse/ (pasta criado no hdfs)</br>
test (tabela criada no hive)</br>
pais.txt (arqiovo carregado)</br>

<a id="link4"></a>
## 4. Links

Tutorials Point</br>
https://www.tutorialspoint.com/hive/index.htm

Comandos HQL</br>
http://myitlearnings.com/hive-queries-inserting-data-into-hive-table/

Introduction to hive</br>
https://www.edupristine.com/blog/introduction-to-hive</br>
https://www.guru99.com/introduction-hive.html</br>


