# Debezium Outbox Pattern with MongoDB

```shell
# Start the topology as defined in https://debezium.io/documentation/reference/stable/tutorial.html
export DEBEZIUM_VERSION=1.8
docker-compose up --build

docker-compose exec mongodb bash -c '/usr/local/bin/init-db.sh'

curl -i -X POST -H "Accept:application/json" -H  "Content-Type:application/json" http://localhost:8083/connectors/ -d @register-mongodb.json
docker-compose exec connect curl -X GET http://connect:8083/connectors/outbox-connector/status


docker-compose exec mongodb bash -c 'mongo -u $MONGODB_USER -p $MONGODB_PASSWORD --authenticationDatabase admin quickstart'
s = db.getMongo().startSession()
new_data = { "_id" : ObjectId("000000000000000000000001"), "hello" : "world" }
s.getDatabase("quickstart").sampleData.insert({ aggregateid : new_data._id, aggregatetype : "Foo", payload : new_data })
s.getDatabase("quickstart").sampleData.find()

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
... % docker run --tty --rm \
    --network mongo-outbox-network \
    quay.io/debezium/tooling:1.2 \
    kafkacat -b kafka:9092 -C -o beginning -q \
    -f "{\"key\":%k, \"headers\":\"%h\"}\n%s\n" \
    -t Foo.events
{"key":000000000000000000000001, "headers":"id=62b0c423b70eec8c787fb66a,uber-trace-id=9ab2937eeb807d11:9ab2937eeb807d11:0:1"}
Struct{_id=000000000000000000000001,hello=world}
```

```
https://github.com/debezium/docker-images/tree/main/examples/mongodb/1.8
docker build -t quickstart-mongo .
```
