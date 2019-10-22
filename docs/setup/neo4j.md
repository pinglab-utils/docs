# Setting Up Neo4j

The detail instruction for installing Neo4j(latest version) can be found at [official website of Neo4j](https://neo4j.com/docs/operations-manual/current/installation/linux/debian/). Here are the simple steps created the current time (2019 -Oct 15).

Before you install, make sure the correct version of java is installed in your device

### Install Neo4j

- **To install Neo4j Community Edition:**

```bash

sudo apt-get install neo4j=<version>

```

- **To install Neo4j Enterprise Edition:**

```bash

sudo apt-get install neo4j-enterprise=<version>

```

### Startting Stopping or Restarting Service

- **To start the neo4j service**:

```bash
sudo service neo4j start
```
Go to ```IP/localhost:7474``` to have neo4j browser.

- **To stop the neo4j service**:

```bash
sudo service neo4j start
```
- **To restart the neo4j service**:

```bash
sudo service neo4j start
```

### For Forgotten Password

1. Stop the neo4j service
2. remove the auth file inside ```/var/lib/neo4j/data/dbms``` 
3. restart the service
4. Follow ```server connect``` command to recreate password


### For remote access


1. Stop the neo4j service
2. Setup ```0.0.0.0``` instead of ```localhost``` in the configuration file
3. Start the neo4j service


### How to use new graph database

1. Stop the neo4j service
2. Find out ```neo4j.config``` file and repoint the new graph database.
3. Start the neo4j service









