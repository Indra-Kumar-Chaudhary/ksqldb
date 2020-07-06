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

4. Create a stream 
    The first thing we're going to do is create a stream. A stream essentially associates a schema with a underlying Kafka topic. Here's what each parameter in the <b> CREATE STREAM statement does: </b>

    CREATE STREAM riderLocation (profileID VARCHAR,latitude DOUBLE, longitude DOUBLE)
        WITH (kafka_topic='location', value_format='json', partitions=1); 

    <ul>
        <li><b>kafka_topic </b></li>
    <ul>