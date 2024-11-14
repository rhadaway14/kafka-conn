# kafka-connNOTE:
IF USING GIT BASH ON WINDOWS, YOU MAY NEED TO RUN FROM POWERSHELL TO AVOID ANY PATH ISSUES


START
docker-compose up -d


CONFIGURE TOPICS
docker exec -it to-capella-working-kafka-1 kafka-topics --bootstrap-server localhost:9092 --create --topic couchbase-sink-topic --partitions 1 --replication-factor 1


CONFIG COUCHBASE
user: Administrator
password: Password1!
bucket: test
scope: _default
collection: _default
whitelist: local IP


UPDATE couchbase-sink.json
Add the capella connection string to couchbase.seed.nodes


UPDATE capella.pem
Copy capella cert into capella.pem


DEPLOY CONNECTORS
curl -X POST -H "Content-Type: application/json" --data @./configs/couchbase-sink.json http://localhost:8083/connectors


CHECK STATUS
curl -X GET http://localhost:8083/connectors/sink-couchbase/status


TEST 
docker exec -it to-capella-working-kafka-1 kafka-console-producer --bootstrap-server localhost:9092 --topic couchbase-sink-topic
{"id": "101", "name": "Test1", "description": "This is a test message for Couchbase."}
{"id": "102", "name": "Test2", "description": "This is a test message for Couchbase."}
{"id": "103", "name": "Test3", "description": "This is a test message for Couchbase."}

CONFLUENT UI
http://localhost:9021
