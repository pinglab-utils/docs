##  Database

After setting up mongoDB database and once parsed data is ready, mongoDB database can be populated with pared data and mapping table

- Install pymongo in the current environment

```bash
pip install pymongo
```

- Import pymongo to current environment

```python
import pymongo
```

- Call client for mongoDB database

```python
client = pymongo.MongoClient("mongodb://localhost:27017/")
```
#### Create new database

- Delete previously created database 

```python
client.drop_database('icd11')
```

- Create a new database with new name
```python
database = client["NAME"]
```

#### Add Collection in the new database

- Create new collection inside the database

```
collection = database["COLLECTION_NAME"]
```

- Populating collection with parsed data

```python
x = collection.insert_many(Data)

```

- Test a sample out of populated data

```python
x = collection.find_one()
print(x)
```
#### Add mapping Tables as new collections

- Create seperate collection within the database for seperate mapping table

```python
mapping_collection = db['MAPPING_NAME']

```
- Populate the collection with mapping table

```python

x = mapping_collection.insert_many(MAPPING_TABLE)

```











