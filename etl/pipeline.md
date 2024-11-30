# ETL

Ce pipeline ETL permet de **nettoyer**, **transformer** et **indexer** des données comme NOAA (ou d'autres données), en tirant parti des puissantes capacités de **HDFS**, **Spark**, **Hive**, **HBase** et **Elasticsearch**.

### **Architecture et pipeline avec Spark ETL pour les données NOAA (température)**

1. **Stockage des données brutes dans HDFS** :  
   - Les fichiers NOAA (par exemple, avec des températures) sont chargés dans HDFS. Exemple :  
     - **`hdfs://namenode:9000/noaa/raw/noaa_temperature_data.csv`**

2. **Script Spark ETL pour nettoyage et transformation des données (exemple avec température)** :  
   - Le script **PySpark** est utilisé pour **nettoyer** et **transformer** les données, y compris la conversion des températures en **Celsius**.

   **Exemple de script PySpark pour l'ETL avec conversion de température** :

   ```python
   from pyspark.sql import SparkSession
   from pyspark.sql.functions import col

   # Créer une session Spark
   spark = SparkSession.builder \
       .appName("NOAA Temperature ETL") \
       .getOrCreate()

   # Charger les données brutes depuis HDFS
   df = spark.read.csv("hdfs://namenode:9000/noaa/raw/noaa_temperature_data.csv", header=True, inferSchema=True)

   # Afficher les premières lignes pour vérifier les données
   df.show(5)

   # Nettoyage des données : Suppression des lignes avec des valeurs manquantes
   df_clean = df.dropna()

   # Transformation des données : Conversion des températures de Fahrenheit à Celsius
   df_clean = df_clean.withColumn("temperature_celsius", (col("temperature_fahrenheit") - 32) * 5/9)

   # Afficher les données transformées pour vérifier
   df_clean.show(5)

   # Sauvegarde des données nettoyées et transformées dans HDFS
   df_clean.write \
       .format("csv") \
       .option("header", "true") \
       .mode("overwrite") \
       .save("hdfs://namenode:9000/noaa/clean_temperature/")
   ```

   **Explication du script** :
   - Le script charge les données de température depuis HDFS.
   - Il **supprime les lignes avec des valeurs manquantes** (si nécessaire).
   - Il **convertit la température** de **Fahrenheit à Celsius**.  
   - Les données transformées sont ensuite **enregistrées dans HDFS**.

3. **Traitement des données avec Hive** :  
   - Après le nettoyage et la transformation des données, vous pouvez les charger dans **Hive** pour les interroger via SQL.  
   - Hive permet d’effectuer des requêtes SQL sur les données stockées dans **HDFS**.

4. **Stockage des données dans HBase** :  
   - Vous pouvez également **stockez certaines données** dans **HBase** pour un accès rapide et des recherches basées sur des clés.
   - Les **HFiles** générés sont stockés dans **HDFS**.

5. **Indexation avec Elasticsearch (connecteur HDFS)** :  
   - Grâce au **connecteur Elasticsearch-HDFS**, vous pouvez **indexer les données transformées** dans Elasticsearch pour une recherche rapide.

   Exemple de commande avec le connecteur Elasticsearch-HDFS pour indexer les données transformées :

   ```bash
   ./bin/elasticsearch-hadoop --input hdfs://namenode:9000/noaa/clean_temperature/ --output hdfs://namenode:9000/noaa/elasticsearch_temperature_index
   ```

---

### **Synthèse du pipeline ETL**

1. **HDFS** :  
   - Contient les **données brutes** (avant transformation) et les **données nettoyées et transformées** (après conversion de température).
   
2. **Spark ETL** :  
   - Traite les données (nettoyage, transformation, conversion de température) avec PySpark.  
   - Sauvegarde les données nettoyées et transformées dans HDFS.

3. **Hive** :  
   - Permet de requêter les données via SQL (en accédant aux données stockées dans HDFS).

4. **HBase** :  
   - Pour un **accès rapide** aux données en temps réel avec des **HFiles** dans HDFS.

5. **Elasticsearch** :  
   - **Indexe** les données pour des recherches avancées, avec la possibilité de stocker les index dans HDFS via le connecteur Elasticsearch-HDFS.

---

### **Exécution du script ETL** :  

1. **Placer les données brutes dans HDFS** :
   ```bash
   hdfs dfs -mkdir -p /noaa/raw/
   hdfs dfs -put noaa_temperature_data.csv /noaa/raw/
   ```

2. **Exécuter le script Spark ETL** :
   ```bash
   spark-submit --master yarn --deploy-mode cluster /home/user/etl_temperature.py
   ```

3. **Vérifier les résultats dans HDFS** :
   ```bash
   hdfs dfs -ls /noaa/clean_temperature/
   ```

4. **Utiliser Hive et HBase pour des requêtes et un accès rapide** :
   - Vous pouvez exécuter des requêtes Hive sur les données transformées ou accéder rapidement aux données via HBase.

5. **Indexation dans Elasticsearch** :
   - Si nécessaire, indexez les données transformées dans Elasticsearch pour des recherches avancées.




