# Slow Start

```
curl -i -X POST -H "Accept:application/json" -H  "Content-Type:application/json" http://localhost:8083/connectors/ -d @source-connector.json
curl -X GET http://localhost:8083/connectors
curl -X GET http://localhost:8083/connectors/cosmos-mongo-source/status
curl -X DELETE http://localhost:8083/connectors/cosmos-mongo-source
```     

```
... bin % ./kafka-console-consumer \
  --bootstrap-server localhost:9092 \
  --topic foo.bar \
  --property print.key=true
"{\"_id\": {\"_data\": {\"$binary\": \"eyJWIjoyLCJSaWQiOiI1ZTRuQUtoRUlUOD0iLCJDb250aW51YXRpb24iOlt7IkZlZWRSYW5nZSI6eyJ0eXBlIjoiRWZmZWN0aXZlIFBhcnRpdGlvbiBLZXkgUmFuZ2UiLCJ2YWx1ZSI6eyJtaW4iOiIiLCJtYXgiOiJGRiJ9fSwiU3RhdGUiOnsidHlwZSI6ImNvbnRpbnVhdGlvbiIsInZhbHVlIjoiXCIzM1wiIn19XX0=\", \"$type\": \"00\"}, \"_kind\": 1}}"	"{\"_id\": \"7d8d8f3c-2ebb-40e1-8ef2-91dfa46d091d\", \"name\": \"whatever..\", \"_class\": \"guru.bonacci.kafkamongo.domain.Bar\"}"
"{\"_id\": {\"_data\": {\"$binary\": \"eyJWIjoyLCJSaWQiOiI1ZTRuQUtoRUlUOD0iLCJDb250aW51YXRpb24iOlt7IkZlZWRSYW5nZSI6eyJ0eXBlIjoiRWZmZWN0aXZlIFBhcnRpdGlvbiBLZXkgUmFuZ2UiLCJ2YWx1ZSI6eyJtaW4iOiIiLCJtYXgiOiJGRiJ9fSwiU3RhdGUiOnsidHlwZSI6ImNvbnRpbnVhdGlvbiIsInZhbHVlIjoiXCIzM1wiIn19XX0=\", \"$type\": \"00\"}, \"_kind\": 1}}"	"{\"_id\": \"fe38a967-2765-44c2-aa02-bdebc121e548\", \"name\": \"whatever..\", \"_class\": \"guru.bonacci.kafkamongo.domain.Bar\"}"
```  
