# proyecto-integrador-modulo-4-Soy-Henry

En este repositorio muestro mi paso por el poryecto integrador del modulo 4.

## 1) HDFS
Copiar los archivos ubicados en la carpeta Datasets, dentro del contenedor "namenode"

  'sudo docker exec -it namenode bash'
  'cd home'
  'mkdir Datasets'
  'exit'
  'sudo docker cp <path><archivo> namenode:/home/Datasets/<archivo>'
  
![Captura de pantalla 2023-10-16 182032](https://github.com/JUrielCerecero/proyecto-integrador-modulo-4-Soy-Henry/assets/121000341/4769cae7-bd83-4bb8-9f75-5323038b08a9)
Ubicarse en el contenedor "namenode"

 ' sudo docker exec -it namenode bash'
Crear un directorio en HDFS llamado "/data".

  'hdfs dfs -mkdir -p /data'

![Captura de pantalla 2023-10-16 182147](https://github.com/JUrielCerecero/proyecto-integrador-modulo-4-Soy-Henry/assets/121000341/aaa9e6c4-b297-44b4-8f9b-9fcfcc7d32f7)

Copiar los archivos csv provistos a HDFS:

  'hdfs dfs -put /home/Datasets/* /data'
  
![Captura de pantalla 2023-10-17 110747](https://github.com/JUrielCerecero/proyecto-integrador-modulo-4-Soy-Henry/assets/121000341/566b7da8-d632-4776-9266-6848ab369f52)

## 2) Hive

Crear tablas en Hive, a partir de los csv ingestados en HDFS.

Para esto, se puede ubicar dentro del contenedor correspondiente al servidor de Hive, y ejecutar desdea allí los scripts necesarios

  'sudo docker exec -it hive-server bash'
  'hive'
Este proceso de creación las tablas debe poder ejecutarse desde un shell script.

Nota: Para ejecutar un script de Hive, requiere el comando:

  'hive -f <script.hql>'
![Captura de pantalla 2023-10-17 110907](https://github.com/JUrielCerecero/proyecto-integrador-modulo-4-Soy-Henry/assets/121000341/c34f2d8a-68f7-4b77-8dd6-ee36a358fe30)

## 3) Formatos de Almacenamiento

Las tablas creadas en el punto 2 a partir de archivos en formato csv, deben ser almacenadas en formato Parquet + Snappy. Tener en cuenta además de aplicar particiones para alguna de las tablas.

![Captura de pantalla 2023-10-17 111424](https://github.com/JUrielCerecero/proyecto-integrador-modulo-4-Soy-Henry/assets/121000341/4d2f8b48-9969-4d7c-b222-fcf2a71c5a7e)

## 4) SQL

Creacionde indices

![Captura de pantalla 2023-10-17 112602](https://github.com/JUrielCerecero/proyecto-integrador-modulo-4-Soy-Henry/assets/121000341/3574cf89-88c4-4a84-985b-93cdc72fa3d1)


## 5) No-SQL

### 1) HBase:
Instrucciones:

	'1- sudo docker exec -it hbase-master hbase shell'

		'create 'personal','personal_data''
  ![paso 5 hbase 1](https://github.com/JUrielCerecero/proyecto-integrador-modulo-4-Soy-Henry/assets/121000341/64c537b5-5fb4-4ad1-ac4d-6e230b2c92df)

    'list 'personal''
   	'put 'personal',1,'personal_data:name','Juan''
		'put 'personal',1,'personal_data:city','Córdoba''
		'put 'personal',1,'personal_data:age','25''
		'put 'personal',2,'personal_data:name','Franco''
		'put 'personal',2,'personal_data:city','Lima''
		'put 'personal',2,'personal_data:age','32''
		'put 'personal',3,'personal_data:name','Ivan''
		'put 'personal',3,'personal_data:age','34''
		'put 'personal',4,'personal_data:name','Eliecer''
		'put 'personal',4,'personal_data:city','Caracas''
		'get 'personal','4''
 ![paso 5 hbase 2](https://github.com/JUrielCerecero/proyecto-integrador-modulo-4-Soy-Henry/assets/121000341/47070bdc-0198-407f-8795-6390c8361a79)

	2-En el namenode del cluster:

		'hdfs dfs -put personal.csv /hbase/data/personal.csv'

	3-'sudo docker exec -it hbase-master bash'
		    'hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.separator=',' -Dimporttsv.columns=HBASE_ROW_KEY,personal_data:name,personal_data:city,personal_data:age personal            hdfs://namenode:9000/hbase/data/personal.csv'
  ![paso 5 hbase 3](https://github.com/JUrielCerecero/proyecto-integrador-modulo-4-Soy-Henry/assets/121000341/1f4ced60-f88b-450d-b5b7-e96944bb8390)
		'hbase shell'
		'scan 'personal''
		'create 'album','label','image''
		'put 'album','label1','label:size','10''
		'put 'album','label1','label:color','255:255:255''
		'put 'album','label1','label:text','Family album''
		'put 'album','label1','image:name','holiday''
		'put 'album','label1','image:source','/tmp/pic1.jpg''
  
  ![paso 5 hbase 4](https://github.com/JUrielCerecero/proyecto-integrador-modulo-4-Soy-Henry/assets/121000341/3a6049cd-9dd9-44c9-bad3-4617eed03a7c)

		'get 'album','label1''

  ![paso 5 hbase 6](https://github.com/JUrielCerecero/proyecto-integrador-modulo-4-Soy-Henry/assets/121000341/c8223e7d-9ca6-443b-a534-1ec7c455c9ab)

  # 2) MongoDB

  Instrucciones
  	1) 	sudo docker cp iris.csv mongodb:/data/iris.csv
		  sudo docker cp iris.json mongodb:/data/iris.json
  
  ![paso 5 mongodb 1](https://github.com/JUrielCerecero/proyecto-integrador-modulo-4-Soy-Henry/assets/121000341/22b6f013-17b9-4ff8-b3c7-1bdfad6c8cfa)

  	2)  sudo docker exec -it mongodb bash

![paso 5 mongo 2](https://github.com/JUrielCerecero/proyecto-integrador-modulo-4-Soy-Henry/assets/121000341/875d997b-f25f-4d6b-9bc6-916b73a6aa61)

	  3) 	mongoimport /data/iris.csv --type csv --headerline -d dataprueba -c iris_csv
		  mongoimport --db dataprueba --collection iris_json --file /data/iris.json --jsonArray

	  4) mongosh
    	use dataprueba
  
  ![paso 5 mongo 3](https://github.com/JUrielCerecero/proyecto-integrador-modulo-4-Soy-Henry/assets/121000341/7a4c2e37-84e5-4200-8d5e-dd59f09ff550)
  
		show collections
		
	![paso 5 mongo 4](https://github.com/JUrielCerecero/proyecto-integrador-modulo-4-Soy-Henry/assets/121000341/1658f36b-5dab-40d7-b704-676aa419257a)

    db.iris_csv.find()
		db.iris_json.find()
  
  ![paso 5 mongo 5](https://github.com/JUrielCerecero/proyecto-integrador-modulo-4-Soy-Henry/assets/121000341/bae39b92-ac8b-472e-becf-5d494f8b8ebf)

	  5) 	mongoexport --db dataprueba --collection iris_csv --fields sepal_length,sepal_width,petal_length,petal_width,species --type=csv --out /data/iris_export.csv
		mongoexport --db dataprueba --collection iris_json --fields sepal_length,sepal_width,petal_length,petal_width,species --type=json --out /data/iris_export.json
				
  	6) 	Descargar desde https://search.maven.org/search?q=g:org.mongodb.mongo-hadoop los jar:
  		https://search.maven.org/search?q=a:mongo-hadoop-hive
  		https://search.maven.org/search?q=a:mongo-hadoop-spark
  		
  		sudo docker cp mongo-hadoop-hive-2.0.2.jar hive-server:/opt/hive/lib/mongo-hadoop-hive-2.0.2.jar
  		sudo docker cp mongo-hadoop-core-2.0.2.jar hive-server:/opt/hive/lib/mongo-hadoop-core-2.0.2.jar
  		sudo docker cp mongo-hadoop-spark-2.0.2.jar hive-server:/opt/hive/lib/mongo-hadoop-spark-2.0.2.jar
  		sudo docker cp mongo-java-driver-3.12.11.jar hive-server:/opt/hive/lib/mongo-java-driver-3.12.11.jar
  ![paso 5 mongo 6](https://github.com/JUrielCerecero/proyecto-integrador-modulo-4-Soy-Henry/assets/121000341/ee913005-e81a-4d89-891a-b66088e08f1e)

  	7) 	sudo docker cp iris.hql hive-server:/opt/iris.hql
  		sudo docker exec -it hive-server bash

  	8) 	hiveserver2
  		chmod 777 iris.hql
  		hive -f iris.hql
![paso 5 mongo 7](https://github.com/JUrielCerecero/proyecto-integrador-modulo-4-Soy-Henry/assets/121000341/6a460399-6a71-48b5-bb73-dada82d6b383)


# 1) Spark y Scala:
Ubicarse en la línea de comandos del Spark master y comenzar PySpark.

'  docker exec -it spark-master bash'
  '/spark/bin/pyspark --master spark://spark-master:7077'
Cargar raw-flight-data.csv desde HDFS.

	'from pyspark.sql.types import *'

	'flightSchema = StructType([
	StructField("DayofMonth", IntegerType(), False),
	StructField("DayOfWeek", IntegerType(), False),
	StructField("Carrier", StringType(), False),
	StructField("OriginAirportID", IntegerType(), False),
	StructField("DestAirportID", IntegerType(), False),
	StructField("DepDelay", IntegerType(), False),
	StructField("ArrDelay", IntegerType(), False),
	]);'

	'flights = spark.read.csv('hdfs://namenode:9000/data/flights/raw-flight-data.csv', schema=flightSchema, header=True)'
  
  	'flights.show()'
	  +----------+---------+-------+---------------+-------------+--------+--------+
|DayofMonth|DayOfWeek|Carrier|OriginAirportID|DestAirportID|DepDelay|ArrDelay|
+----------+---------+-------+---------------+-------------+--------+--------+
|        19|        5|     DL|          11433|        13303|      -3|       1|
|        19|        5|     DL|          14869|        12478|       0|      -8|
|        19|        5|     DL|          14057|        14869|      -4|     -15|
|        19|        5|     DL|          15016|        11433|      28|      24|
|        19|        5|     DL|          11193|        12892|      -6|     -11|
|        19|        5|     DL|          10397|        15016|      -1|     -19|
|        19|        5|     DL|          15016|        10397|       0|      -1|
|        19|        5|     DL|          10397|        14869|      15|      24|
|        19|        5|     DL|          10397|        10423|      33|      34|
|        19|        5|     DL|          11278|        10397|     323|     322|
|        19|        5|     DL|          14107|        13487|      -7|     -13|
|        19|        5|     DL|          11433|        11298|      22|      41|
|        19|        5|     DL|          11298|        11433|      40|      20|
|        19|        5|     DL|          11433|        12892|      -2|      -7|
|        19|        5|     DL|          10397|        12451|      71|      75|
|        19|        5|     DL|          12451|        10397|      75|      57|
|        19|        5|     DL|          12953|        10397|      -1|      10|
|        19|        5|     DL|          11433|        12953|      -3|     -10|
|        19|        5|     DL|          10397|        14771|      31|      38|
|        19|        5|     DL|          13204|        10397|       8|      25|
+----------+---------+-------+---------------+-------------+--------+--------+
only showing top 20 rows
  	'flights.describe()'
![paso 6 spark 2](https://github.com/JUrielCerecero/proyecto-integrador-modulo-4-Soy-Henry/assets/121000341/d4029206-e8e2-4bf2-b0d0-f12d2dd0442e)

Ubicarse en la línea de comandos del Spark master y comenzar Scala.

  docker exec -it spark-master bash
  spark/bin/spark-shell --master spark://spark-master:7077
Cargar raw-flight-data.csv desde HDFS.

	case class flightSchema(DayofMonth:String, DayOfWeek:String, Carrier:String, OriginAirportID:String, DestAirportID:String, DepDelay:String, ArrDelay:String)
	val flights = spark.read.format("csv").option("sep", ",").option("header", "true").load("hdfs://namenode:9000/data/flights/raw-flight-data.csv").as[flightSchema]

  	flights.show()

+----------+---------+-------+---------------+-------------+--------+--------+
|DayofMonth|DayOfWeek|Carrier|OriginAirportID|DestAirportID|DepDelay|ArrDelay|
+----------+---------+-------+---------------+-------------+--------+--------+
|        19|        5|     DL|          11433|        13303|      -3|       1|
|        19|        5|     DL|          14869|        12478|       0|      -8|
|        19|        5|     DL|          14057|        14869|      -4|     -15|
|        19|        5|     DL|          15016|        11433|      28|      24|
|        19|        5|     DL|          11193|        12892|      -6|     -11|
|        19|        5|     DL|          10397|        15016|      -1|     -19|
|        19|        5|     DL|          15016|        10397|       0|      -1|
|        19|        5|     DL|          10397|        14869|      15|      24|
|        19|        5|     DL|          10397|        10423|      33|      34|
|        19|        5|     DL|          11278|        10397|     323|     322|
|        19|        5|     DL|          14107|        13487|      -7|     -13|
|        19|        5|     DL|          11433|        11298|      22|      41|
|        19|        5|     DL|          11298|        11433|      40|      20|
|        19|        5|     DL|          11433|        12892|      -2|      -7|
|        19|        5|     DL|          10397|        12451|      71|      75|
|        19|        5|     DL|          12451|        10397|      75|      57|
|        19|        5|     DL|          12953|        10397|      -1|      10|
|        19|        5|     DL|          11433|        12953|      -3|     -10|
|        19|        5|     DL|          10397|        14771|      31|      38|
|        19|        5|     DL|          13204|        10397|       8|      25|
+----------+---------+-------+---------------+-------------+--------+--------+
only showing top 20 rows

![paso 6 scala](https://github.com/JUrielCerecero/proyecto-integrador-modulo-4-Soy-Henry/assets/121000341/a9c50042-6c17-4dbd-a8fd-5368e7cfd146)

# 2) Kafka
			sudo docker-compose up -d
			sudo docker exec -it kafka_container bash
			cd /opt/kafka/bin
			sh kafka-topics.sh --create --bootstrap-server kafka:9092 --replication-factor 1 --partitions 100 --topic demo
			sh kafka-topics.sh --list --bootstrap-server kafka:9092
			sh kafka-topics.sh --describe --bootstrap-server kafka:9092 --topic demo 
   ![paso 6 kafka](https://github.com/JUrielCerecero/proyecto-integrador-modulo-4-Soy-Henry/assets/121000341/0c081511-a208-4b4f-bb2e-9f761087bad5)
			sh kafka-console-consumer.sh --bootstrap-server kafka:9092 --topic demo --from-beginning
			sh kafka-console-producer.sh --broker-list localhost:9092 --topic demo
	Acceder a <IP_Anfitrion>:9000	
	
			Desde Scala:
			val df = spark.readStream
					.format("kafka")
					.option("kafka.bootstrap.servers", "192.168.1.100:9092")
					.option("subscribe", "json_topic")
					.option("startingOffsets", "earliest") // From starting
					.load()

			df.printSchema()
			
			Más ejemplos:
				https://github.com/dbusteed/kafka-spark-streaming-example
						
			Otra forma de ejecutar:
			docker-compose exec kafka kafka-console-consumer.sh --bootstrap-server kafka:9092 --topic TenMinPsgCnts --from-beginning
![paaso6 kafka 2](https://github.com/JUrielCerecero/proyecto-integrador-modulo-4-Soy-Henry/assets/121000341/53086815-2673-4ae0-8d9c-db1a2c3f6f4b)

# 7) Carga incremental con Spark
Ahora resta evaluar qué sucede cuando en los sistemas fuente, se genere más dato, es decir, siguiendo los datos de esta práctica, qué pasa cuando se carguen más ventas. Se debería tomar las novedades e ingestar en el modelo existente cada día, de modo que la tabla venta, irá creciendo en cantidad de registro de manera diaria. Para este fin, se provee un script en spark que realiza la generación de nuevas ventas, de manera aleatoria, para poder crear una situación, donde se cuenta con novedades para la tabla de venta. El script "Paso06_GeneracionVentasNuevasPorDia.py" utiliza los datasets provistos en la carpeta "Datasets\data_nvo" para generar las novedades de forma automática. Revisar la variable "fecha_nvo" que contiene la fecha para la que se quiere generar información, como tenemos datos hasta el año 2020, la fecha de ejemplo tomada es '2021-01-01'. Es necesiario entonces generar, un script tal que tome las novedades en csv, y las cargue al modelo.

	sudo docker exec -it spark-master /spark/bin/spark-submit --master spark://spark-master:7077 /home/Paso06_GeneracionVentasNuevasPorDia.py

![paso6 generacion ventas incrementos](https://github.com/JUrielCerecero/proyecto-integrador-modulo-4-Soy-Henry/assets/121000341/b77e163b-d9b6-45ea-b743-3e804aa708a1)

Supongamos que tenemos nuestro script, y ahora se quiere programar su ejecución:

	/spark/bin/spark-submit --master spark://spark-master:7077 Paso06_IncrementalVentas.py
 
Con crontab, para que ejecute cada día a las 5 AM:

	$ crontab -e
	5	0	*	*	*	/home/CargaIncremental.sh

	$ crontab -l

![paso 7 crontab](https://github.com/JUrielCerecero/proyecto-integrador-modulo-4-Soy-Henry/assets/121000341/52b7a7d4-39fe-4c67-97c6-9c3f25ed2eb2)

