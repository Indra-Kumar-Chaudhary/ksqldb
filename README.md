# ksqldb
Data streaming processing database 

1. Get ksqlDB 
Since ksqlDB runs natively on Apache Kafka, we'll need to have a Kafka installation running that ksqlDB is configured to use.

Select the docker-compose.yml file to install kafka and ksqlDB. 

2. Start ksqlDB's server 
From the directory containing the docker-compose.yml file 
command: docker-compose up 

3. Start ksqlDB's interactive CLI 
ksqlDB runs as a server which clients connects to in order to issue qureis. 

Run this command to connect to the ksqlDB server and enter an interactive command-line interface (CLI) session. 

docker exec -it ksqldb-cli ksql http://ksqldb-server:8088 
