# Velos_Lyon_HDFS
TP : Data Lake Vélo Lyon - Pipeline temps réel avec Hadoop / MapReduce / Kafka / Hive


## J'ai préparé mon environnement :

```
👉 Un environnement prêt avec :

Hadoop (HDFS + MapReduce)
Kafka
Hive
Atlas

👉 Le tout via Docker Compose
```

J'ai donc : 

-créé le projet sur github
-git copy
-créé mon environnement python "python3 -m venv .venv" et "source .venv/bin/activate"
-créé ma branche develop
-créé les dossiers (data, hadoop, hive kafka, scripts)
-créé mon docker-compose.yml
-lancé le "docker compose up -d" ce qui m'a créé des images et build et run les containers

Containers : 
[ Kafka ]        → transport données
[ Zookeeper ]    → nécessaire pour Kafka
[ Hadoop ]       → stockage + calcul
[ Hive ]         → SQL
[ Atlas ]        → gouvernance

Pour la partie MapReduce je prévois d'utiliser Hadoop Streaming Python et donc écrire les scripts Python qui seront utilisés comme Mapper/Reducer par Hadoop.

## Interfaces web (très utile)

👉 Ouvre dans ton navigateur :

Hadoop : http://localhost:9870
Atlas : http://localhost:21000

## 🧪 7. Tester HDFS

👉 Entrer dans le container namenode :

```
docker exec -it velos_lyon_hdfs-namenode-1 bash
```

Puis :

```
hdfs dfs -ls /
```

## 📁 8. Créer le Data Lake (TRÈS IMPORTANT)

Toujours dans le container :

```
hdfs dfs -mkdir -p /data-lake/raw
hdfs dfs -mkdir -p /data-lake/processed
hdfs dfs -mkdir -p /data-lake/analytics
```

Vérifie :

```
hdfs dfs -ls /data-lake 
```

## 🧠 1. Où sont vraiment les données ? (HDFS / Namenode / Datanode)
❓ J'ai créé les dossiers… mais où ?

👉 Quand je fais :

hdfs dfs -mkdir /data-lake/raw

👉 Je travaille avec HDFS, pas directement avec un container.

🧱 Architecture réelle (important à comprendre)
```
           [ NameNode ]
        (cerveau / metadata)
                │
        ┌───────┴────────┐
        │                │
 [ DataNode 1 ]   [ DataNode 2 ]
   (stockage)        (stockage)
```

✅ Donc :

👉 J'ai créé les dossiers dans :

✔️ HDFS (le système global)
✔️ Physiquement stockés dans les DataNodes
✔️ Référencés par le NameNode

🧠 2. HDFS = Data Lake ? (comparaison Snowflake)

🔹 Oui… MAIS 👇
```
HDFS	Snowflake
Stockage brut	Data warehouse
Pas SQL natif	SQL direct
Distribué	Cloud
Flexible	Structuré
```

🧠 Vision simple

👉 HDFS = disque dur géant
👉 Hive = SQL dessus

```
HDFS (data lake)
   ↓
Hive (SQL)
   ↓
Analyses
```

👀 3. Est-ce que je peux voir les données ?

🔹 3 façons

1. Ligne de commande (principal)
```
hdfs dfs -ls /data-lake/raw
hdfs dfs -cat fichier.json
```
2. Interface Web Hadoop ✅ 

👉 http://localhost:9870

Je peux :

-naviguer dans les dossiers
-voir les fichiers
-télécharger

3. (Optionnel) outils externes
Hue
VS Code extensions

