
### Connecting to GCP compute instance
```bash
gcloud auth login
gcloud config set project astral-outpost-413713
gcloud beta compute ssh --zone "us-west4-b" "instance-20240228-231132"  --project "astral-outpost-413713"
```

### Install
#### Java
```bash
mkdir spark && cd spark
wget https://download.java.net/java/GA/jdk11/9/GPL/openjdk-11.0.2_linux-x64_bin.tar.gz
tar xzfv openjdk-11.0.2_linux-x64_bin.tar.gz
export JAVA_HOME="${HOME}/spark/jdk-11.0.2"
export PATH="${JAVA_HOME}/bin:${PATH}"
java --version
rm openjdk-11.0.2_linux-x64_bin.tar.gz
```

#### Spark
```bash
wget https://archive.apache.org/dist/spark/spark-3.3.2/spark-3.3.2-bin-hadoop3.tgz
tar xzfv spark-3.3.2-bin-hadoop3.tgz
rm spark-3.3.2-bin-hadoop3.tgz
export SPARK_HOME="${HOME}/spark/spark-3.3.2-bin-hadoop3"
export PATH="${SPARK_HOME}/bin:${PATH}"
```
Testing Spark insallation using spark-shell:
```scala
spark-shell
val data = 1 to 10000
val distData = sc.parallelize(data)
distData.filter(_ < 10).collect()
```

### PySpark
```bash
ls ${SPARK_HOME}/python/lib/ | grep py4j
export PYTHONPATH="${SPARK_HOME}/python/:$PYTHONPATH"
export PYTHONPATH="${SPARK_HOME}/python/lib/py4j-0.10.9.5-src.zip:$PYTHONPATH"
```

#### Testing
```bash
cd ~/notebooks
wget https://s3.amazonaws.com/nyc-tlc/misc/taxi+_zone_lookup.csv
jupyter notebook
```
```python
import pyspark
from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .master("local[*]") \
    .appName('test') \
    .getOrCreate()

df = spark.read \
    .option("header", "true") \
    .csv('taxi+_zone_lookup.csv')

df.show()
```