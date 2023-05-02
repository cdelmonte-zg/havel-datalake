## Einführung

In diesem Repository wird die Evaluierung von Havel, einer prototypischen Data-Lake-Architektur-Lösung, vorgestellt. Dies geschieht durch eine modellhafte Implementierung der Hauptfunktionalitäten des Data Lake Havel und die Durchführung von Experimenten, die die Haupteigenschaften des Prototyps testen. Insbesondere wurden Data Discovery, Data Exploration und Data Curating geprüft. Daten-Ingestion und Daten-Integration sind nicht Gegenstand dieser Arbeit und werden daher im Jupyter Notebook durch die Verwendung von csv-Dateien simuliert, die als Ersatz für die aus der Daten-Integration resultierenden Parquet-Dateien dienen. Das csv-Format wurde gewählt, um das Lesen und die mögliche Überprüfung oder den Vergleich der verwendeten Daten zu erleichtern. 

Die Tests erfolgten in einer Jupyterlab Umgebung, die auf dem Master-Knoten der Spark- und Hadoop-Cluster installiert worden ist. Das Notebook enthält eine prototypische Implementierung der Grundfunktionen von Havel in Python, die so organisiert ist, dass sie die vorgeschlagene Architektur weitgehend simuliert. Unterschiede sind nur dort vorhanden, wo dies aus Gründen der vom Jupyter Notebook angebotenen Entwicklungsplattform erforderlich ist. Diese haben jedoch keinen Einfluss auf die Experimente, die zur Überprüfung der Kernfunktionalitäten von Havel durchgeführt wurden. Die Unterschiede sind folgende:

- Die Analyse des Dateiinhalts wurde von der Ingestionsphase in die Wartungsphase des Data Lake verlagert. Dies ist darauf zurückzuführen, dass die Ingestionsphase im Prototyp nicht untersucht und daher nur simuliert wird. 
- Hinzu kommt, dass die Microservices-Architektur in diesem Prototyp nicht übernommen wurde. Dies liegt zum einen an der Art der Jupyter Notebooks und zum anderen an der Tatsache, dass die horizontale Skalierbarkeit des Systems nicht Gegenstand dieser Evaluierung ist.

