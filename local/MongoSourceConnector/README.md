# Quick Start

This directory contains files you need to build the MongoDB Kafka Connector Quick Start
for v1.7 of the connector.

[You can view the quick start here](https://www.mongodb.com/docs/kafka-connector/v1.7/quick-start/).

```
docker exec -it shell /bin/bash

curl -X POST \
     -H "Content-Type: application/json" \
     --data '
     {"name": "mongo-source",
      "config": {
         "connector.class":"com.mongodb.kafka.connect.MongoSourceConnector",
         "key.converter":"org.apache.kafka.connect.json.JsonConverter",
         "key.converter.schemas.enable": false,
         "value.converter":"org.apache.kafka.connect.json.JsonConverter",
         "value.converter.schemas.enable": false,
         "connection.uri":"mongodb://mongo1:27017/?replicaSet=rs0",
         "database":"quickstart",
         "collection":"sampleData",
         "publish.full.document.only": true,
         "output.format.key": "schema",
         "output.schema.key": "{ \"type\": \"record\", \"name\": \"keySchema\", \"fields\" : [ { \"name\": \"documentKey\", \"type\": [\"string\", \"null\"] } ] }"
         }
     }
     ' \
     http://connect:8083/connectors -w "\n"

mongosh mongodb://mongo1:27017/?replicaSet=rs0
use quickstart
db.sampleData.insertOne({"hello":"world"})

.../bin % ./kafka-console-consumer \
  --bootstrap-server localhost:9092 \
  --topic quickstart.sampleData \
  --property print.key=true \
  --value-deserializer org.apache.kafka.connect.json.JsonDeserializer

 "{\"_id\": {\"$oid\": \"62b017a62966a1a6ba46a4f6\"}, \"hello\": \"world\"}"

curl -X GET http://connect:8083/connectors/mongo-source/status
curl -X DELETE http://connect:8083/connectors/mongo-source
```     
