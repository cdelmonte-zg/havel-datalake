In diesem Repository wird die Evaluierung von Havel, einer prototypischen Data Lake Architektur-Loesung, vorgestellt. Dies geschieht durch eine modellhafte Implementierung der Hauptfunktionalitäten des Data Lake Havel und die Durchführung von Experimenten, die die Haupteigenschaften des Prototyps testen. Insbesondere wurden Data Discovery, Data Exploration und Data Curating geprüft.


Die Tests erfolgten in einer Jupyter-Lab Umgebung, die auf dem Master-Knoten der Spark- und Hadoop-Cluster installiert worden ist. Das Notebook enthält eine prototypische Implementierung der Grundfunktionen von Havel in Python, die so organisiert ist, dass sie die vorgeschlagene Architektur weitgehend simuliert. Unterschiede sind nur dort vorhanden, wo dies aus Gründen der vom Jupyter Notebook angebotenen Entwicklungsplattform erforderlich ist. Diese haben jedoch keinen Einfluss auf die Experimente, die zur Überprüfung der Kernfunktionalitäten von Havel durchgeführt wurden. Die Unterschiede sind folgende:

- Die Analyse des Dateiinhalts wurde von der Ingestionsphase in die Wartungsphase des Data Lake verlagert. Dies ist darauf zurückzuführen, dass die Ingestionsphase im Prototyp nicht untersucht und daher nur simuliert wird. 
- Hinzu kommt, dass die Microservices-Architektur in diesem Prototyp nicht übernommen wurde. Dies liegt zum einen an der Art der Jupyter-Notebooks und zum anderen an der Tatsache, dass die horizontale Skalierbarkeit des Systems nicht Gegenstand dieser Evaluierung ist.

Für die Implementierung des MinHash Algoritmus wurde die Python Library [Datasketch](https://github.com/ekzhu/datasketch) benutzt, die probabilistische Datenstrukturen zur Verfügung stellt, mit denen große Datenmengen ohne Genauigkeitsverlust äußerst schnell verarbeitet und durchsucht werden können.


Der für die Tests verwendete Infrastruktur-Stack sieht wie folgt aus:

- 1 Master Node} Asus Zenbook Prime UX31A Ultrabook, 4GB RAM (DDR3 1600 MHz SDRAM), Intel Core i5 3317U, 128GB SSD
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

Der Code wird unter der MIT-Lizenz veröffentlicht.