Für die Implementierung des MinHash Algoritmus wurde die Python Library [Datasketch](https://github.com/ekzhu/datasketch) benutzt, die probabilistische Datenstrukturen zur Verfügung stellt, mit denen große Datenmengen ohne Genauigkeitsverlust äußerst schnell verarbeitet und durchsucht werden können.


## Installation und Konfiguration der Jupyter Notebook-Ausführungsumgebung

### Hadoop- und Spark Cluster

Die Ausführung des Jupyter Notebooks in diesem Repository erfordert ein Hadoop-Cluster in Verbindung mit einem Spark-Cluster. Im Folgenden zwei Anleitungen, die Schritt für Schritt beschreiben, wie diese installiert und konfiguriert werden können:
- [Build Raspberry Pi Hadoop/Spark Cluster from scratch](https://medium.com/analytics-vidhya/build-raspberry-pi-hadoop-spark-cluster-from-scratch-c2fa056138e0)
- [Building a Raspberry Pi Hadoop / Spark Cluster](https://dev.to/awwsmm/building-a-raspberry-pi-hadoop-spark-cluster-8b2)

Die Anleitungen beziehen sich auf einen Raspberry PI-basierten Cluster, sind jedoch auf jedes System anwendbar, auf dem das Betriebssystem Ubuntu läuft.

### Hive

In diesem Modell wird Apache Hive erstmal nicht verwendet. Wenn Sie es trotzdem installieren möchten, erfahren Sie in der folgenden Anleitung, wie Sie das tun können:
- [Big Data | Install and configure Apache Hive (HiveServer, Hive MetaStore)](http://www.mtitek.com/tutorials/bigdata/hive/install.php)

### Jupiterlab und PySpark

Das Modell setzt das Vorhandensein von Python >= v3.7 auf dem Master-Knoten des Spark-Clusters voraus. Jupiterlab muss ebenfalls auf demselben Knoten installiert sein und laufen.
Um Jupyterlab zu installieren und zu konfigurieren und den PySpark-Kernel zu aktivieren, der für die Ausführung des Jupyter-Notebooks erforderlich ist, finden Sie hier eine Anleitung:
- [How to set up PySpark for your Jupyter notebook](https://opensource.com/article/18/11/pyspark-jupyter-notebook)

Außerdem müssen die folgenden Python-Pakete in der virtuellen Python-Umgebung, in der Jupyterlab läuft, vorhanden sein:
- datasketch[redis]
- elasticsearch==7.10.0
- neo4j
- pyyaml
- deepdiff

### Andere Softwaresysteme
Es werden auch andere Softwaresysteme benötigt, die in unserem Modell in Container-Form verwendet und mit docker-compose bereitgestellt und verwaltet werden. 

Eine mögliche Konfiguration wird in der Datei [docker/docker-compose.yml](./docker/docker-compose.yml) vorgeschlagen, die in diesem Repository enthalten ist.
Für tika kann man die Konfigurationen und docker-compose files nutzen, die auf der offiziellen Apache Repository [https://github.com/apache/tika-docker](https://github.com/apache/tika-docker) verfügbar sind.
 
Die für die Ausführung des Jupyter Notebooks erforderlichen Docker-Container sind folgende:

- Elasticsearch (image: docker.elastic.co/elasticsearch/elasticsearch:7.10.1)
- Kibana (image: docker.elastic.co/kibana/kibana:7.10.1)
- Redis (image: redis:6.2-alpine)
- Neo4j (image: neo4j:5)
- Postgres (image: Postgres:15)
- Mongodb (image: mongo: 6.0.4)
- Tika (image: apache/tika:2.6.0.1-full)

Informationen über Docker, docker-compose und seine Verwendung finden Sie in der Anleitung in der offiziellen Dokumentation:
- [Docker](https://docs.docker.com/get-started/)
- [Try Docker Compose](https://docs.docker.com/compose/gettingstarted/)


### Firewalls Konfigurieren

Schließlich ist es notwendig, eventuell vorhandene Firewalls im Subnetz zu konfigurieren. Die Erörterung dieses Schritts geht über den Rahmen der vorgeschlagenen Arbeit hinaus und hängt stark von der Netzwerkinfrastruktur ab, in der sich die Havel befindet. Wir verweisen hierzu auf die Ressourcen, die von den einschlägigen Softwareherstellern und Open-Source-Communities zur Verfügung gestellt werden. Ein Beispiel bietet [https://learn.microsoft.com/de-de/windows/security/threat-protection/windows-firewall/best-practices-configuring](https://learn.microsoft.com/de-de/windows/security/threat-protection/windows-firewall/best-practices-configuring)

### Beispielhafter Infrastruktur-Stack

Der für die Tests vom Autor verwendete Infrastruktur-Stack sieht wie folgt aus:

- 1 Master Node Asus Zenbook Prime UX31A Ultrabook, 4GB RAM (DDR3 1600 MHz SDRAM), Intel Core i5 3317U, 128GB SSD
    - Jupyter Lab v3.3.2 mit PySpark Kernel, 
    - v3.2.4 Cluster, 
    - Apache Spark v3.3.1. Cluster
    - Apache Hive v3.1.3

- 3 Worker Nodes für Hadoop Cluster, Apache Spark Cluster: 3 Raspberry Pi 4 Model B, 4GB RAM (LPDDR4-2400-SDRAM), ARM-Cortex-A72 4x, SanDisk Extreme PRO microSDXC UHS-I Memory Card 64GB
    - Hadoop v3.2.4 Cluster, 
    - Apache Spark v3.3.1. Cluster

- 1 Master Node für andere Softwaresysteme, verfügbar als Docker Containers und konfiguriert mit Docker Compose: Dell Latitude 5511, 32GB RAM SO-DIMM DDR4 - 2666MHz, Intel i7-10850H, 512GB SSD
    - Elasticsearch (image: docker.elastic.co/elasticsearch/elasticsearch:7.10.1)
    - Kibana (image: docker.elastic.co/kibana/kibana:7.10.1)
    - Redis (image: redis:6.2-alpine)
    - Neo4j (image: neo4j:5)
    - Postgres (image: Postgres:15)
    - Mongodb (image: mongo: 6.0.4)
    - Tika (image: apache/tika:2.6.0.1-full)

## Konfiguration des Jupyter Notebooks

Im Jupyter Notebook muss die Klasse Config mit den Werten des Laufzeitsystems konfiguriert werden.

```sh
class Config:
    hadoop_uri = <uri im Format hdfs://url:port des Master-Knotens des Hadoop-Clusters, z.B.: 'hdfs://fonduta.fritz.box:9000'>
    
    search_service_host = <uri im Format http://ip-adresse:port von elasticsearch, z.B.: 'http://192.168.178.52:9200'>
    search_service_user = <Benutzername für die Authentifizierung/Autorisierung bei elasticsearch>
    search_service_password = <Passwort für die Authentifizierung/Autorisierung bei elasticsearch>
    
    graph_service_host = <uri im Format bolt://ip-adresse:port von Graph4j und Beispiel 'bolt://192.168.178.52:7687'>
    graph_service_user = <Benutzernamen für die Authentifizierung/Autorisierung bei graph4j>
    graph_service_password = <Passwort für die Authentifizierung/Autorisierung bei graph4j>
    
    data_description_service_host = <mongodb ip-Adresse, z.B. '192.168.178.52'>
    data_description_service_port = <mongodb Port, z.B. '27027'>
    data_description_service_host_db_prefix = <mongodb Datenbankname, z.B. 'havel'>
    data_description_service_username=<Benutzername für die Authentifizierung/Autorisierung bei mongodb>
    data_description_service_password=<Passwort für die Authentifizierung/Autorisierung bei mongodb>
    
    cache_host = <ip-Adresse von redis, z.B. '192.168.178.52'>
    cache_port = <redis-Port, z.B. '6379'> 
    cache_db = <Anzahl der Datenbanken in redis, z.B. '4'>
    
    content_analyzer_url_base = <HDFS-Endpunkt, der von Tika verwendet wird, um mit HDFS zu interagieren und Dateianalysen durchzuführen, z.B. 'http://master:50070/webhdfs/v1/'>
    
    abs_path = <absoluter Pfad zum Basisverzeichnis der Beispieldaten, z.B. '/sample-data'>    
```

Für tika empfehlen wir die Beibehaltung der Grundkonfiguration, die in der offiziellen docker-compose-Datei vorgeschlagen wird und die als Standard für den entsprechenden Python-Client verwendet wird: *docker-compose-tika-customocr.yml*.

## Konfiguration der Quellen

Eine yaml-Datei wird verwendet, um die Simulation der Ingestions- und Datenintegrationsprozesse zu konfigurieren. Ihre Beschreibung:

```sh
apiVersion: v1

sources:
    <Eindeutiger Name der Datenquelle>:
        description: <Beschreibung der Data Source>
        buckets:
          - name: <name des Buckets, eines logischen Behalters für die Datasets>
            version: <Version des Buckets> 
            url: <Directory in dem HDFS>
            datasets:
                - <Label des Datasets>:
                  name: <Name des Datasets>
                  description: <Beschreibung des Datasets>
                  keywords_whitelist: 
                    - <Liste von Attributen, die von dem Search Engine als Keywords indexiert werden duerfen>
                  attributes_blacklist:
                    - <Liste von Attributen, die für die Berechnung des Aenlichkeitskoeffizient ausgeschlossen werden muessen>

```

## Ausführung des Jupyter Notebooks

Nach der Konfiguration wie oben beschrieben kann das Jupyter Notebook ausgeführt werden.
Für Informationen zur Ausführung von Jupyter Notebooks empfiehlt sich das folgende Tutorial: [https://realpython.com/jupyter-notebook-introduction/](https://realpython.com/jupyter-notebook-introduction/)


## Benutzte Daten im Jupyter Notebook und ihr Vorbereitung

Für die Evaluierung wird „AdventureWorks for Postgres“ [https://github.com/lorint/AdventureWorks-for-Postgres](https://github.com/lorint/AdventureWorks-for-Postgres) als Datenquelle verwendet. Es handelt sich dabei um eine Beispieldatenbank mit 68 Tabellen, die in fünf Schemata organisierte Daten zu Personalwesen, Vertrieb, Produkten und Einkauf enthalten. Sie repräsentiert einen fiktiven Fahrradteile-Großhändler mit einer Hierarchie von fast 300 Mitarbeitern, 500 Produkten,
20.000 Kunden und 31.000 Verkäufen, die jeweils durchschnittlich vier Einträge habe. Für das Experiment wurde das Schema *person* übernommen. Es besteht aus den folgenden Tabellen:

- address (19614 Records)
- addresstype (6 Records)
- businessentity (20777 Records)
- businessentityaddress (19614 Records)
- businessentitycontact (999 Records)
- contacttype (20 Records)
- countryregion (238 Records)
- emailaddress (19972 Records)
- password (19972 Records)
- person (19972 Records)
- personphone (19972 Records)
- phonenumbertype (3 Records)
- stateprovince (181 Records)

Außerdem enthält Das Schema *person* die Sicht:
- vadditionalcontactinfo

Zusätzlich wurde der irreführende Datensatz „Human Resources Data Set“ [https://www.kaggle.com/datasets/rhuebner/human-resources-data-set](https://www.kaggle.com/datasets/rhuebner/human-resources-data-set) hinzugefügt. Dabei handelt es sich um einen Datensatz aus dem Human-Resources-Bereich, der am New England College of Business (Massachusetts) im Grundkurs „HR Metrics and Analytics“ verwendet wird, um den Studierenden beizubringen, wie sie Daten mit Tableau Desktop analysieren können [Tab20; HP10]. Der Datensatz von 311 Records betrifft ein fiktives Unternehmen und enthält die folgenden Eigenschaften:

- Employee_Name
- EmpID
- MarriedID
- MaritalStatusID
- GenderID
- EmpStatusID
- DeptID
- PerfScoreID
- FromDiversityJobFairID
- Salary
- Termd
- PositionID
- Position
- State
- Zip
- DOB
- Sex
- MaritalDesc
- CitizenDesc

Der Datensatz enthält hauptsächlich numerische Daten und wurde aufgenommen um zu testen, wie der Schwellenwert für den MinHash-Algorithmus eingestellt werden sollte, um das Rauschen in den Ergebnissen der Data Discovery in Havel zu reduzieren. Die oben aufgeführten Tabellen und Ansichten wurden in Dateien im CSV-Format exportiert, diese wurden in das verteilte HDFS-Dateisystem geladen. Die CSV-Dateien wurden von Havel analysiert, um daraus die relevanten
Metadaten zu extrahieren, die für die Data Discovery wichtig sind.

Die Daten sind im Ordner [./sample-data](./sample-data) bereitgestellt.

## Lizenz

Der Code wird unter der MIT-Lizenz veröffentlicht.
