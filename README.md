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
        <li><b>kafka_topic </b> - Name of the kafka topic underlying the stream. In this case it will be automatically created because it doen't exists yet, but stream may also be created over topics that alrealy exist.</li>
        <li><b>value_format -</b> Encoding of the message in the Kafka topic. For JSON encoding, each row will be stored as JSON object whose keys/values are column names/values.
        For example: <b> {"profileId":"c2309eec",
        "latitude":37.7877,"longitude": -122.4205}
        </b>
        </li>
        <li><b>partitions</b>- Number of partitions to create for the locations topic. Note that this parameter is not needed for topics that already exists.
        
        <li>
    <ul>
    5. Run a continuous query over the stream 
        Run the query using your interactive CLI session. 
        
        <b> -- Mountain View lat, long: 37.4133, -122.1162
        SELECT * FROM riderLocations
        WHERE GEO_DISTANCE(latitude, longitude, 37.4133, -122.1162) <= 5 EMIT CHANGES;
        </b>

        This query will output all rows from the <b>riderLocations</b> stream whose coordinates are within 5 miles of Mountain View. 

    6. <b>Start another CLI session</b> 
        Since the CLIsession from  (5) is busy waiting for output from the continuous query, let's start another session that we can use to write some data into ksqlDB.

        <b>docker exec -it ksqldb-cli ksql http://ksqldb-server:8088 

    
    INSERT INTO riderLocations (profileId, latitude, longitude) VALUES ('c2309eec', 37.7877, -122.4205);
INSERT INTO riderLocations (profileId, latitude, longitude) VALUES ('18f4ea86', 37.3903, -122.0643);
INSERT INTO riderLocations (profileId, latitude, longitude) VALUES ('4ab5cbad', 37.3952, -122.0813);
INSERT INTO riderLocations (profileId, latitude, longitude) VALUES ('8b6eae59', 37.3944, -122.0813);
INSERT INTO riderLocations (profileId, latitude, longitude) VALUES ('4a7c7b41', 37.4049, -122.0822);
INSERT INTO riderLocations (profileId, latitude, longitude) VALUES ('4ddad000', 37.7857, -122.4011); 

7. <b> Populate the stream with events </b>
Run the each of given INSERT statement within the new CLI session, and  keep an eye on he CLI session from (5) as you do. 

The continuous query will output matching rows in real time as soon as they're written to the riderLocations strea. 
