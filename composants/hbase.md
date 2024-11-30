### **Résumé de HBase**

HBase est une **base de données NoSQL distribuée** qui fonctionne au-dessus de **HDFS** (Hadoop Distributed File System). Contrairement à Hive, HBase est conçue pour des **opérations rapides en lecture et écriture** sur des données massives, avec un accès basé sur des **clés**.

---

### **1. Fonctionnement général d'HBase**
- **HBase stocke les données** : Contrairement à Hive, HBase gère directement le stockage et l'accès aux données, en utilisant HDFS comme système sous-jacent.
- Organisé en **tableaux (tables)** avec des lignes identifiées par des **row keys** (clés de ligne).
- Les colonnes sont regroupées en **families de colonnes**, ce qui permet de structurer les données efficacement.

---

### **2. Structure des données dans HBase**
- Les tables sont **sparse** (peuvent contenir beaucoup de colonnes vides).
- Une cellule est identifiée par :
  - Une **clé de ligne** (row key).
  - Un **nom de colonne** (column).
  - Une **timestamp** (facultatif), ce qui permet de stocker plusieurs versions d’une donnée.

Exemple :
| Row Key | Family:Column | Timestamp   | Value      |
|---------|---------------|-------------|------------|
| user1   | info:name     | 1695123456  | John Doe   |
| user1   | info:age      | 1695123456  | 29         |
| user2   | info:name     | 1695123456  | Jane Smith |

---

### **3. Fonctionnalités principales d’HBase**
- **Lecture et écriture rapides** :
  - Optimisé pour les opérations aléatoires à faible latence.
  - Fonctionne bien pour les requêtes par clé unique ou les plages de clés.
  
- **Scalabilité horizontale** :
  - Conçu pour des clusters massifs.
  - Ajout dynamique de nœuds pour gérer des volumes croissants.

- **Stockage de grandes quantités de données** :
  - Peut gérer des milliards de lignes et des millions de colonnes.
  - Adapté aux cas où les données ne sont pas uniformément structurées.

- **Prise en charge de versions multiples des données** grâce aux horodatages (timestamps).

---

### **4. Comparaison avec Hive**
| **Caractéristique**       | **HBase**                                      | **Hive**                                      |
|----------------------------|-----------------------------------------------|----------------------------------------------|
| **Type de base**           | NoSQL                                         | Entrepôt de données                          |
| **Usage principal**        | Lecture/écriture rapide de petites données.   | Analytique, requêtes SQL sur données massives. |
| **Stockage des données**   | Directement géré par HBase.                   | Données stockées dans HDFS, Hive y accède.   |
| **Structure des données**  | Clés/valeurs (NoSQL).                        | Tables relationnelles (SQL-like).            |
| **Moteur de requêtes**     | API Java ou Scans/gets simples.               | Requêtes SQL (HiveQL).                       |

---

### **5. Cas d'utilisation d’HBase**
- Gestion des **logs** ou des événements.
- Applications nécessitant un accès aléatoire rapide.
- Stockage de données semi-structurées ou dynamiques (comme les informations utilisateur dans des applications web).
- Historisation des données avec plusieurs versions.

---

### **6. Langages d'accès à HBase**
- Utilisation d'une **API Java** pour manipuler directement les tables.
- **HBase Shell** : Interface en ligne de commande pour interagir avec les tables.
- **Hive sur HBase** : Hive peut être configuré pour interroger des données HBase en SQL.

---

### **Conclusion**
- **HBase** : Une base NoSQL distribuée, optimisée pour les accès aléatoires rapides sur de grandes quantités de données, avec des schémas flexibles.
- **HDFS** : Sert de système de stockage sous-jacent à HBase.
- Contrairement à Hive (qui est davantage analytique), HBase est utilisé pour des opérations en temps réel ou des systèmes nécessitant des performances rapides en lecture/écriture.
