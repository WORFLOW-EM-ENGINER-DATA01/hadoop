# resume
---

1. **Hive** :  
   - Hive **n'est pas un système de stockage**.  
   - Il offre une **interface SQL** pour requêter les données **stockées dans HDFS**.  
   - Les données restent toujours dans HDFS, et Hive gère des métadonnées dans son metastore pour les référencer.

2. **HBase** :  
   - HBase est une **base NoSQL** qui organise les données sous forme de **HFiles**, physiquement stockés dans **HDFS**.  
   - Il permet un accès rapide via des clés, tout en utilisant HDFS comme stockage sous-jacent.

3. **Elasticsearch avec connecteur HDFS** :  
   - Elasticsearch est un **moteur de recherche distribué** qui **indexe des données** pour permettre des recherches avancées.  
   - Grâce au **connecteur Elasticsearch-HDFS**, les index Elasticsearch peuvent être **sauvegardés dans HDFS**.  
   - Cela permet de centraliser les données d'indexation dans le même espace de stockage distribué qu’HDFS, tout en exploitant la puissance d'Elasticsearch pour les recherches.

---

### Synthèse adaptée à votre cas
- **HDFS** reste le **système de stockage central** pour toutes vos données, qu'elles soient brutes, nettoyées ou indexées.  
- **Hive** permet de requêter les données HDFS via une interface SQL.  
- **HBase** organise et accède rapidement aux données avec des HFiles dans HDFS.  
- **Elasticsearch**, avec son connecteur HDFS, **stocke ses index dans HDFS**, permettant une intégration transparente avec l’écosystème Hadoop.
