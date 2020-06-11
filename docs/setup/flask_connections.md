# Connecting flask to neo4j, mongoDB, and elasticSearch

### Overview

Because our application's requirement's involve us using a heterogenous mix of data sources we needed a way to bring all of these data sources together quickly and easily. To do this we chose flask, a python webframework that allows us to efficiently manage the complexity involved with dealing with all three databases. The general problem is this: repeatedly connecting to each database is not only relatively computationally expensive over time but interacting with each database by itself is not necessarily intuitive, as each database has its own somewhat similar but ultimately different query syntax. To work around this we created three distinct python modules that create a consistent connection to each database as a python variable, create high level functionality based on python functions that take these variables as arguments and then finally mix and match these functions to handle various tasks. 

The general pattern for each module is as follows:

    database_utils/
        __init__.py
        database_funcs.py
        constants.py
        
where __\_\_init\_\_.py__ specifies what is available for a program importing database_utils, __constants.py__ has the connection to the specified database and any other relevant constants, and __database_funcs.py__ has all of the function implmentations.

Below is an example of this pattern with part of our neo4j_utils module.

### neo4j

The following is the structure of our neo4j module:

    neo4j_utils/
        __init__.py
        neo4j_funcs.py
        constants.py

Below are code snippets from each file with an accompanying explanation of each snippet.

__\_\_init\_\_.py__

```python3
from .graph_funcs import mms_neighborhood
  
from .constants import NEO4J_DRIVER

```

This file specifies that any program importing neo4j_utils has access to a connection to our neo4j database named NEO4J_DRIVER and a function mms_neighborhood, a function that returns the neighborhood of an ICD11 MMS code up to a distance specified.

__graph_funcs.py__
```python3
from .constants import NEIGHBORHOOD_QUERY

def mms_neighborhood(mms_id, distance, driver):
    """Returns all of the node ids at or below the distance specified."""
    query = NEIGHBORHOOD_QUERY.format(mms_id=mms_id, distance=distance)
    with driver.session() as sesh:
        results = sesh.run(query)
        ids = [result['adj_node']['id'] for result in results]
        return ids

```

This file is a python program that imports a python object named NEIGHBORHOOD_QUERY from the constants.py file and implements a function named mms_neighborhood. mms_neighborhood takes three arguments: an mms_id(as a string), distance (as an integer or suitable numeric type), and a connection to a neo4j database. With the specified mms_id and distance we create a query string that our connection can then send and run.

__constants.py__
```python3
#All of the constants used for Neo4j related functions
  
from neo4j import GraphDatabase

NEO4J_DRIVER = GraphDatabase.driver(uri="bolt://localhost:7687",
                                    auth=("neo4j","icd11"))


NEIGHBORHOOD_QUERY = """MATCH 
(node:icd11_code)-[:child_of*0..{distance}]-(adj_node:icd11_code)
WHERE node.id = '{mms_id}'
RETURN adj_node
ORDER BY adj_node.height"""

```

This file is a python program that creates a neo4j connection and the query for finding a neighborhood of a node.