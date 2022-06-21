# Slow Start

```
curl -i -X POST -H "Accept:application/json" -H  "Content-Type:application/json" http://localhost:8083/connectors/ -d @source-connector.json
curl -X GET http://localhost:8083/connectors/mongo-source/status
curl -X DELETE http://localhost:8083/connectors/mongo-source
```     
