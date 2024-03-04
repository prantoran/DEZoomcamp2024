
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
- View Spark jobs at localhost:4040

## Dataset
- https://github.com/DataTalksClub/nyc-tlc-data


## Google Cloud Storage
### Pushing to GCS
```bash
gcloud auth login
gsutil -m cp -r pq/ gs://<bucketname>/pq
```

### Reading from GCS using Spark
#### Setup Setup GCS Connector for Hadoop
- Download [Cloud Storage connector for Hadoop 3.x](https://cloud.google.com/dataproc/docs/concepts/connectors/cloud-storage#non-clusters).
- Alternative is to download using gsutil.
```bash
cd ..
mkdir lib
cd lib/
gsutil cp gs://hadoop-lib/gcs-connector-hadoop3-latest.jar gcs-connector-hadoop3-latest.jar
```

```bash
BUCKET="hadoop-lib"
KEY="gcs/gcs-connector-hadoop3-2.2.5.jar"
URL="https://storage.googleapis.com/${BUCKET}/${KEY}"
wget ${URL}
```

## Local Cluster and Spark-Submit

Creating a stand-alone cluster ([docs](https://spark.apache.org/docs/latest/spark-standalone.html)):

```bash
cd $SPARK_HOME
./sbin/start-master.sh
```

Visit localhost:8080 in the browser.

Start works using:

```bash
URL="spark://queen:7077"
./sbin/start-worker.sh ${URL}
```

Turn the notebook into a script:

```bash
jupyter nbconvert --to=script 08_local_spark_cluster.ipynb
```


Connect to locally running spark cluster (1 executor + 1 worker):
```bash
import pyspark
from pyspark.sql import SparkSession
from pyspark.sql import functions as F

parser = argparse.ArgumentParser()

input_green = args.input_green
input_yellow = args.input_yellow
output = args.output


spark = SparkSession.builder \
    .master("spark://queen:7077") \
    .appName('test') \
    .getOrCreate()
```


### Spark Submit
- For avoiding to hard-code master URL in code.

```bash
URL="spark://queen:7077"

# https://spark.apache.org/docs/latest/submitting-applications.html
spark-submit \
    --master ${URL} \
    08_local_spark_cluster.py \
        --input_green "data/pq/green/2021/*" \
        --input_yellow "data/pq/yellow/2021/*" \
        --output=data/report-2021
```

## Stop all the workers and executors
```bash
cd $SPARK_HOME
./sbin/stop-worker.sh
./sbin/stop-master.sh
```

## Debugging
- In case a port is unavailable, then kill the worker process and create a new worker
- For finding the worker process id, run the create command which will indicate the running worker's process id.
```bash
URL="spark://queen:7077" 
./sbin/start-worker.sh ${URL}             
# org.apache.spark.deploy.worker.Worker running as process 88370.  Stop it first.

kill 88370

URL="spark://queen:7077" 
./sbin/start-worker.sh ${URL}
# starting org.apache.spark.deploy.worker.Worker, logging to /home/pinku/spark/spark-3.3.2-bin-hadoop3/logs/spark-pinku-org.apache.spark.deploy.worker.Worker-1-queen.out
```
Better:
```bash
cd $SPARK_HOME
./sbin/stop-workers.sh
```


