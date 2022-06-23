# mongo-source-connectors

## MongoDB Source connectors
* https://www.mongodb.com/docs/kafka-connector/current/source-connector/configuration-properties/change-stream/#std-label-source-configuration-change-stream
* https://debezium.io/documentation/reference/stable/connectors/mongodb.html
* https://www.confluent.io/hub/microsoftcorporation/kafka-connect-cosmos (Cosmos SQL API / Mongo API?)

## Environments
* Local
* Atlas
* CosmosDB

## Hosting
* Confluent Cloud
* Confluent self-hosted

## References
* https://docs.microsoft.com/en-us/azure/cosmos-db/mongodb/change-streams?tabs=csharp

```
curl -s -X PUT -H "Content-Type:application/json" \
   http://localhost:8083/admin/loggers/com.mongodb \
   -d '{"level": "DEBUG"}' \
   | jq '.'


curl -s -X PUT -H "Content-Type:application/json" \
  http://localhost:8083/admin/loggers/org.apache.kafka.connect \
  -d '{"level": "DEBUG"}' \
  | jq '.'

curl -s -X PUT -H "Content-Type:application/json" \
  http://localhost:8083/admin/loggers/org.apache.kafka \
  -d '{"level": "INFO"}' \
  | jq '.'  
```   
