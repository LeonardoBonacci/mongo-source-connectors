# Debezium Outbox Pattern with MongoDB

```shell
# Start the topology as defined in https://debezium.io/documentation/reference/stable/tutorial.html
export DEBEZIUM_VERSION=1.8
docker-compose up -d

curl -i -X POST -H "Accept:application/json" -H  "Content-Type:application/json" http://localhost:8083/connectors/ -d @register-mongodb.json

docker-compose exec connect curl -X GET http://connect:8083/connectors/outbox-connector/status

{
    "_id": {
        "$oid": "62b1279b6395bffbd7b3138e"
    },
    "aggregatetype" : "Foo",
    "payload": {
      "hello": "wr"
    }
}

# Consume messages from the outbox event topic
docker run --tty --rm \
    --network mongo-outbox-network \
    quay.io/debezium/tooling:1.2 \
    kafkacat -b kafka:9092 -L

# Shut down
docker-compose down
docker-compose exec connect curl -X DELETE http://connect:8083/connectors/outbox-connector
```

```
... %  docker run --tty --rm \
    --network mongo-outbox-network \
    quay.io/debezium/tooling:1.2 \
    kafkacat -b kafka:9092 -C -o beginning -q \
    -f "{\"key\":%k, \"headers\":\"%h\"}\n%s\n" \
    -t Foo.events
{"key":, "headers":"id=62b1279b6395bffbd7b3138e,uber-trace-id=2b4cc768e8340e41:2b4cc768e8340e41:0:1"}
Struct{hello=wr}
{"key":, "headers":"id=62b1289d6395bffbd7b3138f,uber-trace-id=7061d73072465777:7061d73072465777:0:1"}
Struct{hello=sssswr}
```
