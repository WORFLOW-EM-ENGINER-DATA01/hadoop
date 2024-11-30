## **Architecture globale**
### **1. Composants clés :**
- **Hadoop (HDFS)** : Sert de stockage principal pour les données brutes.
- **Spark** : Effectue le traitement des données (batch ou streaming).
- **Hive** : Fournit une interface SQL pour interroger les données stockées et transformées.
- **HBase** : Stocke les données pour un accès rapide en temps réel.
- **Elasticsearch** : Permet la recherche rapide et l'indexation des données enrichies.
- **API (Express, FastAPI ou Spring Boot)** : Interface entre les utilisateurs/clients et les composants backend.

---

## **Étapes pour développer votre API**
### **1. Définir les endpoints de l'API**
Voici des exemples d'endpoints que vous pourriez exposer :
- **/data/upload** : Charger des données brutes dans HDFS.
- **/data/transform** : Lancer un job Spark pour transformer les données.
- **/data/query/hive** : Exécuter des requêtes SQL sur Hive.
- **/data/query/hbase** : Lire/écrire des données sur HBase.
- **/data/search** : Effectuer une recherche dans Elasticsearch.

---

### **2. Technologies pour l'API**
- **Langage** : Python (avec FastAPI/Flask) ou Java (avec Spring Boot).
- **Connecteurs** :
  - Hadoop : `pyarrow` ou `hdfs` pour Python ; `org.apache.hadoop` pour Java.
  - Hive : `pyhive` ou JDBC.
  - Spark : `pyspark` ou Spark JDBC.
  - HBase : `happybase` pour Python ; `hbase-client` pour Java.
  - Elasticsearch : `elasticsearch-py` ou `Elasticsearch Java API`.

---

### **3. Développement de l'API**
#### **Exemple en Python avec FastAPI**
1. **Installation des bibliothèques** :
   ```bash
   pip install fastapi uvicorn pyspark pyhive happybase elasticsearch hdfs
   ```

2. **Structure du projet** :
   ```
   /api_project/
       ├── app/
       │   ├── main.py             # Point d'entrée de l'API
       │   ├── hdfs_utils.py       # Interactions avec HDFS
       │   ├── spark_utils.py      # Jobs Spark
       │   ├── hive_utils.py       # Interactions Hive
       │   ├── hbase_utils.py      # Interactions HBase
       │   └── elastic_utils.py    # Interactions Elasticsearch
       └── requirements.txt        # Dépendances
   ```

3. **Code pour l'API**
Voici un exemple pour quelques endpoints :

**a. Point de départ : `main.py`**
```python
from fastapi import FastAPI
from app import hdfs_utils, spark_utils, hive_utils, hbase_utils, elastic_utils

app = FastAPI()

@app.get("/health")
def health_check():
    return {"status": "API is running"}

@app.post("/data/upload")
def upload_to_hdfs(file_path: str):
    return hdfs_utils.upload(file_path)

@app.post("/data/transform")
def transform_data():
    return spark_utils.run_job()

@app.get("/data/query/hive")
def query_hive(query: str):
    return hive_utils.run_query(query)

@app.get("/data/search")
def search_elasticsearch(index: str, query: dict):
    return elastic_utils.search(index, query)
```

**b. Interactions avec HDFS : `hdfs_utils.py`**
```python
from hdfs import InsecureClient

client = InsecureClient('http://namenode:50070', user='hadoop')

def upload(file_path):
    with open(file_path, 'rb') as file_data:
        client.write('/data/' + file_path.split('/')[-1], file_data)
    return {"status": "Uploaded to HDFS"}
```

**c. Job Spark : `spark_utils.py`**
```python
from pyspark.sql import SparkSession

def run_job():
    spark = SparkSession.builder \
        .appName("Data Transformation Job") \
        .getOrCreate()

    df = spark.read.csv("hdfs://namenode:9000/data/input.csv", header=True)
    df_transformed = df.filter(df["status"] == "active")
    df_transformed.write.mode("overwrite").parquet("hdfs://namenode:9000/data/output")
    return {"status": "Job completed"}
```

**d. Requête Elasticsearch : `elastic_utils.py`**
```python
from elasticsearch import Elasticsearch

es = Elasticsearch(['http://elasticsearch-host:9200'])

def search(index, query):
    response = es.search(index=index, body=query)
    return response
```

---

### **4. Tester et déployer l'API**
1. **Lancement de l'API** :
   ```bash
   uvicorn app.main:app --host 0.0.0.0 --port 8000
   ```

2. **Tester avec Postman ou cURL** :
   - Charger un fichier :
     ```bash
     curl -X POST "http://localhost:8000/data/upload?file_path=/path/to/file.csv"
     ```
   - Rechercher dans Elasticsearch :
     ```bash
     curl -X GET "http://localhost:8000/data/search?index=my-index&query={...}"
     ```

3. **Déploiement** :
   - Utilisez Docker pour containeriser l'API.
   - Déployez l'API sur un serveur (VM, Kubernetes, ou service cloud comme AWS/GCP).

---

### **Conseils pour une intégration fluide**
1. **Gestion des logs** : Configurez un système de logs pour monitorer les requêtes et les erreurs.
2. **Sécurité** : Ajoutez une authentification à l’API (JWT ou OAuth).
3. **Optimisation** : Utilisez des caches (Redis) pour réduire les appels répétés aux composants.
4. **Scalabilité** : Servez l’API via un gestionnaire comme **NGINX** ou un service cloud (AWS Lambda, Azure Functions).

