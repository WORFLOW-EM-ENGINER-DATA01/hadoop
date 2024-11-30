# ETL 

## **1. Cluster Hadoop avec Spark intégré**
### **Où exécuter ?**
- Sur un cluster Hadoop où Spark est installé.
- Habituellement, le script ETL est déployé depuis une **machine de soumission** (souvent appelée *gateway node* ou *edge node*) dans le cluster.

### **Processus :**
1. Placez le script Python sur le **node de soumission**.
2. Utilisez la commande `spark-submit` pour envoyer le travail au cluster :
   ```bash
   spark-submit --master yarn --deploy-mode cluster etl_noaa.py
   ```
   - `--master yarn` : Spark utilise YARN pour gérer les ressources du cluster Hadoop.
   - `--deploy-mode cluster` : Le script est exécuté directement sur le cluster, pas sur la machine locale.

3. Les nœuds du cluster traiteront les données, et les résultats seront stockés sur HDFS.

---

## **2. Plateforme Cloud**
Si vous utilisez un service cloud (AWS, Azure, GCP), voici où exécuter ce script :

### **a. AWS (Amazon Web Services)**
- Utilisez **Amazon EMR** (Elastic MapReduce) pour configurer un cluster Hadoop/Spark.
- Déployez le script via l'interface AWS CLI ou directement dans le cluster.
  ```bash
  spark-submit --master yarn --deploy-mode cluster etl_noaa.py
  ```
- Les données NOAA peuvent être stockées sur **Amazon S3**, et les résultats nettoyés également.

### **b. Google Cloud Platform (GCP)**
- Utilisez **Dataproc**, le service géré de Google pour Spark et Hadoop.
- Le script peut être exécuté à partir d'une machine d'administration ou directement dans le cluster Dataproc.
- Les données peuvent être chargées depuis et vers **Google Cloud Storage** (GCS).

### **c. Microsoft Azure**
- Utilisez **Azure HDInsight** pour gérer votre cluster Hadoop/Spark.
- Le script peut être soumis via Azure CLI ou directement à partir d'une interface Web.
- Les fichiers peuvent être stockés dans **Azure Blob Storage**.

---

## **3. Infrastructure Big Data spécifique**
Certaines entreprises possèdent leurs propres environnements Big Data. Voici des options fréquentes :

### **a. Databricks**
- Databricks est une plateforme collaborative pour Spark.
- Vous pouvez y exécuter le script dans un **notebook** Databricks ou via le gestionnaire de tâches.
- Les données NOAA peuvent être importées dans le stockage géré par Databricks, comme AWS S3 ou Azure Blob Storage.

### **b. Cloudera/Hortonworks**
- Ces distributions Hadoop/Spark offrent une interface utilisateur comme Cloudera Manager ou Ambari.
- Le script ETL est soumis à partir d’un terminal ou d’une machine de soumission intégrée au cluster.

---

## **4. Docker/Kubernetes**
- Si vous utilisez une infrastructure basée sur **conteneurs** :
  - Déployez un cluster Hadoop/Spark à l'aide de Docker Compose ou Kubernetes.
  - Montez le script ETL dans le conteneur de soumission Spark :
    ```bash
    docker run --rm -v $(pwd):/scripts spark-image \
      spark-submit --master spark://spark-master:7077 /scripts/etl_noaa.py
    ```

---

## **Conclusion**
- Si vous travaillez sur un **cluster Hadoop/Spark**, le script sera exécuté via une **machine de soumission** connectée au cluster.
- Si vous utilisez une plateforme **cloud**, le script est déployé via l’interface ou des outils en ligne de commande spécifiques.
- Les données seront traitées dans le cluster, et les résultats seront stockés dans HDFS ou un stockage cloud tel qu’Amazon S3 ou Google Cloud Storage.
