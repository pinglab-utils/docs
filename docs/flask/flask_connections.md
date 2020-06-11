# Connecting flask to neo4j, mongoDB, and elasticSearch

### Overview

Because our application's requirement's involve us using a heterogenous mix of data sources we needed a way to bring all of these data sources together quickly and easily. To do this we chose flask, a python webframework that allows us to efficiently manage the complexity involved with dealing with all three databases. The general problem is this: repeatedly connecting to each database is not only relatively computationally expensive over time but interacting with each database by itself is not necessarily intuitive, as each database has its own somewhat similar but ultimately different query syntax. To work around this we created three distinct python modules that create a consistent connection to each database as a python variable, create high level functionality based on python functions that take these variables as arguments and then finally mix and match these functions to handle various tasks. 

The general pattern for each module is as follows:

    database_utils/
        __init__.py
        database_funcs.py
        constants.py
        
where \_\_init\_\_.py specifies what is available for a program importing database_utils, constants.py has the connection to the specified connection and any other relevant constants, and database_funcs.py has all of the function implmentations.

Below is an example of this pattern with part of our neo4j_utils module.

### neo4j