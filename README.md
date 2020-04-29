

A demo packaged using docker-compose, which uses Confluent Cloud, kSQLDB and InfluxDB.

It is docker-composed Postgres, kSQLDB, influxDB, Connect talking to CC and CCSR.
Source Connector from Postgres; then a bunch of kSQLDB streams and tables to count, join and aggregate, and then two influxDB Sinks of the table contents.
The next step will be a Chronograf connected to InfluxDB that springs into life when data starts streaming.
A getNews script pumps in lots of yesterdays news stories.

I want to figure out how to apply e2e encryption to tokenize the "country" attribute in two postgres tables; then stream them untokenized and tokenized. The influxDB counts should be the same for both streams.

The other project I'm thinking of is how to add a kSQLDB UDF that will geobox the lat/long of the story to the nearest city (or any geographical function return. CountryName; GeoBox; whatever). This is to prove that we can still use UDFs on kSQLDB for CC systems. We cannot use UDFs for CC-kSQL.

all objects are prefixed by your confluent username so it all creates/drops cleanly without stomping on each other

The runme & cleanup scripts will login to Confluent Cloud each time; to avoid irritating timeout errors. 
Before you run it:
in your .profile set 
export CC_USERNAME to the email address that you use to login to CC
CC_PASSWORD to your CC secret
CC_CLUSTER to the name of the CC cluster to use. You can use GlobeDev.
For example:
export CC_USERNAME="teehan@confluent.io"
export CC_PASSWORD="X@1zge........MC"
export CC_CLUSTER="GlobeDev"

Scripts do things:
cleanup: list and then drop all topics that contain your username. 
  runme: run the build interactively, with prompts and pauses
getNews: a shell script to wget and postgres load CSV files from data.gdelt.org for yesterdays news. Control-C at any time.


First Run:
- start docker with 8GB
- in one tab do a docker-compose up. After container downloads it takes ~60 seconds for Kafka Connect to start
- in another tab execute runme. It will prompt between commands. If anything errors out, control-C it.

Reruns:
- CTRL-C the docker tab and wait for all containers to exit
- docker system prune to clear out Kafka Connect jobs etc. This step is required as "runme" doesnt do drops.
- cleanup to clear out the topics
- as in First Run.

Viewing the Results:
- runme will query influxDB to check if data is streaming back down from CC. 
- Open Chronograf on http://localhost:8888 and import dashboard NewsActivity.json to view the streaming pipeline





Subsequent Runs:
- 









