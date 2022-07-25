# Apache_Spark

## INSTALLATION

```
sudo apt update
```

```
sudo apt install apt-transport-https ca-certificates gnupg2 curl software-properties-common
```

```
sudo curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
```

```
sudo apt-key fingerprint 0EBFCD88
```

```
printf 'deb [arch=amd64] https://download.docker.com/linux/debian buster stable\n'| sudo tee /etc/apt/sources.list.d/docker-ce.list
```

```
sudo apt-get update
```

```
sudo apt-get install docker-ce
```

```
docker --version
```

## LANCEMENT

```
service docker start
```

```
service docker status
```

```
sudo docker-compose up
```

```
sudo docker exec -it docker-spark_master_1 /bin/bash
```

### Utiliser scala

```
./spark-shell
```

<p>En scala on peut utiliser : </p>
<ul>
    <li>def : pour definir une methode</li>
    <li>var : pour definir une variable</li>
    <li>val : pour definir une constance</li>
</ul>

```
val tr = sc.textFile("/tmp/data/table_ronde/*.txt")
```

```
tr.map( l => l.length  )
```

```
ln.reduce( (a, b) => a + b  )
```

```
tr.flatMap( _.split(" ") ).take(10)
```

```
tr.flatMap( _.split(" ") ).filter(mot => mot.contains("table" )).take(10)
```

```
tr.flatMap( _.split(" ") ).map(_.replaceAll("[,.]", "")).filter(mot => mot.contains("table" )).take(10)
```

```
tr.flatMap( _.split(" ") ).map(_.replaceAll("[,.]", "")).filter(mot => mot.contains("table" )).map(mot => (mot, 1)).reduceByKey(_ + _).take(10)
```

### Utiliser Python

```
./pyspark
```

```
val tr = sc.textFile("/tmp/data/table_ronde/*.txt"
```

```
tr.flatMap( lambda l : l.split(" ") ).filter(lambda mot : "table" in mot).take(10)
```

```
tr.flatMap( lambda l : l.split(" ") ).filter(lambda mot : "table" in mot).saveAsTextFile("/tmp/data/tables")
```

### Creer un Dataframe

```
cepages = [
  ("Aglianico", "R", "IT"),
  ("Aligoté", "B", "FR"),
  ("Amigne", "B", "CH"),
  ("Arbois", "B", "FR"),
  ("Arinarnoa", "R", "FR"),
  ("Arinto", "B", "PT"),
  ("Arriloba", "B", "FR"),
  ("Gewurztraminer", "B", "CH"),
  ("Grechetto", "B", "IT"),
  ("Grignolino", "R", "IT"),
  ("Grüner Veltiner", "B", "AT")
]
```

```
df = spark.createDataFrame(cepages, ["Nom", "Couleur", "Pays"])
```

```
df.Nom
```

```
df.columns
```

```
df.take(5)
```

```
df.filter(df.Couleur == 'R').show()
```

### Ajouter le package spark-avro

```
./bin/spark-shell --packages com.databricks:spark-avro_2.11:4.0.0
```

```
./bin/spark-shell --master yarn --conf spark.ui.port=0 --packages org.apache.spark:spark-avro_2.11:2.4.0
```

```
import com.databricks.spark.avro._
```

**Lecture du fichier avro**

```
val co = spark.read.avro("/tmp/data/communes.avro")
```

### Lecture du fichier csv
```
val df = spark.read.option("header",true).csv("/tmp/data/infos.csv")
```

```
co.show()
```

### Jointure des 2 Dataframes
```
co.join(co2, co("ID") === co2("ID")).show()
```

```
co.filter("Nom == GAYE").join(co2, co("ID") === co2("ID")).show()
```

### Ecriture et lectures de fichier parquet
```c
o.write.parquet("/tmp/data/infos.parquet")
```

```
val df = spark.read.parquet("/tmp/data/infos.parquet")
```
