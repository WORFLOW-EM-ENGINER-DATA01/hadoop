# Hébergements

Pour déployer et exécuter Hadoop efficacement, il est essentiel de choisir des serveurs adaptés aux charges de traitement et de stockage massives associées à cette technologie. Voici les recommandations générales pour les serveurs utilisés dans un cluster Hadoop : 

---

### **1. Serveurs maîtres :**
Ces serveurs hébergent les composants principaux, comme **NameNode** et **ResourceManager**, qui coordonnent le cluster.

- **CPU** : 4-8 cœurs minimum, plus si le cluster est grand.
- **RAM** : 16 à 32 Go (les processus de gestion nécessitent une bonne mémoire vive).
- **Disque** : SSD ou HDD rapide (pour stocker les métadonnées du système de fichiers HDFS).
- **Réseau** : Connexion rapide, au moins 1 Gbps, idéalement 10 Gbps.

---

### **2. Serveurs esclaves (datanodes, nodemanagers) :**
Ces serveurs effectuent le stockage des données (dans HDFS) et le traitement distribué (via YARN).

- **CPU** : 8-16 cœurs pour gérer les calculs distribués.
- **RAM** : 32 à 128 Go, en fonction des charges de travail.
- **Disque** : 
  - Plusieurs disques HDD (ou SSD pour les performances) avec des volumes de 1 To à plusieurs To chacun.
  - Hadoop utilise les disques de manière linéaire, donc plus il y a de disques, mieux c'est pour la performance.
- **Réseau** : 1 Gbps au minimum, idéalement 10 Gbps pour minimiser les goulots d'étranglement.

---

### **3. Stockage disque :**
- Hadoop repose sur le principe de **Data Locality** : les données sont stockées là où elles seront traitées. Prévoir un volume de stockage important pour les nœuds DataNode.
- Les configurations RAID ne sont généralement pas nécessaires pour Hadoop, car HDFS gère la redondance via la réplication des blocs (par défaut, 3 copies de chaque bloc).

---

### **4. Infrastructure réseau :**
- Réseau haute performance (1 Gbps est un minimum, 10 Gbps est préférable pour de grands clusters).
- Un switch réseau de haute qualité est nécessaire pour connecter les nœuds avec une faible latence.

---

### **5. Nombre de nœuds :**
- **Petits clusters** : 3-10 nœuds esclaves, 1-2 maîtres.
- **Clusters moyens** : 20-50 nœuds esclaves.
- **Grands clusters** : Plus de 100 nœuds esclaves.

---

### **6. Systèmes d'exploitation :**
Hadoop est conçu pour les systèmes UNIX/Linux. Les distributions populaires sont :
- **CentOS**, **Red Hat Enterprise Linux** (RHEL), ou **Ubuntu**.
- Configurer les serveurs pour une interconnexion SSH sans mot de passe.

---

### **7. Cloud ou on-premise ?**
- Les déploiements cloud (AWS EMR, Google Dataproc, Azure HDInsight) permettent de démarrer rapidement sans investir dans des serveurs physiques.
- Les déploiements sur site nécessitent un investissement initial important, mais offrent plus de contrôle.

