# Apache_Spark
*Auteur :* **Papa Ba GAYE**
## INSTALLATION

**Au moment de repository, on est actuellement à la version 3.3.0**

```
sudo apt update
```

```
wget https://dlcdn.apache.org/spark/spark-3.3.0/spark-3.3.0-bin-hadoop3.tgz
```

```
tar xzf spark-3.3.0-bin-hadoop3.tgz
```

```
sudo mv spark-3.3.0-bin-hadoop3 /opt/spark
```

## Utiliser scala
**Lancer pyspark**
```
spark-shell
```

<p>En scala on peut utiliser : </p>
<ul>
    <li>def : pour definir une methode</li>
    <li>var : pour definir une variable</li>
    <li>val : pour definir une constance</li>
</ul>

**Lecture des fichiers .txt**
```
val tr = sc.textFile("/tmp/data/table_ronde/*.txt")
```

**Map avec une fonction lambda**
```
tr.map( l => l.length  )
```

**Reduce fonction**
```
ln.reduce( (a, b) => a + b  )
```

**Split fonction**
```
tr.flatMap( _.split(" ") ).take(10)
```

```
tr.flatMap( _.split(" ") ).filter(mot => mot.contains("table" )).take(10)
```

**Split & Filter ... fonction**
```
tr.flatMap( _.split(" ") ).map(_.replaceAll("[,.]", "")).filter(mot => mot.contains("table" )).take(10)
```

```
tr.flatMap( _.split(" ") ).map(_.replaceAll("[,.]", "")).filter(mot => mot.contains("table" )).map(mot => (mot, 1)).reduceByKey(_ + _).take(10)
```

## Utiliser Python

**Lancer pyspark**
```
pyspark
```

**Lecture des fichiers .txt**
```
val tr = sc.textFile("/tmp/data/table_ronde/*.txt"
```

**FlatMap**
```
tr.flatMap( lambda l : l.split(" ") ).filter(lambda mot : "table" in mot).take(10)
```

```
tr.flatMap( lambda l : l.split(" ") ).filter(lambda mot : "table" in mot).saveAsTextFile("/tmp/data/tables")
```

## Creer un Dataframe

**Creation d'une liste de tuple**
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

**Creation d'un dataframe**
```
df = spark.createDataFrame(cepages, ["Nom", "Couleur", "Pays"])
```

**Afficher une colonne**
```
df.Nom
```

**Afficher les colonnes (variables)**
```
df.columns
```

**Afficher les 5 premieres observations**
```
df.take(5)
```

**Filtrer le df**
```
df.filter(df.Couleur == 'R').show()
```

## Ajouter le package spark-avro

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

## Lecture du fichier csv

**Ouverture du fichier csv**
```
val df = spark.read.option("header",true).csv("/tmp/data/infos.csv")
```

```
val df = sqlCtx.read.format("csv").option("header", "true").option("interSchema", "true").load("/home/papealygaye/Desktop/Ventes.csv")
```

**Afficher le df sous forme de tableau**
```
co.show()
```

## Jointure des 2 Dataframes
```
co.join(co2, co("ID") === co2("ID")).show()
```

```
co.filter("Nom == GAYE").join(co2, co("ID") === co2("ID")).show()
```

### Ecriture et lectures de fichier parquet

**Ecriture**
```
o.write.parquet("/tmp/data/infos.parquet")
```

**Lecture**
```
val df = spark.read.parquet("/tmp/data/infos.parquet")
```

**Voir les types de chaque variable**
```
df.printSchema()
```

**Faire une projection comme dans sql**
```
df.select(df("Nom"), df("Age")).show()
```

**...**
```
df.select("Nom", "Age").show()
```

**...**
```
./bin/spark-submit  --master local /tmp/data/fantoir.py
```

### Convertir un Dataframe en RDD
**Importatuion de SQLContext
```
import org.apache.spark.sql.SQLContext
```

**Creation du Dataframe**
```
val df = sqlCtx.read.format("csv").option("header", "true").option("interSchema", "true").load("/home/papealygaye/Desktop/Ventes.csv")
```

**Convertion du Dataframe en RDD**
```
val dfToRdd = df.rdd
```

**Convertion du typr Row du RDD en type String**
```
val dfToRddStr = df.select("codeClient").map(value => value.toString()).rdd
```

**Mapping**
```
val rddMap = dfToRddStr.map(x => (x, 1))
```

**reduce**
```
val rddRed = rddMap.reduceByKey(_+_)
```
