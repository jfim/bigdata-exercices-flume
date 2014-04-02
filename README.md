# Exercices avec Flume

Un exemple de fichier de configuration de Flume:

```
monagent.sources = recepteur
monagent.sinks = evier
monagent.channels = canal

monagent.sources.recepteur.type = netcat
monagent.sources.recepteur.bind = 0.0.0.0
monagent.sources.recepteur.port = 44419
monagent.sources.recepteur.channels = canal
monagent.sources.recepteur.max-line-length = 65535
monagent.sources.recepteur.ack-every-event = false

monagent.channels.canal.type = memory
monagent.channels.canal.capacity = 50000
monagent.channels.canal.transactionCapacity = 100

monagent.sinks.evier.type = hdfs
monagent.sinks.evier.channel = canal
monagent.sinks.evier.hdfs.path = hdfs://127.0.0.1/user/bigdata/flume-%y%m%dT%H%M
monagent.sinks.evier.hdfs.rollSize = 0
monagent.sinks.evier.hdfs.rollCount = 0
monagent.sinks.evier.hdfs.idleTimeout = 30
monagent.sinks.evier.hdfs.minBlockReplicas = 1
monagent.sinks.evier.hdfs.useLocalTimeStamp = true
monagent.sinks.evier.hdfs.fileType = DataStream
monagent.sinks.evier.hdfs.round = true
monagent.sinks.evier.hdfs.roundValue = 60
monagent.sinks.evier.hdfs.roundUnit = minute
monagent.sinks.evier.hdfs.writeFormat = Text
```

On peut démarrer Flume avec:

```
flume-ng agent -n monagent -f flumeconfig.conf
```

## Questions

1. Selon vous, est-ce que Hive supporte les clefs uniques?
2. Considérant la réponse précédente, comment feriez vous pour importer un ensemble de données qui peut contenir des valeurs avec des clefs identiques à celles qui sont déjà dans la table?
3. Les données du premier janvier 2014 sont disponibles ici : https://github.com/jfim/bigdata-exercices-flume/raw/master/On_Time_On_Time_Performance_2014_1_1.tsv.bz2
Créez une copie de vos tables de dimension, puis écrivez les requêtes nécessaires dans Hive pour charger les données du premier janvier dans une nouvelle table de faits. Pour extraire les données sur la machine Linux, vous pouvez utiliser la commande bunzip2.
4. Créez un workflow Oozie qui permet de charger ces données. Vous pouvez passer des paramètres dans votre workflow Oozie, par exemple LOAD DATA INPATH '${INPUT}' INTO TABLE ontime; va charger les fichiers d'entrée que Oozie trouve.
5. Créez un coordonateur Oozie qui va appeler votre workflow Oozie avec un ensemble de données récurrent.
6. Créez un fichier de configuration Flume qui écrit dans un répertoire sur HDFS. Vous pouvez vous inspirer du fichier ci-haut, mais vous aurez probablement à changer la fréquence à laquelle les nouveaux répertoires sont crées.
7. Oozie ne permet pas de spécifier des fichiers à exclure, mais permet de spécifier des ensembles de données relatifs (par exemple, l'ensemble de données précédent ou celui d'il y a cinq minutes). Comment allez-vous faire pour éviter que Oozie charge un fichier dans lequel Flume est en train d'écrire?
8. Démarrez Flume avec votre agent. Demandez au professeur de vous envoyer les évènements du premier janvier pour tester votre pipeline de données.
9. Si votre pipeline de données fonctionne, demandez au professeur de vous envoyer le mois de janvier au complet.
