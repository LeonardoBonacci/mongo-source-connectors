# Outbox Pattern with MongoDB

```shell
# Start the topology as defined in https://debezium.io/documentation/reference/stable/tutorial.html
export DEBEZIUM_VERSION=1.8
docker-compose up --build

# Start MongoDB connector
curl -i -X POST -H "Accept:application/json" -H  "Content-Type:application/json" http://localhost:8083/connectors/ -d @register-mongodb.json

# Initialize MongoDB replica set and insert some test data
docker-compose exec mongodb bash -c '/usr/local/bin/init-inventory.sh'

# Insert a new order and outbox event with multi-document transaction in the database via MongoDB client
docker-compose exec mongodb bash -c 'mongo -u $MONGODB_USER -p $MONGODB_PASSWORD --authenticationDatabase admin inventory'

new_order = { "_id" : ObjectId("000000000000000000000001"), "purchaser_id" : NumberLong(1004), "quantity" : 1, "product_id" : NumberLong(107) }
s = db.getMongo().startSession()
s.getDatabase("inventory").outboxevent.insert({ aggregateid : new_order._id, aggregatetype : "Order", payload : new_order })

# Consume messages from the outbox event topic
docker run --tty --rm \
    --network mongo-outbox-network \
    quay.io/debezium/tooling:1.2 \
    kafkacat -b kafka:9092 -C -o beginning -q \
    -f "{\"key\":%k, \"headers\":\"%h\"}\n%s\n" \
    -t Order.events

# Shut down the cluster
docker-compose down
```

```
docker-compose exec connect curl -X GET http://connect:8083/connectors/outbox-connector/status
docker-compose exec connect curl -X DELETE http://connect:8083/connectors/outbox-connector
```
