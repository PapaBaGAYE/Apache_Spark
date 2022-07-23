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
sudo docker compose up
```

```
sudo docker exec -it docker-spark_master_1 /bin/bash
```
<hr>

**Utiliser scala**

```
spark-shell
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
