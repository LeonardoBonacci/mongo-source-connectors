# Slow Start

Cosmos SQL style -> works

```
curl -i -X POST -H "Accept:application/json" -H  "Content-Type:application/json" http://localhost:8083/connectors/ -d @cosmosdb-source-connector.json
curl -X GET http://localhost:8083/connectors
curl -X GET http://localhost:8083/connectors/cosmosdb-source-connector/status
curl -X DELETE http://localhost:8083/connectors/cosmosdb-source-connector
```     
