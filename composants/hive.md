# Hive QL

### **1. Fonctionnement de Hive avec HDFS :**
- **HDFS (Hadoop Distributed File System)** est le système de stockage distribué de Hadoop.
- Hive s'appuie sur HDFS pour **stocker les données brutes** (comme des fichiers CSV, JSON, Parquet, ORC, etc.).
- Ces fichiers sont généralement volumineux et répartis sur plusieurs nœuds du cluster Hadoop.
- Hive ne déplace pas ou ne copie pas les données dans un autre système : il travaille directement avec les fichiers présents sur HDFS.

---

### **2. Requêtes SQL avec Hive :**
- Hive propose une interface appelée **HiveQL** (un dialecte de SQL).
- Cela permet d'exécuter des requêtes SQL comme :
  - `SELECT`, `JOIN`, `GROUP BY`, etc., pour analyser les données.
  - Les requêtes SQL sont **converties en jobs MapReduce**, Tez, ou Spark par Hive, selon le moteur configuré.
- Ces jobs sont exécutés sur le cluster Hadoop pour traiter les données.

---

### **3. Métadonnées stockées dans une base de données relationnelle :**
- Hive ne gère pas uniquement les fichiers, il a aussi besoin de stocker des **métadonnées** comme :
  - Les définitions des tables (`CREATE TABLE`, colonnes, types de données, etc.).
  - Les partitions.
  - Les emplacements des fichiers sur HDFS.
  - Les formats des données (texte, Parquet, etc.).
- Ces métadonnées sont stockées dans un composant appelé **metastore**, qui nécessite une **base de données relationnelle** (comme MySQL, PostgreSQL, MariaDB, etc.).
- Par défaut, Hive utilise **Apache Derby**, une base relationnelle embarquée, pour stocker ces métadonnées en mode local.

---

### **Flux simplifié du fonctionnement de Hive :**
1. **Les données brutes** sont stockées dans HDFS.
2. **Les définitions des tables et métadonnées** sont enregistrées dans une base relationnelle via le metastore.
3. Lorsqu'une requête SQL est exécutée :
   - Hive consulte le metastore pour comprendre la structure des tables.
   - Il génère un plan d'exécution (MapReduce, Tez ou Spark).
   - Les données sont traitées directement depuis HDFS.
4. Les résultats sont retournés sous forme de table.

---

### **Illustration : Exemple concret**
#### Table Hive :
```sql
CREATE TABLE sales (
    id INT,
    product STRING,
    amount DOUBLE
)
STORED AS PARQUET
LOCATION '/user/hive/warehouse/sales';
```

- Les fichiers des ventes sont stockés dans HDFS au chemin `/user/hive/warehouse/sales`.
- La définition de la table (nom, colonnes, types) est stockée dans le metastore (dans une base relationnelle comme MySQL).
- Lorsque vous exécutez une requête comme :
  ```sql
  SELECT product, SUM(amount) 
  FROM sales 
  GROUP BY product;
  ```
  - Hive lit les métadonnées dans le metastore.
  - Il génère un job MapReduce pour lire les fichiers depuis HDFS et agréger les données.

---

### **Conclusion**
- **HDFS** : Stocke les données brutes distribuées.
- **HiveQL** : Fournit une interface SQL pour interroger ces données.
- **Metastore (base relationnelle)** : Stocke les définitions de tables et les métadonnées.

Hive est un outil puissant pour travailler avec des données volumineuses dans un cluster Hadoop tout en utilisant une syntaxe SQL familière.