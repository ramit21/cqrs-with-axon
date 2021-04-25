# cqrs-with-axon

CQRS = Command Query Request Segragation. CQRS is a software design pattern which suggests that applications should be divided into a "command" and "query" part. A Command is an intent with the required information to alter the state of the object. Query is used to read the state of the object. 

In your microservices design, both command and query would be seperate microservices, with their respective data stores. When a command is executed, write DB is updated, and the update gets stored as an immutable event to an event store. The query application would then read this event and update its read database. All read queries made to the query microservice would then return the data from the read database. You may even design the query api to read from materialised views built on top of DB tables being updated by the commands.

"Aggregate" is the prime building block for implementing CQRS. An Aggregate is an entity or group of entities that is always kept in a consistent state (within a single ACID transaction). The Aggregate Root is the entity within the aggregate that is responsible for maintaining this consistent state. 

"Axon" is a platform that consists of Axon Framework and Axon Server.

Axon Framework is a Java framework that is used to simplify the building of event driver microservices that are based on CQRS, event sourcing and Domain driven design.

Axon server can be used as an event bus.

Q. When to use CQRS, and when not to use it?
Ans. CQRS is a very helpful pattern when there is a huge difference in no. of write and read operations. This allows us to optimise/scale read and write sides independently. However, CQRS is an overkill for simple CRUD based applications.

----------

Steps:
1. Run mvn install on root project. Note the axon-spring-boot-starter dependency in respective pom files.
2. Create a docker network:
```
docker network create --attachable -d overlay myNet
```
3. Start axon server using docker:
```
docker run -d --name axon-server -p 8024:8024 -p 8124:8124 --network myNet --restart always axionq/axonserver:latest
```
4. Access Axon server UI:
```
http://localhost:8024
```
5. Run mongodb docker, which will server as store for events:
```
docker run -it -d --name mongo-container -p 27017:27017 --network myNet --restart always -v mongodb_data_container:/data/db mongo:latest
```


About the project:

core.lib is a library that has common events that would be used by both command and query modules.
