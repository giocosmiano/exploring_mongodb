- [MongoDB - Cross-platform NoSQL/Document-Oriented DB](https://docs.mongodb.com/manual/)
  - [Documentation](https://docs.mongodb.com/)
  - [Introduction](https://docs.mongodb.com/manual/introduction/)
  - [How to Install MongoDB on Ubuntu 20.04](https://www.digitalocean.com/community/tutorials/how-to-install-mongodb-on-ubuntu-20-04)
  - [How to Install and Configure MongoDB on Ubuntu 20.04 LTS](https://www.howtoforge.com/tutorial/install-mongodb-on-ubuntu/)

- Installing `mongodb` in `Ubuntu 20.04 x64`
  - [Download MongoDB Community Server](https://www.mongodb.com/download-center/community)
    - [Click the `Download` button for installation package - mongodb-org-server_4.4.0_amd64.deb](https://www.mongodb.com/download-center/community/releases)
    - [Download the latest binary](https://www.mongodb.org/dl/linux/x86_64-ubuntu2004)
  - [Install MongoDB Community Edition on Ubuntu](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/)

```bash
   $ sudo dpkg -i mongodb-org-server_4.4.0_amd64.deb
```

- As personal preference, add these settings to `.bashrc` mongodb-status, mongodb-start, mongodb-stop, mongodb-restart

```bash
   export APPLICATIONS_HOME="${HOME}/Documents/_applications"
   export MONGODB_HOME="${APPLICATIONS_HOME}/mongodb-linux-x86_64-ubuntu1804-4.2.0"
   export MONGODB_BIN="${MONGODB_HOME}/bin"

   alias mongo-restart='sudo systemctl restart mongodb'
   alias mongo-start='sudo systemctl start mongodb'
   alias mongo-stop='sudo systemctl stop mongodb'
   alias mongo-status='sudo systemctl status mongodb'
   alias mongo-enable='sudo systemctl enable mongodb'
   alias mongo-disable='sudo systemctl disable mongodb'
```

- Configure to start `mongodb` automatically with the server

```bash
   $ sudo cp mongodb.service /etc/systemd/system
```

- Update the `systemd` service after copying `mongodb` services 

```bash
   $ sudo systemctl daemon-reload
```

- Enable to auto-start `mongodb` service

```bash
   $ mongo-enable
```

- Start/Re-Start `mongodb` service

```bash
   $ mongo-start
   $ mongo-restart
```

- Check `mongodb` version

```bash
   $ mongod --version
```

```sql
   db.version()
```

- Issue installing MongoDB in `Ubuntu 18.04`
  - [MongoDB Zip Installation Failed in Ubuntu 18.04](https://stackoverflow.com/questions/51006934/mongodb-zip-installation-failed-in-ubuntu-18-04)

- Installing `mongodb` UI
  - [dbKoda](https://www.dbkoda.com/)
  - [NoSQLBooster for MongoDB](https://nosqlbooster.com/downloads)
  - [MongoDB Compass](https://www.mongodb.com/download-center/compass)
    - [Click the `Download` button for installation package - mongodb-compass_1.19.6_amd64.deb](https://www.mongodb.com/download-center/compass)

- Sample MongoDB data sets
  - [MongoDB Atlas Sample Datasets](https://docs.atlas.mongodb.com/sample-data/available-sample-datasets/)
    - [Sample AirBnB Listings Dataset](https://docs.atlas.mongodb.com/sample-data/sample-airbnb/#sample-airbnb)
  - [MongoDB sample Collections](https://medium.com/dbkoda/mongodb-sample-collections-52d6a7745908)
  - [Sample collections for MongoDB](https://github.com/SouthbankSoftware/dbkoda-data)
  - [MongoDB json files](https://github.com/ozlerhakan/mongodb-json-files)

## Connecting Kafka to MongoDB Source
  - [How to connect Kafka to MongoDB Source](https://medium.com/tech-that-works/cloud-kafka-connector-for-mongodb-source-8b525b779772)
    - [Setting MongoDB Replica Set](https://www.youtube.com/watch?v=I6J9M0J66jo)
    - [MongoDB Kafka Connect Tutorial | Apache Kafka](https://www.youtube.com/watch?v=AF9WyW4npwY)
  - [Topic name must be in the form `logicalName.databaseName.collectionName`](https://debezium.io/documentation/reference/1.2/connectors/mongodb.html#mongodb-topic-names)
    - [Kafka Debezium Connector for MongoDB](https://debezium.io/documentation/reference/1.2/connectors/mongodb.html)
    - So in essence, create the topic name with `mongoConn.sampleGioDB.books`
      - mongodb.name = mongoConn (see connect-mongodb-source.properties)
      - db name is `sampleGioDB` (in Mongo)
      - collection name is `books` (in Mongo)
```bash
  $ kafka-topics --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic mongoConn.sampleGioDB.books
  $ kafka-topics --list --zookeeper localhost:2181
  $ cd $CONFLUENT_HOME
  $ bin/connect-standalone etc/schema-registry/connect-avro-standalone.properties etc/kafka/connect-mongodb-source.properties
  $ kafka-console-consumer --bootstrap-server localhost:9092 --topic mongoConn.sampleGioDB.books  --from-beginning
```

## Setting up Mongo to run in replicated instances (replica set)
  - copy the replicated mongo configurations/services 
```bash
   $ sudo cp replicated_mongodb*.conf /etc
   $ sudo cp replicated_mongodb*.service /etc/systemd/system
   $ sudo systemctl daemon-reload
```
  - create DB directories (see replicated_mongodb${n}.conf)
```bash
   $ mkdir -p $HOME/Documents/_mongoDBData/db1
   $ mkdir -p $HOME/Documents/_mongoDBData/db2
   $ mkdir -p $HOME/Documents/_mongoDBData/db3
```
  - As personal preference, add these settings to `.bashrc` mongodb-status, mongodb-start, mongodb-stop, mongodb-restart
```bash
alias mongodb-restart='sudo systemctl restart replicated_mongodb1.service replicated_mongodb2.service replicated_mongodb3.service'
alias mongodb-start='sudo systemctl start replicated_mongodb1.service replicated_mongodb2.service replicated_mongodb3.service'
alias mongodb-stop='sudo systemctl stop replicated_mongodb1.service replicated_mongodb2.service replicated_mongodb3.service'
alias mongodb-status='sudo systemctl status replicated_mongodb1.service replicated_mongodb2.service replicated_mongodb3.service'
alias mongodb-enable='sudo systemctl enable replicated_mongodb1.service replicated_mongodb2.service replicated_mongodb3.service'
alias mongodb-disable='sudo systemctl disable replicated_mongodb1.service replicated_mongodb2.service replicated_mongodb3.service'
```
  - Open 3 terminal tabs and run each command to get the host names that will be used for primary/secondary nodes
```bash
   $ mongo --port 27017  # open in terminal tab 1
   $ mongo --port 27018  # open in terminal tab 2
   $ mongo --port 27019  # open in terminal tab 3
```
```bash
   > db.serverStatus()  // find the `host` of the node from each `mongo --port 2701n`
   > db.help()
```
  - Setup the replicated instances from master node
```bash
   $ mongo --port 27017  # this is going to be the primary node
```
```bash
   > rs.status()
   > rs.initiate()
   > rs.add("127.0.0.1:27018")  // OR rs.add("gio-Satellite-P70-A:27018") -- based on the host from db.serverStatus()
   > rs.add("127.0.0.1:27019")  // OR rs.add("gio-Satellite-P70-A:27019") -- based on the host from db.serverStatus()
   > rs.status()
```

## Setting up Mongo to run in single instance
  - copy the replicated mongo configuration/service 
```bash
   $ sudo cp replicated_mongodb1.conf /etc
   $ sudo cp replicated_mongodb1.service /etc/systemd/system
   $ sudo systemctl daemon-reload
```
  - create DB directories (see replicated_mongodb1.conf)
```bash
   $ mkdir -p $HOME/Documents/_mongoDBData/db1
```
  - As personal preference, add these settings to `.bashrc` mongodb-status, mongodb-start, mongodb-stop, mongodb-restart
```bash
alias mongodb-restart='sudo systemctl restart replicated_mongodb1.service'
alias mongodb-start='sudo systemctl start replicated_mongodb1.service'
alias mongodb-stop='sudo systemctl stop replicated_mongodb1.service'
alias mongodb-status='sudo systemctl status replicated_mongodb1.service'
alias mongodb-enable='sudo systemctl enable replicated_mongodb1.service'
alias mongodb-disable='sudo systemctl disable replicated_mongodb1.service'
```
    